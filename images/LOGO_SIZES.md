# Logo & Icon Sizes for ReplyMate

## Header logo (displayed at 40×40 px)
- **File:** `icon64.png` — 64×64 px (1x displays — downscaled for crisp rendering)
- **File:** `icon128.png` — 128×128 px (2x/retina displays)
- **Format:** PNG with transparency

> **Quality tip:** The header uses `srcset="icon64.png 1x, icon128.png 2x"` so both standard and retina displays get sharp icons. Never use a source smaller than the display size (e.g. 32px → 40px causes blur from upscaling).

## Favicon (browser tab)
- **File:** `icon32.png` — 32×32 px
- **File:** `icon16.png` — 16×16 px
- **File:** `icon128.png` — 128×128 px (apple-touch-icon for iOS home screen)
- **Format:** PNG

## Partner logos (Gmail, Outlook, Google — displayed at 20px height)
- **Gmail:** `gmail48.png` (1x), `gmail64.png` (2x)
- **Outlook:** `outlook48.png` — add `outlook64.png` for retina
- **Google:** `google48.png` — add `google64.png` for retina
