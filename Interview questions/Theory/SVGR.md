# SVGR Notes

## 1. What is SVGR?

SVGR is a tool that converts `.svg` files into React components.

```text
SVG file
   ↓
SVGR
   ↓
React Component
```

Example:

```jsx
import { ReactComponent as WarningIcon } from './warning.svg';

<WarningIcon />
```

SVGR is mainly useful when SVGs need to behave like reusable React components instead of static image files. 【1-fb39e4】【2-abf055】

---

## 2. Does SVGR come with React?

No.

SVGR is **not part of React itself**.

React only renders components. SVGR is a build tool/plugin that helps convert SVG files into React components during build time.

```text
React alone        ❌ Does not include SVGR
CRA                ✅ Includes SVGR through webpack config
Custom Webpack     ✅ Need to install @svgr/webpack
Vite               ✅ Need to install vite-plugin-svgr
```

---

## 3. SVGR in Create React App (CRA)

In **Create React App**, SVGR support is already available.

No explicit installation is required.

CRA has SVGR configured internally through its webpack setup, so this works directly:

```jsx
import { ReactComponent as WarningIcon } from './warning.svg';

function App() {
  return <WarningIcon />;
}

export default App;
```

So, in CRA:

```text
No need to install SVGR separately.
It comes pre-configured through CRA webpack setup.
```

## 4. What is the Issue with Regular SVG?

Regular SVG is usually imported as an asset/image.

Example:

```jsx
import warningIcon from './warning.svg';

<img src={warningIcon} alt="warning" />
```

Build output:

```text
warning.svg
   ↓
Copied as static asset
   ↓
dist/assets/warning.hash.svg
```

### 5. Problems with regular SVG

- Hard to change internal `fill` or `stroke`
- Cannot easily pass React props
- Cannot use like a component
- Less flexible for reusable UI icons
- Better only for static images like logos or banners


```jsx
import warningIcon from './assets/warning.svg';
<img src={warningIcon} alt="Warning" />
```

Regular SVG is good for:

```text
logo.svg
banner.svg
background.svg
large-illustration.svg
```

---

## 6. SVG as Manual JS Component

We can manually create an SVG React component.

Example:

```jsx
function WarningIcon({ size = 20, color = 'currentColor' }) {
  return (
    <svg width={size} height={size} viewBox="0 0 24 24" fill={color}>
      <path d="M12 2L2 22h20L12 2z" />
    </svg>
  );
}

export default WarningIcon;
```

Usage:

```jsx
<WarningIcon size={24} color="orange" />
```

### Benefit

- Full control
- Can pass props
- Easy to style

### Problem

- Manual work
- Need to convert SVG syntax to JSX
- Hard to maintain many icons
- Duplicate files may be created

Example issue:

```xml
stroke-width="2"
fill-rule="evenodd"
```

must be manually changed to:

```jsx
strokeWidth="2"
fillRule="evenodd"
```

**SVGR handles this conversion automatically during build time.**

---

## 7. Does SVGR help with build: 
NO - main benefit is getting svg as react components automatically

---
## 8. Benefits of SVGR

### 1. Converts SVG to React Component Automatically

No need to manually create `.jsx` icon components.

```text
warning.svg
   ↓
<WarningIcon />
```
### 2. Easier Styling

You can use:

```jsx
<WarningIcon className="text-orange-500" />
```

or:

```jsx
<WarningIcon width={16} height={16} />
```

### 3. currentColor
# currentColor in SVG

## What is `currentColor`?

`currentColor` is a special CSS value that makes an SVG inherit the current CSS `color` property from its parent element.

Example:

```svg
<path fill="currentColor" />
```

Instead of hardcoding color:

```svg
<path fill="#FF0000" />
```

the icon color can be controlled from CSS.

---

## Default Color

If no color is specified, `currentColor` usually uses the browser's default text color.

```jsx
<WarningIcon />
```

Result:

```text
Usually black
```

---

## Using CSS Color

```jsx
<div style={{ color: 'orange' }}>
  <WarningIcon />
</div>
```

Result:

```text
Orange icon
```

The SVG inherits the parent color.

---

## Using Tailwind

```jsx
<WarningIcon className="text-red-500" />
<WarningIcon className="text-blue-500" />
<WarningIcon className="text-gray-400" />
```

Result:

```text
Same icon
Different colors
```

---

## Why `currentColor` is Useful

Without `currentColor`:

```svg
<path fill="#FF0000" />
```

Even if you write:

```jsx
<WarningIcon className="text-blue-500" />
```

the icon may still stay red because the path has a hardcoded fill.

---

With `currentColor`:

```svg
<path fill="currentColor" />
```

```jsx
<WarningIcon className="text-blue-500" />
```

Result:

```text
Blue icon
```

---

## Manual SVG Component with Color Prop

You can also control color manually using props.

```jsx
function WarningIcon({ color = 'blue' }) {
  return (
    <svg viewBox="0 0 24 24">
      <path fill={color} d="..." />
    </svg>
  );
}
```

Usage:

```jsx
<WarningIcon color="red" />
```

This works.

---

## Issue with Manual Color Prop

If the SVG has many paths:

```jsx
function WarningIcon({ color }) {
  return (
    <svg>
      <path fill="#FF0000" d="..." />
      <path fill="#00FF00" d="..." />
      <path fill="#0000FF" d="..." />
    </svg>
  );
}
```

Passing:

```jsx
<WarningIcon color="blue" />
```

will not automatically change all paths.

You must manually update each path:

```jsx
<path fill={color} d="..." />
<path fill={color} d="..." />
<path fill={color} d="..." />
```

For complex icons, this becomes difficult to maintain.

---

## With `currentColor`

```svg
<path fill="currentColor" />
```

Then color can be controlled from outside:

```jsx
<WarningIcon className="text-blue-500" />
```

No need to pass color props to each path.

---

## Do We Need to Target Path?

Yes, if the SVG has hardcoded colors.

Example:

```svg
<path fill="#FF0000" />
```

Then this may not work:

```jsx
<WarningIcon className="text-blue-500" />
```

You may need to:

- edit the SVG and replace hardcoded color with `currentColor`
- target paths using CSS
- or manually pass color props to each path

---

## Difference Between Manual Prop and `currentColor`

Manual color prop:

```text
Color is controlled inside the SVG component
```

`currentColor`:

```text
Color is controlled from CSS / Tailwind / parent element
```

---

## Simple Takeaway

```text
SVGR = Converts SVG into React component

currentColor = Lets CSS control SVG color

Manual color prop = Also works, but needs code changes inside SVG

currentColor = Cleaner for reusable icon systems
```

---

## Benefits of `currentColor`

- Same SVG can be reused with different colors
- Works well with CSS
- Works well with Tailwind
- No need for multiple colored SVG files
- No need to pass color props to every path
- Better for reusable icon libraries
- Useful for themes like light mode and dark mode

---

## Final Summary

Use `currentColor` when you want the SVG icon color to be controlled from CSS.
it makes reusable icons cleaner and easier to maintain.

```jsx
<WarningIcon className="text-orange-500" />
<WarningIcon className="text-red-500" />
<WarningIcon className="text-gray-400" />
```
Same icon, different colors, no SVG changes.

---

### 4. Better Reusability

Same icon can be reused in multiple places:

```jsx
<WarningIcon />
<WarningIcon className="error" />
<WarningIcon className="disabled" />
```

---

### 5. Less Manual Conversion

SVGR automatically converts SVG attributes into JSX-compatible attributes.

Example:

```xml
stroke-width
```

becomes:

```jsx
strokeWidth
```

---

### 6. Easier Design Updates

If the design team updates the SVG:

```text
Replace SVG file
   ↓
Build runs
   ↓
Component is updated automatically
```

No need to manually update JSX.

---

## 9. Should we use SVGR always? When SVGR May Not Be Necessary
SVGR can be used for any SVG, including logos and illustrations.
However, for purely static graphics, the extra flexibility of a React component is often not needed.

Common examples:

- Logos
- Large illustrations
- Background graphics
- Hero banners
- Static decorative images

Example:

```jsx
import logo from './logo.svg';

function App() {
  return <img src={logo} alt="Company Logo" />;
}
```

### Why?

These assets typically:

- Do not need React props
- Do not need dynamic colors or sizing
- Do not require interaction
- Rarely change at runtime

In such cases, using the SVG as a regular asset keeps the implementation simple.

### Key Point

```text
SVGR is not wrong for logos or illustrations.
It is simply most valuable when SVGs need to behave like reusable React components.
```
---

## 10. Final Summary

SVGR converts SVG files into React components during build time.

It is useful because:

- SVGs become reusable React components
- No manual JSX conversion is needed
- Styling becomes easier
- Props can be passed
- Icons are easier to maintain
- CRA already supports it through webpack
- Custom Webpack/Vite projects need setup

One-liner:

```text
SVGR lets us keep SVG files in assets and use them as React components without manually creating JS icon components.
```
