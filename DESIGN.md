---
name: Dark Travel System
colors:
  surface: '#0f1512'
  surface-dim: '#0f1512'
  surface-bright: '#343b37'
  surface-container-lowest: '#0a0f0d'
  surface-container-low: '#171d1a'
  surface-container: '#1b211e'
  surface-container-high: '#252b28'
  surface-container-highest: '#303633'
  on-surface: '#dee4de'
  on-surface-variant: '#bccac1'
  inverse-surface: '#dee4de'
  inverse-on-surface: '#2c322e'
  outline: '#87948c'
  outline-variant: '#3d4943'
  surface-tint: '#68dbae'
  primary: '#68dbae'
  on-primary: '#003827'
  primary-container: '#26a37a'
  on-primary-container: '#003121'
  inverse-primary: '#006c4e'
  secondary: '#ffb869'
  on-secondary: '#482900'
  secondary-container: '#a76500'
  on-secondary-container: '#fffbff'
  tertiary: '#ffb3ad'
  on-tertiary: '#5f1413'
  tertiary-container: '#db726b'
  on-tertiary-container: '#560d0e'
  error: '#ffb4ab'
  on-error: '#690005'
  error-container: '#93000a'
  on-error-container: '#ffdad6'
  primary-fixed: '#86f8c9'
  primary-fixed-dim: '#68dbae'
  on-primary-fixed: '#002115'
  on-primary-fixed-variant: '#00513a'
  secondary-fixed: '#ffdcbb'
  secondary-fixed-dim: '#ffb869'
  on-secondary-fixed: '#2b1700'
  on-secondary-fixed-variant: '#673d00'
  tertiary-fixed: '#ffdad6'
  tertiary-fixed-dim: '#ffb3ad'
  on-tertiary-fixed: '#410003'
  on-tertiary-fixed-variant: '#7e2a27'
  background: '#0f1512'
  on-background: '#dee4de'
  surface-variant: '#303633'
typography:
  headline-lg:
    fontFamily: Plus Jakarta Sans
    fontSize: 40px
    fontWeight: '700'
    lineHeight: '1.2'
  headline-md:
    fontFamily: Plus Jakarta Sans
    fontSize: 32px
    fontWeight: '600'
    lineHeight: '1.3'
  headline-sm:
    fontFamily: Plus Jakarta Sans
    fontSize: 24px
    fontWeight: '600'
    lineHeight: '1.4'
  body-lg:
    fontFamily: Be Vietnam Pro
    fontSize: 18px
    fontWeight: '400'
    lineHeight: '1.6'
  body-md:
    fontFamily: Be Vietnam Pro
    fontSize: 16px
    fontWeight: '400'
    lineHeight: '1.6'
  body-sm:
    fontFamily: Be Vietnam Pro
    fontSize: 14px
    fontWeight: '400'
    lineHeight: '1.5'
  label-md:
    fontFamily: Plus Jakarta Sans
    fontSize: 14px
    fontWeight: '600'
    lineHeight: '1'
    letterSpacing: 0.05em
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  unit: 8px
  gutter: 24px
  margin: 32px
  container-max: 1280px
---

## Brand & Style

This design system is engineered for the sophisticated traveler who plans journeys under the glow of the moon. The brand personality is adventurous yet grounded, blending the excitement of global discovery with the reliability of premium logistics. By shifting to a dark mode aesthetic, the UI evokes a sense of nocturnal luxury and focused utility, reducing eye strain for late-night itinerary planning while making travel imagery pop.

The design style follows a refined **Minimalism** approach. It utilizes high-contrast typography and intentional negative space to ensure clarity. The aesthetic is anchored by structured card-based layouts that organize complex travel data into digestible units, creating a tactile digital experience that feels as organized as a well-packed suitcase.

## Colors

The palette is defined by a deep, atmospheric foundation that allows the brand's core colors to radiate. The primary Teal Green (#1D9E75) is used for high-priority actions and success states, symbolizing the lush destinations and steady guidance. The Amber (#BA7517) serves as a strategic accent, highlighting interactive elements, warnings, or premium features like star ratings and special offers.

Backgrounds are layered using Dark Charcoal (#0F1115) for the base and Deep Slate (#1C1F26) for card surfaces, creating a clear hierarchical distinction. Text is rendered in pure white for maximum legibility on headlines, while light gray is reserved for secondary information to maintain a balanced visual weight.

## Typography

The typography strategy employs a dual-font system to balance character with utility. **Plus Jakarta Sans** is used for headlines and navigational labels; its soft, rounded terminals provide a welcoming, optimistic tone essential for the travel industry. **Be Vietnam Pro** is utilized for all body copy and descriptions, chosen for its exceptional readability in dark environments and its contemporary, warm feel.

High contrast is maintained by keeping headlines in pure white. For body text, the line height is generous (1.6) to prevent "haloing" effects often found in white text on dark backgrounds, ensuring the content remains comfortable to read during long browsing sessions.

## Layout & Spacing

The layout philosophy is based on a **Fluid Grid** system that adapts to various screen sizes while maintaining strict alignment. A 12-column grid is standard for desktop, reducing to 4 columns for mobile. All spatial relationships are governed by an 8px base unit, ensuring a consistent rhythm across paddings, margins, and component heights.

Cards are the primary container for content, separated by 24px gutters to allow the deep background to act as a natural separator. Large 32px margins on the page edges ensure that content never feels cramped against the bezel of the device, reinforcing the sense of "space" and "freedom" inherent in travel.

## Elevation & Depth

This design system moves away from traditional drop shadows, which can appear muddy on dark charcoal surfaces. Instead, it utilizes **Tonal Layers** and **Low-Contrast Outlines**. Depth is communicated by color shifting: the "higher" an element is in the stack, the lighter its surface color becomes.

Floating elements or active cards should use a subtle 1px border (#2D343F) to define their edges against the dark background. When a card requires additional emphasis, a very faint, large-radius glow using the Primary Teal (at 5-10% opacity) may be applied to suggest a soft ambient lift without creating harsh shadows.

## Shapes

The shape language is consistently **Rounded**, using a 0.5rem (8px) base radius. This softening of the corners makes the dark, high-contrast UI feel more approachable and less "technical" or "industrial." 

- Standard components (Buttons, Inputs): 0.5rem
- Content Containers (Cards, Modals): 1rem
- Decorative elements or featured tags: Pill-shaped (Full radius)

This hierarchy of roundedness helps users distinguish between interactive controls and structural layout elements at a glance.

## Components

### Buttons
Primary buttons use the Teal Green (#1D9E75) with white text. Secondary buttons use a transparent background with the subtle border (#2D343F) and light gray text. The Amber (#BA7517) is reserved for "special" call-to-actions, such as "Book Now" or "Limited Offer."

### Cards
Cards are the heart of the design system. They feature the Deep Slate (#1C1F26) background and the subtle 1px border. On hover, the border color should shift to the Teal Green to provide clear interactive feedback.

### Inputs & Selects
Input fields use a darker background than the surface cards to create an "inset" feel. Labels are positioned above the field in Plus Jakarta Sans (Label-md) for clarity. The focus state is indicated by a Teal Green border and a subtle inner glow.

### Chips & Tags
Used for travel categories (e.g., "Eco-friendly," "Luxury," "Solo"). These use a semi-transparent version of the primary or secondary color (15% opacity) with a solid text color of the same hue to ensure they are legible but don't compete with the primary action buttons.

### Travel-Specific Components
- **Itinerary Timeline:** A vertical line component using the Teal Green, with Deep Slate nodes for each travel stop.
- **Price Indicators:** High-contrast Amber text used for pricing to ensure it is the first thing a user notices on a travel card.
- **Progressive Image Loaders:** Dark gray skeletons that match the card background to prevent jarring white flashes during image loading.