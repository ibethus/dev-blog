---
title: 'The Power of SVG for the Web'
date: 2025-02-03T18:20:06+02:00
draft: false
---

 <style>
    a:hover{
        text-decoration: underline;
    }
</style>

## Introduction

SVG images (Scalable Vector Graphics) are widely used on the web due to their ability to scale without losing quality. They are XML-based files that can be easily included and modified in a web page.  
In this article, we will explore how to add HTML links to these images, dynamically change their style with CSS, and finally how to make them _responsive_.

## Adding HTML Links to SVG Images

Adding HTML links to an SVG image is very simple. Just wrap any SVG element with the `<a>` tag to make it clickable:

```html
<svg width="100%" height="100%" viewBox="0 0 100 100" preserveAspectRatio="xMidYMid meet" xmlns="http://www.w3.org/2000/svg">
    <rect x="0" y="0" width="100" height="100" fill="lightblue"/>
    <a href="https://example.com">
        <rect x="25" y="25" width="50" height="50" fill="lightgreen"/>
        <text x="50%" y="50%" text-anchor="middle" dominant-baseline="middle" fill="white" font-size="5px">Click me!</text>
    </a>
</svg>
```
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
    <rect x="0" y="0" width="200" height="200" fill="lightblue"/>
    <a href="https://example.com">
        <rect x="50" y="50" width="100" height="100" fill="lightgreen"/>
        <text x="50%" y="50%" text-anchor="middle" dominant-baseline="middle" fill="white" font-size="10px">Click me!</text>
    </a>
</svg>

## Dynamically Changing the Style of SVG Images

Similarly, you can use standard CSS selectors to apply styles to each element of the SVG, for example, to change a color. Note that the SVG's style is often embedded directly within the tags by the SVG creation software (Im using [photopea.com](photopea.com)).

```html
<svg version="1.2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 400" width="400" height="400">	
    <style>
        .bubble { fill: #bc2424 } 
    </style>
    <path id="line1" class="line" d="m284.6 126.1l-0.9 4.9-197.7-35.7 0.9-4.9z"/>
    <path id="line2" class="line" d="m281.5 128.9l4.3 2.5-88.6 153.4-4.3-2.5z"/>
    <path id="line3" class="line" d="m82 100l3.5-3.5 105.8 192.5-3.6 3.5z"/>
    <path id="circle1" class="bubble" d="m87.5 127c-18 0-32.5-14.5-32.5-32.5 0-18 14.5-32.5 32.5-32.5 18 0 32.5 14.5 32.5 32.5 0 18-14.5 32.5-32.5 32.5z"/>
    <path id="circle2" class="bubble" d="m282 151c-14.4 0-26-11.6-26-26 0-14.4 11.6-26 26-26 14.4 0 26 11.6 26 26 0 14.4-11.6 26-26 26z"/>
    <path id="circle3" class="bubble" d="m187 342c-27.7 0-50-22.4-50-50 0-27.6 22.3-50 50-50 27.7 0 50 22.4 50 50 0 27.6-22.3 50-50 50z"/>
</svg>
<style>
    .bubble:hover {
        fill: lightblue;
    }
</style>
```

<svg version="1.2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 400" width="400" height="400">	
    <style>
        .bubble { fill: #bc2424 } 
    </style>
    <path id="line1" class="line" d="m284.6 126.1l-0.9 4.9-197.7-35.7 0.9-4.9z"/>
    <path id="line2" class="line" d="m281.5 128.9l4.3 2.5-88.6 153.4-4.3-2.5z"/>
    <path id="line3" class="line" d="m82 100l3.5-3.5 105.8 192.5-3.6 3.5z"/>
    <path id="circle1" class="bubble" d="m87.5 127c-18 0-32.5-14.5-32.5-32.5 0-18 14.5-32.5 32.5-32.5 18 0 32.5 14.5 32.5 32.5 0 18-14.5 32.5-32.5 32.5z"/>
    <path id="circle2" class="bubble" d="m282 151c-14.4 0-26-11.6-26-26 0-14.4 11.6-26 26-26 14.4 0 26 11.6 26 26 0 14.4-11.6 26-26 26z"/>
    <path id="circle3" class="bubble" d="m187 342c-27.7 0-50-22.4-50-50 0-27.6 22.3-50 50-50 27.7 0 50 22.4 50 50 0 27.6-22.3 50-50 50z"/>
</svg>
<style>
    .bubble:hover {
        fill: lightblue;
    }
</style>

## Making SVG Images Responsive

SVG is a format that easily adapts to screen size constraints.

To make an SVG image responsive, use two attributes: `viewBox` and `preserveAspectRatio`. These allow you to create SVGs that adapt gracefully to any screen size.

Let's look at this example:

```html
<svg width="100%" height="100%" viewBox="0 0 100 100" preserveAspectRatio="xMidYMid meet" xmlns="http://www.w3.org/2000/svg">
    <a href="https://example.com">
        <rect x="10" y="10" width="80" height="80" fill="lightblue" />
        <text x="50" y="50" text-anchor="middle" dominant-baseline="middle" fill="white" font-size="10px">Click me!</text>
    </a>
</svg>
```

<svg width="100%" height="100%" viewBox="0 0 100 100" preserveAspectRatio="xMidYMid meet" xmlns="http://www.w3.org/2000/svg">
    <a href="https://example.com">
        <rect x="10" y="10" width="80" height="80" fill="lightblue" />
        <text x="50" y="50" text-anchor="middle" dominant-baseline="middle" fill="white" font-size="10px">Click me!</text>
    </a>
</svg>

> Try resizing the window to see the SVG adapt.

In this code, `viewBox="0 0 100 100"` defines our "SVG world" - a virtual canvas of 100×100 units where our elements will live, regardless of the actual size on the screen.

As for the attribute `preserveAspectRatio="xMidYMid meet"`, it ensures that our SVG always remains well-proportioned.

## Conclusion: SVG, Your Best Ally for Modern Web Design

SVG is the ultimate image format for the web! Let's recap why you should adopt it today:

- **Interactivity**: Easily add links, clickable areas, and user interactions.
- **Dynamic Styling**: Modify colors, shapes, and transitions directly via CSS.
- **100% Responsive**: An image that perfectly adapts to all screens, from smartphones to 4K displays.
- **Super Lightweight**: Files that are much smaller than equivalent PNG or JPG files.

Unlike traditional bitmap formats, SVG gives you full control over every element of your image. Imagine being able to change the color of a logo on hover, animate a chart, or make certain parts clickable - all without touching Photoshop!

Ready to get started? Try integrating an interactive SVG into your next project, and you'll see the difference. Your users (and your portfolio) will thank you! 😉