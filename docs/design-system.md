# Design System - Agriculture Billing Software

## Color Palette

### Primary Colors
- **Primary Purple**: `#667eea` - Main brand color
- **Secondary Purple**: `#764ba2` - Gradient accent
- **Success Green**: `#10b981` - Positive actions, stock status
- **Warning Orange**: `#f59e0b` - Low stock, pending items
- **Danger Red**: `#ef4444` - Critical alerts, errors
- **Info Blue**: `#3b82f6` - Information, links

### Neutral Colors
- **Dark**: `#1a202c` - Primary text
- **Medium**: `#4a5568` - Secondary text
- **Light**: `#718096` - Tertiary text
- **Background**: `#f5f7fa` - Page background
- **Card**: `#ffffff` - Card background
- **Border**: `#e2e8f0` - Borders, dividers

## Typography

### Font Family
- Primary: `-apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen', 'Ubuntu', 'Cantarell', sans-serif`

### Font Sizes
- **Heading 1**: 1.875rem (30px) - Page titles
- **Heading 2**: 1.5rem (24px) - Section titles
- **Heading 3**: 1.25rem (20px) - Card titles
- **Body**: 1rem (16px) - Regular text
- **Small**: 0.875rem (14px) - Labels, captions
- **Tiny**: 0.75rem (12px) - Badges, tags

### Font Weights
- **Regular**: 400
- **Medium**: 500
- **Semibold**: 600
- **Bold**: 700

## Spacing System

Based on 8px grid:
- **xs**: 0.25rem (4px)
- **sm**: 0.5rem (8px)
- **md**: 1rem (16px)
- **lg**: 1.5rem (24px)
- **xl**: 2rem (32px)
- **2xl**: 3rem (48px)

## Components

### Buttons

#### Primary Button
```css
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
color: white;
padding: 0.75rem 1.5rem;
border-radius: 8px;
font-weight: 600;
```

#### Secondary Button
```css
background: white;
color: #667eea;
border: 2px solid #667eea;
padding: 0.75rem 1.5rem;
border-radius: 8px;
font-weight: 600;
```

#### Icon Button
```css
background: none;
border: none;
padding: 0.5rem;
font-size: 1.125rem;
```

### Cards
```css
background: white;
border-radius: 12px;
padding: 1.5rem;
box-shadow: 0 1px 3px rgba(0,0,0,0.1);
```

### Input Fields
```css
padding: 0.75rem 1rem;
border: 2px solid #e2e8f0;
border-radius: 8px;
font-size: 1rem;
```

Focus state:
```css
border-color: #667eea;
outline: none;
```

### Status Badges
- **Good/Success**: Green background
- **Warning/Low**: Orange background
- **Critical/Error**: Red background
- **Info**: Blue background

```css
padding: 0.25rem 0.75rem;
border-radius: 12px;
font-size: 0.75rem;
font-weight: 600;
color: white;
```

## Layout

### Sidebar
- Width: 260px
- Background: Gradient purple
- Fixed position
- Collapsible on mobile

### Main Content
- Margin-left: 260px (when sidebar open)
- Padding: 2rem
- Background: #f5f7fa

### Top Header
- Height: 64px
- Background: white
- Sticky position
- Border-bottom: 1px solid #e2e8f0

## Icons

Using emoji icons for simplicity:
- 🧾 Billing
- 📦 Inventory
- 👥 Customers
- 📊 Dashboard
- 📈 Reports
- 🔍 Search
- 🎤 Voice
- 💵 Cash
- 📱 UPI
- 💳 Card
- ✏️ Edit
- 🗑️ Delete
- 👁️ View

## Responsive Breakpoints

- **Mobile**: < 768px
- **Tablet**: 768px - 1024px
- **Desktop**: > 1024px

### Mobile Adaptations
- Sidebar hidden by default
- Single column layouts
- Larger touch targets (min 44px)
- Simplified navigation

## Animations

### Transitions
```css
transition: all 0.2s ease;
```

### Hover Effects
- Buttons: `transform: translateY(-2px)`
- Cards: `box-shadow: 0 8px 16px rgba(0,0,0,0.1)`
- Icons: `transform: scale(1.2)`

## Accessibility

- Minimum contrast ratio: 4.5:1
- Focus indicators on all interactive elements
- ARIA labels for icon buttons
- Keyboard navigation support
- Screen reader friendly

## Dark Mode (Future)

Planned color scheme:
- Background: `#1a202c`
- Card: `#2d3748`
- Text: `#f7fafc`
- Border: `#4a5568`
