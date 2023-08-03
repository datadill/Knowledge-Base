---
description: 'Source: https://www.youtube.com/watch?v=xy9FEdic4vw'
---

# Math Functions

Clamp()

[https://geary.co/clamp-calculator/](https://geary.co/clamp-calculator/)

* REMs are great, but as you move down from desktop -> mobile.. you don't need all of that padding space
  * In-experienced people will edit each breakpoint
* Sets a minimum, maximum, and preferred range and scales fluidly between them
  * ex. clamp(min,preferred,max)
    * clamp(5rem, 3.125vw + 4rem, 10rem)
      * What is the preferred value?
* Great for padding, headings, text, etc



Min()

* Selects the minimum calculated value
* Behaves as if you're setting a **maximum** value
* Supports mixed units
* Supports nested calculations as a value
* Accepts two or three values, but most commonly two values
* **When to use: "I need this element to be sized dynamically, but it should never get larger than X value"**
* min(1366px, 100vw - 12rem)
  * On desktop, the content is max 1366 pixels
    * On mobile, it then selects the lesser of the two values so it takes the VW - 12rem

Max()

* **When to use: "I need this element to be sized dynamically, but it should never get smaller than X value"**

Calc()

* Perform basic or advanced math calculations
* Supports mixed units
* Is supported within color functions
* Is insanely helpful when using variables
* Is critical for creating "unbreakable" situations
* **Ex. Calc(100vh - 6rem)**
  * **Maybe you are making a hero page and you want to show the visitor 60 pixels of your next section to entice them to scroll down**
  * **The reason you do not use 94vh is because that value will adjust depending on the device**
