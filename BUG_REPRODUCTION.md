# tw-animate-css Bug Reproduction

This repository reproduces the bug reported in [Wombosvideo/tw-animate-css#58](https://github.com/Wombosvideo/tw-animate-css/issues/58).

## Bug Description

When using `tw-animate-css` with a custom design system that overrides Tailwind's spacing tokens, the build fails due to a missing `--spacing` variable.

## Environment

- **React Router**: v7.7.1
- **Tailwind CSS**: v4.1.4
- **tw-animate-css**: v1.3.8

## Steps to Reproduce

1. Create a React Router v7 app with Tailwind v4
2. Install and import `tw-animate-css`
3. Override spacing tokens with custom CSS:
   ```css
   @theme inline {
     --spacing-*: initial;
     --spacing-sp-0: 0px;
     --spacing-sp-1: 4px;
   }
   ```
4. Use tw-animate-css classes in components (e.g., `animate-fade-in`, `animate-slide-in-up`)
5. Run `npm run build`

## Expected Result

The build should succeed with custom spacing tokens.

## Actual Result

Build fails with error:
```
The --spacing(…) function requires that the `--spacing` theme variable exists, but it was not found.
```

## Root Cause

The `tw-animate-css` package relies on Tailwind's `--spacing()` function, which breaks when the base `--spacing` variable is removed by custom token overrides (`--spacing-*: initial;`).

## Files Modified

- `app/app.css`: Added tw-animate-css import and custom spacing tokens
- `app/welcome/welcome.tsx`: Added animation classes to trigger the bug

## To Reproduce the Bug

```bash
npm install
npm run build
```

The build will fail with the spacing error.

## Current Workaround

Adding `--spacing: 0.25rem;` to the theme fixes the build but undermines the custom spacing system:

```css
@theme inline {
  --spacing-*: initial;
  --spacing: 0.25rem;  /* Workaround */
  --spacing-sp-0: 0px;
  --spacing-sp-1: 4px;
}
```