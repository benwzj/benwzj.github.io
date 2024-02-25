---
layout: post
title: Some Concepts in HTML5
date: 2024-02-24
category: HTML
tags: HTML CSS
---

## Viewport

The viewport is the user's visible area of a web page and is often used when talking about mobile devices.

A more specific meaning of the term viewport refers to a meta element in HTML 5, which plays a crucial role in mobile optimization. The element scales the displayed content so that the size of the screen can be used efficiently. 

In this case, the meta element viewport ensures that all content is equally legible and displayed correctly and completely on screens of different sizes. The viewport element adapts web pages to the screen’s length and width so that mobile browsers can display the entire content correctly.

Thanks to the viewport, websites on mobile devices are not displayed in the same way as on a desktop screen. Users do not have to zoom in but can view the content of a page in a way that matches the small display. Viewports as meta elements (in combination with responsive web design) help web browsers to break up the pages and reassemble them on small screens in a way that enables users to receive a meaningful and readable image. Viewports thus have the task of preventing display problems by determining output formats that are tailored to the respective mobile device.

### Setting The Viewport
HTML5 introduced a method to let web designers take control over the viewport, through the `<meta>` tag.
You should include the following `<meta>` viewport element in all your web pages:
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
This gives the browser instructions on how to control the page's dimensions and scaling.

- The `width=device-width` part sets the width of the page to follow the screen-width of the device (which will vary depending on the device).
- The `initial-scale=1.0` part sets the initial zoom level when the page is first loaded by the browser.

> Tip: If you are browsing page without this `meta` in a phone, you will have bad experience.

If you specify the viewport in a CSS file instead, you should put it right at the beginning of the file to ensure a correct display. Here's an example of how that code could look like:
```css
@viewport {
width: device-width;
}
```

### CSS Viewport Units

- `vh` stands for viewport height. This unit is based on the height of the viewport. A value of 1vh is equal to 1% of the viewport height. A value of 100vh is equal to 100% of the viewport height.
- `vw` stands for viewport width. This unit is based on the width of the viewport. A value of 1vw is equal to 1% of the viewport width.
- `vmin` stands for viewport minimum. This unit is based on the smaller dimension of the viewport. If the viewport height is smaller than the width, the value of 1vmin will be equal to 1% of the viewport height. Similarly, if the viewport width is smaller than the height, the value of 1vmin will be equal to 1% of the viewport width.
- `vmax` stands for viewport maximum. This unit is based on the larger dimension of the viewport. If the viewport height is larger than the width, the value of 1vmax will be equal to 1% of viewport height. Similarly, if the viewport width is larger than the height, the value of 1vmax will be equal to 1% of the viewport width.

