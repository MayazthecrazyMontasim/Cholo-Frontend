---
name: Cholo Travel
colors:
  surface: '#f5fbf5'
  surface-dim: '#d6dbd6'
  surface-bright: '#f5fbf5'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#eff5ef'
  surface-container: '#eaefea'
  surface-container-high: '#e4eae4'
  surface-container-highest: '#dee4de'
  on-surface: '#171d1a'
  on-surface-variant: '#3d4943'
  inverse-surface: '#2c322e'
  inverse-on-surface: '#ecf2ed'
  outline: '#6d7a73'
  outline-variant: '#bccac1'
  surface-tint: '#006c4e'
  primary: '#00694c'
  on-primary: '#ffffff'
  primary-container: '#008560'
  on-primary-container: '#f5fff7'
  inverse-primary: '#68dbae'
  secondary: '#885200'
  on-secondary: '#ffffff'
  secondary-container: '#fdad4e'
  on-secondary-container: '#704200'
  tertiary: '#565e5c'
  on-tertiary: '#ffffff'
  tertiary-container: '#6e7674'
  on-tertiary-container: '#f6fefb'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#86f8c9'
  primary-fixed-dim: '#68dbae'
  on-primary-fixed: '#002115'
  on-primary-fixed-variant: '#00513a'
  secondary-fixed: '#ffdcbb'
  secondary-fixed-dim: '#ffb869'
  on-secondary-fixed: '#2b1700'
  on-secondary-fixed-variant: '#673d00'
  tertiary-fixed: '#dce4e1'
  tertiary-fixed-dim: '#c0c8c6'
  on-tertiary-fixed: '#151d1c'
  on-tertiary-fixed-variant: '#404847'
  background: '#f5fbf5'
  on-background: '#171d1a'
  surface-variant: '#dee4de'
typography:
  display:
    fontFamily: Hind Siliguri
    fontSize: 32px
    fontWeight: '700'
    lineHeight: 40px
    letterSpacing: -0.02em
  h1:
    fontFamily: Hind Siliguri
    fontSize: 24px
    fontWeight: '600'
    lineHeight: 32px
    letterSpacing: -0.01em
  h2:
    fontFamily: Hind Siliguri
    fontSize: 20px
    fontWeight: '600'
    lineHeight: 28px
    letterSpacing: 0em
  body-lg:
    fontFamily: Hind Siliguri
    fontSize: 18px
    fontWeight: '400'
    lineHeight: 28px
    letterSpacing: 0em
  body-md:
    fontFamily: Hind Siliguri
    fontSize: 16px
    fontWeight: '400'
    lineHeight: 24px
    letterSpacing: 0em
  body-sm:
    fontFamily: Hind Siliguri
    fontSize: 14px
    fontWeight: '400'
    lineHeight: 20px
    letterSpacing: 0em
  label-md:
    fontFamily: Hind Siliguri
    fontSize: 12px
    fontWeight: '500'
    lineHeight: 16px
    letterSpacing: 0.05em
  button:
    fontFamily: Hind Siliguri
    fontSize: 16px
    fontWeight: '600'
    lineHeight: 24px
    letterSpacing: 0.01em
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  base: 4px
  xs: 4px
  sm: 8px
  md: 16px
  lg: 24px
  xl: 32px
  gutter: 16px
  margin: 20px
---

## Brand & Style

The brand personality for this design system is rooted in the concept of "Guided Discovery." It balances the excitement of travel with the reliability of a local companion. The target audience includes urban commuters, domestic tourists, and international travelers seeking authentic experiences. 

The aesthetic is **Modern/Corporate with Minimalist influences**, prioritizing clarity and ease of navigation. To evoke a sense of trust and safety, the UI avoids aggressive visual flair, opting instead for structural integrity, ample whitespace, and high legibility. The interface remains quiet and functional, allowing destination imagery to provide the emotional color while the UI provides the steady hand.

## Colors

The palette is anchored by a flagship Teal Green, chosen to represent growth, safety, and the natural landscape. The Amber accent is used sparingly to draw attention to critical actions, status indicators, or "premium" travel features without overwhelming the user. 

- **Primary (#1D9E75):** Used for main action buttons, active states, and branding elements.
- **Accent (#BA7517):** Reserved for highlights, ratings, and warnings.
- **Neutrals:** A range of cool greys (Slate) ensures that text contrast is high and the "white/light grey" surfaces feel crisp rather than muddy.
- **Semantic Colors:** Success is tied to the Primary Green; Error uses a soft red (#E11D48).

## Typography

This design system utilizes **Hind Siliguri** across all levels to ensure seamless bilingual support (Bangla and English). As a sans-serif, it provides the "modern and clean" look requested while maintaining high legibility at small sizes on mobile devices.

The typographic scale is optimized for mobile-first consumption. Headlines use a slightly heavier weight (600-700) to create a clear hierarchy against the 400-weight body text. Labels are set in a medium weight with slight letter spacing to differentiate them from interactive body text.

## Layout & Spacing

The layout philosophy follows a **Fluid Grid** model based on an 8px base unit. For mobile devices, the system utilizes a 4-column grid with 20px side margins to prevent content from touching the screen edges.

Vertical rhythm is strictly maintained through the 8px increments. Components are separated by 16px (md) or 24px (lg) depending on the degree of thematic separation. Internal padding for cards and containers should default to 16px to ensure touch targets remain accessible and content feels breathable.

## Elevation & Depth

To maintain the "clean and modern" style without heavy gradients, this design system uses **Low-contrast outlines** paired with **Ambient shadows**. 

- **Surfaces:** Most content sits on white containers (#FFFFFF) over a light grey background (#F8FAFC).
- **Outlines:** Use a subtle 1px border (#E2E8F0) for most cards and inputs to define boundaries without adding visual weight.
- **Shadows:** Use a single, soft elevation level for interactive cards. The shadow should be highly diffused: `0px 4px 12px rgba(0, 0, 0, 0.05)`. 
- **Z-Index Strategy:** Depth is used to communicate temporary states (e.g., Modals, Bottom Sheets) rather than decorative layering.

## Shapes

The design system adopts a **Rounded** (Level 2) shape language. This specific degree of corner radius (8px to 24px) strikes a balance between professional geometry and friendly approachability.

- **Standard Buttons/Inputs:** 8px radius.
- **Cards/Containers:** 16px radius.
- **Image Containers:** 16px radius.
- **Bottom Sheets:** 24px top-only radius to create a soft, "drawer" feel.

## Components

- **Buttons:** Primary buttons use a solid Teal Green fill with white text. Secondary buttons use the Teal Green for the label and a 1px border. No gradients or inner shadows.
- **Inputs:** Fields use a 1px Slate-200 border. On focus, the border transitions to Primary Teal. 
- **Cards:** Used for travel packages and destinations. Cards feature a 1px border and a very soft ambient shadow on hover/active states. 
- **Chips:** Small, 100px-radius pills used for categories (e.g., "Flight," "Bus," "Hotel"). Use a subtle teal tint (#F2FAF7) for the background and Primary Teal for text.
- **Bottom Navigation:** A persistent mobile bar with a white background, a light top border (#F1F5F9), and active icons highlighted in Primary Teal.
- **Status Badges:** Small, rounded indicators using the Accent Amber for "Pending" or "Limited" states, and Primary Teal for "Confirmed."