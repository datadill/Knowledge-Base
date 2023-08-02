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
