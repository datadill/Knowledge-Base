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
6. Not using semantic HTML tags - Use headings, sections, footer tags etc so that screen readers can understand the page. In Oxygen, you can easily change the tags of a div for example.
7. Using divs to create lists - On the main div that contains the list, use a custom "ul" tag, and for each div make the tag a custom "li". Alternatively, you can use Rich Text module and insert the line items there, but the only downside of this is that it is hard to create an icon list.
8. Pixels instead of relative units -  Use REM and set your root font size to 62.5% so that 1 REM = 10px, 1.6 REM = 16px, etc.
9. Improper heading levels - If you want to make a heading smaller, lower the REM.. do NOT set the heading level to something lower as that will mess up SEO.
10. Setting heading size in global settings - In Oxygen, the problem with this is that you have no control of how responsive the text will be on mobile vs desktop. Instead, make classes for each heading and change the REM for each breakpoint. Alternatively, use a paid plugin like: [https://automaticcss.com/](https://automaticcss.com/)
11. Static images - Image URL vs Media Library.. typically you want to choose a size for the photo such as "medium" to keep the size small. Additionally, if you use SRCSET ([https://www.youtube.com/watch?v=0jc74V5wYRk](https://www.youtube.com/watch?v=0jc74V5wYRk)) images it will automatically load smaller images if it detects the device size only requires that size.
12. Background vs Real images - do not use background images if the image is important because there is no SEO value and screen readers do not see it.
13. Not removing pagination - people try to hide the pagination button with DISPLAY: none, but this still gets indexed and that is bad.
14. Not using custom post types for queryable items - create custom post types similar to how WooCommerce creates Products
15. No 404 template



