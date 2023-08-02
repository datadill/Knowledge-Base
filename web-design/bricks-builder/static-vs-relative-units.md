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
* Caution: EM units compound and are influenced by the font size of parent containers
  * This is why the REM unit was introduced -- EMs are often undesirable as a relative unit
* Don't use EMs for font sizes - it's what nightmares are made of!
