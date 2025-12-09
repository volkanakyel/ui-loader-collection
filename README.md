# Loader Collection

A premium collection of 44 minimalist loaders designed for high-end projects. Built with Nuxt, featuring pure CSS animations and a sophisticated filtering system.

---

## Features

- **44 Unique Loaders** — Carefully crafted animations in pure CSS
- **8 Categories** — Organized by style and complexity
- **Zero Dependencies** — No external animation libraries
- **Fully Responsive** — Adapts seamlessly across all devices
- **Dark Mode** — Minimalist black and white aesthetic
- **Smooth Transitions** — Premium morphism effects on category changes
- **Click to Preview** — Full-screen modal view for each loader
- **Production Ready** — Optimized for performance

---

## Categories

### **Circular** (11 loaders)

Classic rotating and orbital animations with smooth curves.

- Radar Sweep
- Orbital Ring
- Morph Circle
- Pulse Ring
- Spinner
- Wave Ripple
- Dual Orbit
- Dots
- Arc Sweep
- Concentric Spin
- Ellipse Morph

### **Linear** (6 loaders)

Straight-line movements with clean geometric precision.

- Bars
- Infinity Loop
- Cross Fade
- Line Bounce
- Rectangle Slide
- Parallel Lines

### **Angular** (10 loaders)

Sharp edges and geometric patterns with precise movements.

- Square Corners
- Particle Swarm
- Grid Pulse
- Triangle Spin
- Square Rotate
- Zigzag
- Corner Dots
- Diamond Pulse
- Staircase
- Arrow Path

### **Geometric** (5 loaders)

Complex polygon-based designs with sophisticated animations.

- Hexagon Mesh
- Pentagon Orbit
- Octagon Spin
- Tesseract
- Kaleidoscope

### **Organic** (4 loaders)

Natural, flowing movements inspired by physics.

- Wave Flow
- Breathing
- Sine Pulse
- Elastic Band

### **Digital** (4 loaders)

Tech-inspired effects with modern aesthetics.

- Glitch Matrix
- Binary Rain
- Pixel Fade
- Scan Line

### **Premium** (4 loaders)

The most complex and visually striking animations.

- Fractal Spin
- Helix DNA
- Quantum Field
- Singularity

---

## Installation

### Clone the repository

```bash
git clone <repository-url>
cd ui-loader-collection
```

### Install dependencies

```bash
npm install
```

### Run development server

```bash
npm run dev
```

Visit `http://localhost:3000/loader` to view the collection.

---

## Usage

### Basic Implementation

Each loader can be used independently by copying its CSS class:

```vue
<template>
  <div class="loader-1"></div>
</template>

<style>
.loader-1 {
  width: 64px;
  height: 64px;
  position: relative;
  animation: rotation 2s linear infinite;
}

.loader-1::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 50%;
  height: 2px;
  background: linear-gradient(to right, rgba(255, 255, 255, 0.8), transparent);
  transform-origin: left center;
}

@keyframes rotation {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
```

### Customization

#### Change Size

```css
.loader-1 {
  width: 128px; /* Double size */
  height: 128px;
}
```

#### Change Color

```css
.loader-1::before {
  background: linear-gradient(to right, #3b82f6, transparent);
}
```

#### Adjust Speed

```css
.loader-1 {
  animation: rotation 1s linear infinite; /* Faster */
}
```

---

## File Structure

```
pages/
  └── loader.vue        # Main loader collection page
```

### Key Components

- **Filter System** — 8 category pills with smooth transitions
- **Grid Layout** — Responsive 1-2-3 column layout
- **Transition Effects** — Morphism overlay with staggered animations
- **Modal View** — Full-screen preview with 2.5x scale

---

## Technical Details

### Transitions

- **Category Change** — Phased animation with blur morphism

  - Exit: 0.35s with 15px blur
  - Gap: 0.35s delay
  - Entry: 0.6s elastic bounce with stagger

- **Hover Effects** — Subtle border and separator changes
  - Border: Transparent → 8% white opacity
  - Background: 1% white overlay
  - Letter spacing expansion

### Performance

- **Hardware Acceleration** — `will-change` for smooth 60fps
- **CSS-only** — No JavaScript animation overhead
- **Optimized Easing** — Custom cubic-bezier curves
- **Staggered Rendering** — Reduces paint complexity

---

## Browser Support

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

Requires support for:

- CSS Grid
- CSS Custom Properties
- CSS Backdrop Filter
- CSS Clip Path
- Vue 3 Transition Components

---

## Customization Guide

### Creating New Categories

1. Add category to array:

```ts
const categories = [
  // ...existing
  { id: 'custom', label: 'Custom' },
]
```

2. Tag loaders with category:

```ts
{ name: 'New Loader', category: 'custom' }
```

### Adding New Loaders

1. Add loader to array:

```ts
{ name: 'Custom Loader', category: 'circular' }
```

2. Create CSS class (increment number):

```css
.loader-45 {
  width: 64px;
  height: 64px;
  /* your animation */
}
```

---

## Design Principles

1. **Minimalism** — Black and white only, no colors
2. **Subtlety** — Gentle hover effects, non-disruptive
3. **Performance** — Pure CSS, hardware-accelerated
4. **Consistency** — All loaders 64x64px by default
5. **Smoothness** — Custom easing curves throughout
6. **Clarity** — Clear naming and categorization

---

## License

MIT License - feel free to use in commercial and personal projects.

---

## Credits

Designed and developed with attention to detail for high-end projects.

Built with:

- **Nuxt** — Vue framework
- **Tailwind CSS** — Utility-first CSS
- **TypeScript** — Type safety
- **Pure CSS** — Zero animation dependencies

---

## Contributing

Contributions are welcome! Please ensure:

- Loaders follow minimalist black/white aesthetic
- Animations are pure CSS (no JavaScript)
- Code is well-commented
- Loaders are 64x64px default size
- Smooth transitions with appropriate easing

---

**View Collection:** `/loader`

**Total Loaders:** 44

**Total Categories:** 8

**Zero Dependencies:** ✓

**Production Ready:** ✓
