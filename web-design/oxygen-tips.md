---
description: Oxygen is a developer focused, low-code page builder built on Wordpress.
---

# Oxygen Tips

20 Mistakes to Avoid&#x20;

Source: [https://www.youtube.com/watch?v=FgjO4fJybZs\&t=13s](https://www.youtube.com/watch?v=FgjO4fJybZs\&t=13s)



1. Using IDs instead of classes to style elements
2. Random class naming - If you have a card for example that contains a heading, text, button.. start off by giving the div a class name of "card" and then for every element inside prefix the class name with "card". For example: card\_\_btn, card\_\_heading. If you want to make a dark version of the card, use "--". For example: card--featured could put a shadow on one of the cards to make it stand out.
3. Not locking classes - Once something is the way you like it, go to Advanced -> Lock Selector Styles
4. Using the columns module - Columns module uses Flexbox, but CSS Grid is better for laying out content. Flexbox does have an advantage in positioning content.
5. Putting everything in one section
6. Not using semantic HTML tags - Use headings, sections, etc so that screen readers can access the page. In Oxygen, you can easily change the tags of a div for example.



