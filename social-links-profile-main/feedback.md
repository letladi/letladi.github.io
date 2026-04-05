# Frontend Mentor — Social Links Profile: Feedback

**Challenge:** Social Links Profile (Newbie)
**Marker:** Antigravity AI
**Date:** 2026-04-05

---

## Overall Assessment: ✅ Very Good — 8.5/10

Another clean, competent solution. The core layout, colour palette, and hover interaction are all correct and the fluid sizing trick with `min()` has been carried over from the previous challenge — great. A few semantic HTML issues and some minor visual inaccuracies around sizing and font-weight keep it just shy of perfect.

---

## ✅ What Was Done Well

### 1. Spot-On Colour Palette
All five design tokens from the style guide (`--green`, `--white`, `--gray-700/800/900`) are defined correctly in `:root` and used consistently throughout. This is exactly the right way to set up a design system.

### 2. Fluid Card Sizing
```css
width: min(320px, calc(100% - 48px));
```
Same excellent technique as the previous challenge. The card will never touch the screen edges on small viewports, while staying correctly bounded on larger ones. 

### 3. Hover State Accuracy
```css
ul a:hover {
    color: var(--gray-800);
    background-color: var(--green);
}
```
The green-background / dark-text hover state matches the `active-states.jpg` design reference precisely.

### 4. Body Layout
`display: grid; place-items: center; height: 100dvh;` on the body is the most concise and correct way to perfectly centre a single card. Using `dvh` (dynamic viewport height) instead of the older `vh` is also a modern, considered choice that avoids the classic mobile browser address-bar issue.

### 5. CSS Custom Properties
Every magic number (spacing, rounding, avatar size, font size) is extracted into a well-named variable. This makes the stylesheet very easy to read and tweak.

---

## ❌ Issues & Areas to Improve

### 1. 🏷️ Wrong Heading Level for Location
**File:** `index.html`, line 136

```html
<!-- Current -->
<p class="green">London, United Kingdom</p>
```

Wait — actually location is a `<p>`. But looking above, line 78:

```css
.user-info h2 {
    padding-top: var(--min-padding);
}
```

There is a rule targeting `.user-info h2`, but there is no `<h2>` in the markup. The location is already a `<p>`. The orphaned CSS rule for `h2` is harmless dead code, but it signals the element was an `h2` at some point and should be cleaned up.

> **Fix:** Remove the `.user-info h2` CSS rule since it targets nothing in the current HTML.

---

### 2. 🔤 Name Font Weight Is Wrong
**File:** `index.html`, line 73–76

```css
/* Current */
.user-info > h1 {
    margin-top: 20px;
    font-weight: 400;  /* ← regular weight */
}
```

In the design reference (`destkop-design.jpg` and `mobile-design.jpg`), "Jessica Randall" is rendered in **bold (700)**. The Inter font is loaded with weights 400–700, so weight 700 is available. Setting `font-weight: 400` makes the name appear lighter than the design intends.

> **Fix:**
> ```css
> .user-info > h1 {
>     margin-top: 20px;
>     font-weight: 700;
> }
> ```

---

### 3. 📐 Card & Button Border Radius Too Small
**File:** `index.html`, lines 41, 103, 115

```css
/* Current */
--sml-rounding: 5px;

/* Applied to both card and buttons */
figure        { border-radius: var(--sml-rounding); }  /* 5px */
ul a          { border-radius: var(--sml-rounding); }  /* 5px */
```

Comparing the design reference to the rendered output, both the card container and the buttons have a noticeably more generous corner radius — approximately **12px** on the card and **8–9px** on the buttons. At 5px, both elements look sharper / squarer than intended.

> **Fix:** Separate the two concerns and increase the values:
> ```css
> :root {
>     --card-rounding: 12px;
>     --btn-rounding: 8px;
> }
> figure { border-radius: var(--card-rounding); }
> ul a   { border-radius: var(--btn-rounding); }
> ```

---

### 4. 📏 Button Vertical Padding Is Slightly Tight
**File:** `index.html`, line 97

```css
/* Current */
ul a {
    padding: 10px 0;
}
```

The design shows the button labels with a bit more breathing room — closer to **12–14px** top and bottom. At 10px the buttons look compact.

> **Fix:**
> ```css
> ul a {
>     padding: 12px 0;
> }
> ```

---

### 5. 🧱 Questionable Use of `<figure>` / `<figcaption>`
**File:** `index.html`, lines 131–148

```html
<figure>
    <img src="..." alt="user avatar" />
    <figcaption>
        <div class="user-info">...</div>
        <p>...</p>
        <ul>...</ul>
    </figcaption>
</figure>
```

The `<figure>` element is specifically intended for self-contained content that is referenced from the main document — typically an image, diagram, or code listing with an optional caption. Wrapping a full profile card (name, location, bio, *and* a list of nav links) inside `<figure>/<figcaption>` stretches the semantic intent.

> **Better alternative:** Use a straightforward `<main>` + `<article>` structure:
> ```html
> <main>
>   <article class="profile-card">
>     <img src="..." alt="Jessica Randall's avatar" />
>     <div class="user-info">
>       <h1>Jessica Randall</h1>
>       <p class="location green">London, United Kingdom</p>
>     </div>
>     <p class="bio">"Front-end developer and avid reader."</p>
>     <ul>...</ul>
>   </article>
> </main>
> ```
> This is semantically clean: `<main>` marks the primary page content, and `<article>` maps perfectly to a self-contained profile block.

---

### 6. ♿ Alt Text Could Be More Descriptive
**File:** `index.html`, line 132

```html
<img src="./assets/images/avatar-jessica.jpeg" alt="user avatar" />
```

"user avatar" is generic. Since the person's name is known, the alt text should identify them.

> **Fix:**
> ```html
> <img src="./assets/images/avatar-jessica.jpeg" alt="Jessica Randall" />
> ```

---

### 7. 📱 `height: 100dvh` Needs a Safety Net
**File:** `index.html`, line 57

```css
body {
    height: 100dvh;
}
```

On very short viewports (landscape phones, small tablets), the card is taller than `100dvh`, causing it to be clipped or overflow. Adding `min-height` instead of (or in addition to) `height` ensures the page scrolls rather than clips when content overflows.

> **Fix:**
> ```css
> body {
>     min-height: 100dvh;
>     /* remove height: 100dvh */
> }
> ```

---

## 📊 Scoring Breakdown

| Category               | Score | Notes |
|------------------------|-------|-------|
| Visual accuracy        | 8/10  | Good overall; border radius and name weight diverge from design |
| Code quality / CSS     | 9/10  | Excellent variable usage; dead `.user-info h2` rule to clean up |
| Responsiveness         | 9/10  | Great `min()` sizing; needs `min-height` safety net |
| Semantic HTML          | 7/10  | `<figure>` misuse; alt text is generic |
| Best practices         | 9/10  | Modern CSS throughout; minor padding tweak needed |
| **Overall**            | **8.5/10** | |

---

## 🔧 Suggested Revised Snippet (Key Fixes)

```html
<main>
  <article class="profile-card">
    <img src="./assets/images/avatar-jessica.jpeg" alt="Jessica Randall" />
    <div class="user-info">
      <h1>Jessica Randall</h1>
      <p class="location green">London, United Kingdom</p>
    </div>
    <p class="bio">"Front-end developer and avid reader."</p>
    <ul>
      <li><a href="#">GitHub</a></li>
      <li><a href="#">Frontend Mentor</a></li>
      <li><a href="#">LinkedIn</a></li>
      <li><a href="#">Twitter</a></li>
      <li><a href="#">Instagram</a></li>
    </ul>
  </article>
</main>
```

```css
:root {
    --green: hsl(75, 94%, 57%);
    --white: hsl(0, 0%, 100%);
    --gray-900: hsl(0, 0%, 8%);
    --gray-800: hsl(0, 0%, 12%);
    --gray-700: hsl(0, 0%, 20%);
    --body-font-size: 14px;
    --card-rounding: 12px;
    --btn-rounding: 8px;
    --avatar-size: 88px;
}

body {
    font-family: "Inter", sans-serif;
    background-color: var(--gray-900);
    display: grid;
    place-items: center;
    min-height: 100dvh;        /* ← min-height, not height */
    font-size: var(--body-font-size);
    color: var(--white);
    text-align: center;
}

.profile-card {
    width: min(320px, calc(100% - 48px));
    background-color: var(--gray-800);
    padding: 24px 20px;
    border-radius: var(--card-rounding);  /* ← 12px */
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
}

.profile-card img {
    border-radius: 50%;
    width: var(--avatar-size);
}

.user-info h1 {
    font-size: 1.4rem;
    font-weight: 700;          /* ← bold, not 400 */
}

.user-info .location {
    color: var(--green);
    font-size: 0.875rem;
    font-weight: 600;
    margin-top: 6px;
}

ul {
    list-style-type: none;
    display: flex;
    flex-direction: column;
    width: 100%;
    gap: 15px;
}

ul a {
    display: block;
    padding: 12px 0;           /* ← 12px, not 10px */
    text-decoration: none;
    color: var(--white);
    background-color: var(--gray-700);
    border-radius: var(--btn-rounding);  /* ← 8px */
    font-weight: 700;
}

ul a:hover {
    color: var(--gray-900);
    background-color: var(--green);
}
```

---

## 💬 Final Thoughts

This is a solid challenge completion — the essentials are all there and the code is well-organised. The main areas to focus on going forward are:

1. **Semantic HTML** — lean on `<main>`, `<article>`, `<section>` for layout scaffolding rather than repurposing `<figure>`.
2. **Pixel accuracy on radii/padding** — these small visual details accumulate and are the difference between "close" and "pixel-perfect".
3. **`min-height` over `height`** on the body — a critical habit for robustness across content lengths and viewport sizes.

Keep going — the progression from challenge to challenge is clear and impressive! 🚀

---

*This feedback was generated by **Antigravity AI**. The challenge solution was written entirely by the developer — AI was only used for evaluation after the fact.*
