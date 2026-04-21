# Really Sick Animations

Create cinematic motion graphics using pure CSS. Screen-record the browser output for lightweight, scalable video content.

## When to Use

- Landing page hero animations
- Product demo motion graphics  
- Social media content (YouTube, TikTok, LinkedIn)
- Website transitions and reveals
- Anywhere you need video but don't want heavy After Effects files

## Why CSS Animations for Video

- **Tiny file sizes** — CSS text vs video renders
- **Infinitely scalable** — vector-based, no resolution loss
- **Easy iteration** — change a value, refresh, re-record
- **No software** — just browser + screen recorder
- **60fps by default** — browser rendering is smooth

## Core Techniques

### 1. Parameterized Animations with CSS Variables

Use CSS custom properties to make animations reusable and tweakable:

```css
.transition {
  --delay: 0.2s;        /* When animation starts */
  --duration: 1s;       /* How long it runs */
  --easing: cubic-bezier(0.87, 0.05, 0.02, 0.97);
  
  animation: 
    slideIn var(--duration) var(--easing) both,
    slideOut var(--duration) var(--easing) forwards;
  animation-delay: 
    calc(0s + var(--delay)), 
    calc(1.4s + var(--delay));
}
```

### 2. The Dramatic Easing Curve

This cubic-bezier creates snappy, cinematic motion:

```css
--cinematic: cubic-bezier(0.87, 0.05, 0.02, 0.97);
```

- Starts fast (0.87, 0.05)
- Holds in the middle
- Snaps to finish (0.02, 0.97)

Use it for reveals, wipes, and entrances.

### 3. Layered Pseudo-Elements

Create depth by stacking ::before and ::after with staggered delays:

```css
.wipe-effect {
  position: absolute;
  inset: 0;
  overflow: hidden;
  
  &::before, &::after {
    content: "";
    position: absolute;
    inset: 0;
    animation: slideIn 1s var(--cinematic) both;
  }
  
  &::before {
    background: #ccc;  /* light layer */
    animation-delay: 0s;
  }
  
  &::after {
    background: #666;  /* dark layer */
    animation-delay: 0.2s;  /* staggered */
  }
}
```

### 4. Clip-Path Wipes

Diagonal and shaped reveals using clip-path:

```css
/* Diagonal wipe */
@keyframes diagonalIn {
  from {
    clip-path: polygon(
      0 0, 0 0, 
      calc(var(--skew) * -1) 100%, 
      calc(var(--skew) * -1) 100%
    );
  }
  to {
    clip-path: polygon(
      0 0, calc(100% + var(--skew)) 0, 
      100% 100%, 
      calc(var(--skew) * -1) 100%
    );
  }
}

/* Arrow wipe */
@keyframes arrowIn {
  from {
    clip-path: polygon(
      calc(var(--sharpness) * -1) 0, 
      calc(var(--sharpness) * -1) 0, 
      0 50%, 
      0 50%
    );
  }
  to {
    clip-path: polygon(
      0 0, 
      calc(100% + var(--sharpness)) 0, 
      100% 50%, 
      0 50%
    );
  }
}
```

### 5. Orchestrated Sequences

Chain multiple animations with calculated delays:

```css
.scene-1 { --delay: 0s; }
.scene-2 { --delay: 2.4s; }   /* starts after scene 1 */
.scene-3 { --delay: 4.6s; }   /* starts after scene 2 */
```

Each scene has enter + exit animations with overlapping timing.

## Complete Example: Title Reveal

```html
<!DOCTYPE html>
<html>
<head>
<style>
  @import url("https://fonts.googleapis.com/css2?family=Inter:wght@300;700&display=swap");
  
  :root {
    --cinematic: cubic-bezier(0.87, 0.05, 0.02, 0.97);
    --bg: #0a0a0a;
    --accent: #ff3366;
  }
  
  body {
    margin: 0;
    background: var(--bg);
    height: 100vh;
    display: grid;
    place-items: center;
    font-family: "Inter", sans-serif;
  }
  
  .stage {
    width: 800px;
    height: 450px;
    position: relative;
    overflow: hidden;
    background: var(--bg);
  }
  
  /* Wipe overlay */
  .wipe {
    position: absolute;
    inset: 0;
    overflow: hidden;
    
    &::before, &::after {
      content: "";
      position: absolute;
      inset: 0;
      animation: 
        wipeIn 1s var(--cinematic) both,
        wipeOut 1s var(--cinematic) forwards;
    }
    
    &::before {
      background: var(--accent);
      animation-delay: 0s, 1.4s;
    }
    
    &::after {
      background: #fff;
      animation-delay: 0.15s, 1.25s;
    }
  }
  
  @keyframes wipeIn {
    from { transform: translateX(-101%); }
    to { transform: translateX(0); }
  }
  
  @keyframes wipeOut {
    from { transform: translateX(0); }
    to { transform: translateX(101%); }
  }
  
  /* Text reveal */
  .title {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
    opacity: 0;
    animation: fadeUp 0.8s var(--cinematic) 0.8s forwards;
  }
  
  .title h1 {
    margin: 0;
    font-size: 64px;
    font-weight: 700;
    color: #fff;
    letter-spacing: -2px;
  }
  
  .title p {
    margin: 16px 0 0;
    font-size: 20px;
    font-weight: 300;
    color: rgba(255,255,255,0.6);
  }
  
  @keyframes fadeUp {
    from {
      opacity: 0;
      transform: translate(-50%, calc(-50% + 30px));
    }
    to {
      opacity: 1;
      transform: translate(-50%, -50%);
    }
  }
</style>
</head>
<body>
  <div class="stage">
    <div class="wipe"></div>
    <div class="title">
      <h1>YOUR BRAND</h1>
      <p>Motion that moves</p>
    </div>
  </div>
</body>
</html>
```

## Screen Recording Tips

1. **Use a fixed viewport** — set explicit width/height on your container
2. **Standard sizes:**
   - YouTube: 1920x1080 (16:9)
   - Instagram/TikTok: 1080x1920 (9:16)
   - LinkedIn: 1200x627 (1.91:1)
3. **Record at 60fps** — CSS animations are smooth
4. **Chrome DevTools** — use Device Toolbar for exact viewport sizing
5. **Clean background** — no browser UI, use `body { margin: 0; }`
6. **Multiple takes** — refresh and re-record until perfect

## Workflow

1. **Design** — sketch the animation sequence
2. **Build** — HTML + CSS with parameterized variables
3. **Preview** — open in browser, tweak timing
4. **Record** — screen capture the animation
5. **Edit** — trim in any video editor (optional)
6. **Publish** — upload as video content

## Advanced Patterns

### Staggered Grid Reveal

```css
.grid-item {
  opacity: 0;
  transform: translateY(20px);
  animation: fadeUp 0.6s var(--cinematic) forwards;
}

.grid-item:nth-child(1) { animation-delay: 0.1s; }
.grid-item:nth-child(2) { animation-delay: 0.2s; }
.grid-item:nth-child(3) { animation-delay: 0.3s; }
```

### Continuous Loop (for backgrounds)

```css
@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-20px); }
}

.floating {
  animation: float 4s ease-in-out infinite;
}
```

## Resources

- **Inspiration:** [yui540.com/motions](https://yui540.com/motions)
- **Source code:** [github.com/yui540/css-animations](https://github.com/yui540/css-animations)
- **Clip-path generator:** [bennettfeely.com/clippy](https://bennettfeely.com/clippy)
- **Easing functions:** [easings.net](https://easings.net)
