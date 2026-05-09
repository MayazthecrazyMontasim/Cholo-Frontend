# Cholo SAAS Readiness Plan

## Current State Analysis
- **Frontend**: Static HTML + Tailwind (Bangladesh trip planner)
- **Backend**: FastAPI with SQLite, JWT auth, single-user model
- **Deployment**: Vercel (frontend) + Render (backend)
- **Architecture**: Single-instance, monolithic

---

## SAAS Transformation Strategy

### Phase 1: Multi-Tenancy Architecture (Critical)

#### 1.1 Database Schema Changes
```sql
-- New tables for multi-tenancy

CREATE TABLE organizations (
    id BIGINT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    slug VARCHAR(100) UNIQUE NOT NULL,  -- For subdomain/custom domain
    domain VARCHAR(255),
    branding_config JSON,  -- Logo, colors, custom name
    created_at TIMESTAMP,
    owner_id BIGINT
);

CREATE TABLE workspaces (
    id BIGINT PRIMARY KEY,
    organization_id BIGINT NOT NULL,
    name VARCHAR(255),
    is_default BOOLEAN,
    created_at TIMESTAMP,
    FOREIGN KEY(organization_id) REFERENCES organizations(id)
);

-- Modify existing User table
ALTER TABLE user ADD COLUMN (
    organization_id BIGINT,
    workspace_id BIGINT,
    role VARCHAR(20) DEFAULT 'member'  -- admin, editor, viewer
);

-- Tenant-scoped tables
-- Apply organization_id + workspace_id filters to all trip/destination/budget data
```

#### 1.2 Multi-Tenant Middleware
```python
# Backend: Extract tenant from header/subdomain
@app.middleware("http")
async def tenant_middleware(request: Request, call_next):
    # Extract tenant from:
    # 1. Custom header: X-Organization-ID
    # 2. Subdomain: {org}.cholo.app
    # 3. Auth token claims
    
    request.state.tenant_id = extracted_tenant_id
    request.state.workspace_id = extracted_workspace_id
    response = await call_next(request)
    return response
```

---

### Phase 2: Authentication & Authorization

#### 2.1 Multi-Organization Auth Flow
```python
# auth.py enhancements

class UserCreate(BaseModel):
    email: str
    password: str
    full_name: str
    organization_name: str  # Auto-create org on signup

@router.post("/register")
async def register(user_data: UserCreate):
    # 1. Create organization (if new)
    # 2. Create user with organization_id
    # 3. Assign default workspace
    # 4. Grant admin role in workspace
```

#### 2.2 Role-Based Access Control (RBAC)
```python
# Roles per workspace
ROLES = {
    "admin": ["read", "write", "delete", "invite", "settings"],
    "editor": ["read", "write"],
    "viewer": ["read"],
    "guide": ["read", "write", "custom_tours"]  # For tour guides
}
```

#### 2.3 JWT Claims Enhancement
```json
{
  "sub": "user_id",
  "org_id": "organization_id",
  "workspace_id": "workspace_id",
  "role": "admin",
  "permissions": ["read", "write"]
}
```

---

### Phase 3: Frontend Architecture

#### 3.1 Organization Switcher
```html
<!-- New: Multi-org selector in navbar -->
<select id="orgSelector">
  <option value="org_1">My Travel Agency</option>
  <option value="org_2">Family Adventures</option>
</select>

<!-- Workspace selector -->
<select id="workspaceSelector">
  <option value="ws_1">Bangladesh Tours 2026</option>
  <option value="ws_2">SE Asia Portfolio</option>
</select>
```

#### 3.2 Tenant-Specific Branding
```javascript
// Load org branding config
async function loadOrgBranding(orgId) {
  const org = await fetch(`/api/orgs/${orgId}`);
  const branding = org.branding_config;
  
  document.documentElement.style.setProperty('--primary-color', branding.primary_color);
  document.title = branding.app_name || 'Cholo';
}
```

#### 3.3 API Client Enhancement
```javascript
// All API calls include tenant context
const API_CLIENT = {
  baseURL: getApiUrl(),
  headers: {
    'X-Organization-ID': getCurrentOrgId(),
    'X-Workspace-ID': getCurrentWorkspaceId()
  }
};
```

---

### Phase 4: Data Isolation & Security

#### 4.1 Query Filtering
Every database query must include tenant filters:
```python
# BAD
trips = db.query(Trip).all()

# GOOD
trips = db.query(Trip).filter(
    Trip.organization_id == request.state.tenant_id,
    Trip.workspace_id == request.state.workspace_id
).all()
```

#### 4.2 Row-Level Security (RLS)
If using PostgreSQL:
```sql
CREATE POLICY tenant_isolation ON trips
USING (organization_id = current_setting('app.current_org_id')::bigint);

ALTER TABLE trips ENABLE ROW LEVEL SECURITY;
```

---

### Phase 5: Subscription & Billing

#### 5.1 Database Models
```python
class SubscriptionPlan(BaseModel):
    id: str  # free, starter, pro, enterprise
    name: str
    price: float
    features: {
        max_workspaces: int,
        max_users: int,
        max_trips: int,
        custom_domain: bool,
        api_access: bool
    }

class OrganizationSubscription(BaseModel):
    organization_id: BIGINT
    plan_id: str
    stripe_subscription_id: str
    status: str  # active, past_due, canceled
    renews_at: datetime
```

#### 5.2 Feature Gating
```python
@app.post("/workspaces")
async def create_workspace(workspace: WorkspaceCreate, request: Request):
    org = db.get_organization(request.state.tenant_id)
    plan = org.subscription.plan
    
    current_count = db.count_workspaces(org.id)
    if current_count >= plan.features['max_workspaces']:
        raise HTTPException(status_code=403, detail="Workspace limit reached. Upgrade to pro.")
```

---

### Phase 6: API for Partners

#### 6.1 New Partner Endpoints
```
GET    /api/v1/orgs/{org_id}/dashboard      - Overview stats
GET    /api/v1/orgs/{org_id}/trips          - List org trips
POST   /api/v1/orgs/{org_id}/trips          - Create trip
GET    /api/v1/orgs/{org_id}/destinations   - Available destinations
POST   /api/v1/orgs/{org_id}/bookings       - Create booking
GET    /api/v1/orgs/{org_id}/analytics      - Usage analytics
```

#### 6.2 API Key Management
```python
class APIKey(BaseModel):
    organization_id: BIGINT
    key: str  # hashed
    name: str
    scopes: list[str]
    rate_limit: int
    created_at: datetime
    last_used: datetime
```

---

### Phase 7: Deployment Infrastructure

#### 7.1 Database Migration
```
Current: SQLite (single-instance)
Target: PostgreSQL (multi-tenant capable, RLS support)
```

**Migration Steps**:
1. Set up PostgreSQL (Supabase or AWS RDS)
2. Run schema migrations with tenant schema
3. Backfill existing user → organization mapping
4. Update connection string in FastAPI

#### 7.2 Deployment Topology
```
Frontend:
  - Vercel (supports custom domains for each tenant)
  - Environment: VITE_API_BASE_URL per build? Or dynamic lookup?

Backend:
  - Single API endpoint (tenant identified via header/JWT)
  - Render.com or AWS ECS
  - PostgreSQL database (isolated per tenant)
```

#### 7.3 Custom Domain Support
```
cholo.app                  → Marketing site
{org}.cholo.app           → Tenant subdomain
my-agency.cholo.app       → Custom subdomain
```

---

## Implementation Roadmap

### **Week 1: Foundation**
- [ ] Set up PostgreSQL database
- [ ] Design multi-tenant schema
- [ ] Add organization + workspace models
- [ ] Implement tenant middleware

### **Week 2: Auth Refactor**
- [ ] Extend User model with org/workspace
- [ ] Implement RBAC system
- [ ] Update JWT to include tenant claims
- [ ] Create org/workspace invitation flow

### **Week 3: Frontend Updates**
- [ ] Add org/workspace switcher UI
- [ ] Update API client for multi-tenant headers
- [ ] Implement tenant-specific branding
- [ ] Update auth flow for org context

### **Week 4: Data Isolation**
- [ ] Add tenant filters to all queries
- [ ] Implement RLS policies
- [ ] Test data isolation
- [ ] Audit API endpoints for tenant leaks

### **Week 5: Billing Integration**
- [ ] Add Stripe subscription models
- [ ] Implement feature gating
- [ ] Create subscription management UI
- [ ] Set up billing webhooks

### **Week 6: Partner API**
- [ ] Design and document partner API
- [ ] Implement API key management
- [ ] Add rate limiting
- [ ] Create API documentation (OpenAPI/Swagger)

### **Week 7-8: Testing & Deployment**
- [ ] Multi-tenant integration tests
- [ ] Security audit (data isolation, auth)
- [ ] Performance testing
- [ ] Gradual rollout to production

---

## Key Technical Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| **Database** | PostgreSQL (Supabase) | Native RLS, better for multi-tenant |
| **Tenant Identification** | JWT claims (sub: org_id) | Stateless, secure, scalable |
| **Subdomain Strategy** | {org}.cholo.app + custom domains | Common SAAS pattern, flexible |
| **Billing** | Stripe | Industry standard, webhooks, subscriptions |
| **Frontend Framework** | Keep current static + add context API | Low friction migration |

---

## Security Checklist

- [ ] **Data Isolation**: Every query includes tenant filter
- [ ] **Authentication**: Multi-org login, session per org
- [ ] **Authorization**: RBAC enforced at API level
- [ ] **Rate Limiting**: Per-org API quotas
- [ ] **Audit Logging**: Track who accessed what, when
- [ ] **API Security**: API key rotation, scopes
- [ ] **CORS**: Restrict to org domains only
- [ ] **Secrets Management**: Rotate Stripe keys, API secrets

---

## Estimated Effort

| Component | Days | Priority |
|-----------|------|----------|
| Database schema + migration | 3 | 🔴 Critical |
| Multi-tenant middleware | 2 | 🔴 Critical |
| Auth refactor (RBAC) | 4 | 🔴 Critical |
| Frontend org switcher | 3 | 🟡 High |
| Billing integration | 5 | 🟡 High |
| Partner API | 4 | 🟢 Medium |
| Testing + deployment | 5 | 🔴 Critical |
| **Total** | **~26 days** | - |

---

## Next Steps

1. **Approve SAAS strategy** - Subdomain vs. path-based tenancy?
2. **Choose database** - Supabase? AWS RDS? Self-hosted?
3. **Design org model** - Start with phase 1 schema
4. **Create migration plan** - How to handle existing data?
5. **Set up test environment** - Multi-tenant testing framework

