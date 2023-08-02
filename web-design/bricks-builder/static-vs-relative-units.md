---
description: 'Source: https://www.youtube.com/watch?v=cwfxZRLqyus'
---

# Static vs Relative Units

Pixels

* Static unit
* Not accessible (no font scaling)
* Should still be used for media queries
* "pixel perfect" design is a thing of the past
* Pixels can be used for media queries

REM

* Relative to the root font size (most browsers default to 16 px)
  * 1rem = 16px at 100% root font size
  * 1rem = 10px at 62.5% root font size
  * Best use for fixed values (e.g. 3rem of padding will always be equal to 30 pixels at normal font scaling)
  * Need to consider font scaling for things like border, padding, etc

EM

* Relative to the element's calculated font size
  * Think of it as a ratio (1.5rem = 1.5x the font size)
* Best used in situations where things need to respond to changes in calculated font size (e.g. buttons)
* Caution: EM units compound and are **influenced by the font size of parent containers**
  * This is why the REM unit was introduced -- EMs are often undesirable as a relative unit
* Don't use EMs for font sizes - it's what nightmares are made of!
* Ex. If you have a div that is 6rem pixels, then another container that has 2em.. that is 120 px.. and then another div with 1.5... you now end up with 180px

Percent

* Relative to the dimensions of the parent element
  * Ex. If a div is 100px wide, and the content is 50%, then the content will be 50px
* Be careful because as it scales down it can get very narrow

CH

* relative to the width of the "0" character of the current font
* Normal paragraph text should have a width of between 50 and 75 characters (ch)
* It is not an exact science, but it is very useful for dealing with text

Viewport Units

* Relative to the size of the viewport
  * ex. desktop, mobile, etc
* vw
  * 60vw is similar to 60%, but instead of being relative to the parent it is 60% of the viewport
* vh
* vmin
  * Ex. 60% of the smaller side so on a laptop that has a smaller y axis (screen size) a button with 60vmin will shrink vertically with the smallest size
* vmax
  * opposite of vmin
