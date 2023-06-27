# <Heading>Preflight</Heading>
An opinionated set of base styles (from Tailwind CSS).

## <Heading hidden>Overview</Heading>

Built on top of [modern-normalize](https://github.com/sindresorhus/modern-normalize), Preflight is a set of base styles that are designed to smooth over cross-browser inconsistencies and make it easier for you to work within the constraints of your design system.

While most of the styles in Preflight are meant to go unnoticed — they simply make things behave more like you'd expect them to — some are more opinionated and can be surprising when you first encounter them.

For a complete reference of all the styles applied by Preflight, [see the stylesheet](https://unpkg.com/preflightcss/preflight.css).

---

## Default margins are removed

Preflight removes all of the default margins from elements like headings, blockquotes, paragraphs, etc.

```css
blockquote,
dl,
dd,
h1,
h2,
h3,
h4,
h5,
h6,
hr,
figure,
p,
pre {
  margin: 0;
}
```

This makes it harder to accidentally rely on margin values applied by the user-agent stylesheet that are not part of your spacing scale.

---

## Headings are unstyled

All heading elements are completely unstyled by default, and have the same font-size and font-weight as normal text.

```css
h1,
h2,
h3,
h4,
h5,
h6 {
  font-size: inherit;
  font-weight: inherit;
}
```

The reason for this is two-fold:

- **It helps you avoid accidentally deviating from your type scale**. By default, browsers assign sizes to headings that don't exist in Preflight's default type scale, and aren't guaranteed to exist in your own type scale.
- **In UI development, headings should often be visually de-emphasized**. Making headings unstyled by default means any styling you apply to headings happens consciously and deliberately.

---

## Lists are unstyled

Ordered and unordered lists are unstyled by default, with no bullets/numbers and no margin or padding.

```css
ol,
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
```

### Accessibility considerations

Unstyled lists are [not announced as lists by VoiceOver](https://unfetteredthoughts.net/2017/09/26/voiceover-and-list-style-type-none/). If your content is truly a list but you would like to keep it unstyled, [add a "list" role](https://www.scottohara.me/blog/2019/01/12/lists-and-safari.html) to the element:

```html
<ul role="list">
  <li>One</li>
  <li>Two</li>
  <li>Three</li>
</ul>
```

---

## Images are block-level

Images and other replaced elements (like `svg`, `video`, `canvas`, and others) are `display: block` by default.

```css
img,
svg,
video,
canvas,
audio,
iframe,
embed,
object {
  display: block;
  vertical-align: middle;
}
```

This helps to avoid unexpected alignment issues that you often run into using the browser default of `display: inline`.

If you ever need to make one of these elements `inline` instead of `block`, simply use the `inline` utility:

```html
<img class="inline" src="..." alt="...">
```

---

## Images are constrained to the parent width

Images and videos are constrained to the parent width in a way that preserves their intrinsic aspect ratio.

```css
img,
video {
  max-width: 100%;
  height: auto;
}
```

This prevents them from overflowing their containers and makes them responsive by default. If you ever need to override this behavior, use the `max-w-none` utility:

```html
<img class="max-w-none" src="..." alt="...">
```

---

## Border styles are reset globally

In order to make it easy to add a border by simply adding the `border` class, Tailwind overrides the default border styles for all elements with the following rules:

```css
*,
::before,
::after {
  border-width: 0;
  border-style: solid;
  border-color: theme('borderColor.DEFAULT', currentColor);
}
```

Since the `border` class only sets the `border-width` property, this reset ensures that adding that class always adds a solid 1px border using your configured default border color.

This can cause some unexpected results when integrating certain third-party libraries, like Google maps for example.

When you run into situations like this, you can work around them by overriding the Preflight styles with your own custom CSS:

```css
.google-map * {
  border-style: none;
}
```