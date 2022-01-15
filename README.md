# Kau Chim

This is a demo project for making a Kau Chim webpage.

## About the name

In China, Kau Chim (simplified Chinese: 求签) is a fortune telling practice in which people request answers from a sacred oracle lot.

Actually, there are many words to represent an activity like Kau Chim, like "ballot", "draw straws", and so on. But they tend to emphasize that a group of people choose one member of themselves to perform a task when none has volunteered for it. So I pick "Kau Chim" as the name of this demo.

## Demand analysis

There have been many webpages for Kau Chim, of which the operation process is to firstly display a picture on the webpage, then multiple flashed pictures will be displayed in turn after the user clicks on it. Finally it stays in one picture after the user clicks again, and that's the result of a Kau Chim.

## Implementation

Following [This page](https://mp.weixin.qq.com/s/f2DOJQS7BF2BeSxehn99dg), the demo implements Kau Chim with `<svg>` tags.

Scalable Vector Graphics (SVG) is an XML-based markup language developed by the World Wide Web Consortium (W3C) for describing two-dimensional based vector graphics. It's a text-based, open Web standard for describing images that can be rendered cleanly at any size and are designed specifically to work well with other web standards.

Let's get startted from the following code:

```html
<svg width="300" height="200" xmlns="http://www.w3.org/2000/svg">
  <rect width="100%" height="100%" fill="red" />
  <circle cx="150" cy="100" r="80" fill="green" />
  <text x="150" y="125" font-size="60" text-anchor="middle" fill="white">SVG</text>
</svg>
```

<svg width="300" height="200" xmlns="http://www.w3.org/2000/svg">
  <rect width="100%" height="100%" fill="red" />
  <circle cx="150" cy="100" r="80" fill="green" />
  <text x="150" y="125" font-size="60" text-anchor="middle" fill="white">SVG</text>
</svg>

We can see that, the background is set to red by drawing a rectangle `<rect>` that covers the complete image area, and A green circle `<circle>` with a radius of 80px is drawn atop the center of the red rectangle (center of circle offset 150px to the right, and 100px downward from the top left corner). At last, the text "SVG" is drawn. The interior of each letter is filled in with white. The text is positioned by setting an anchor where we want the midpoint to be: in this case, the midpoint should correspond to the center of the green circle. Fine adjustments can be made to the font size and vertical position to ensure the final result is aesthetically pleasing. As we can see, the order of rendering elements is affected by the order in which they are written, i.e., later elements are rendered atop previous elements.

To achieve the flashing effect of the pictures, we need to use animation functions of the `<svg>` tag. The `transform` attribute defines a list of transform definitions that are applied to an element and the element's children, for example, to scale, to rotate, to skew, and so on. For this demo, we use function `translate(<x> [<y>])` which moves the object by x and y.

In our case, we can flash the pictures by shifting the whole image frame by frame every so often, and limit the display area to a single frame by setting the `viewBox` attribute. The element rendered for the first frame is written at last to cover the flashing pictures, and once clicked, it moves away to show them.

Here is a simple demo:

```html
<svg height="100%" viewBox="0 0 200 200">
  <g transform="translate(0 -1200)">
    <g transform="translate(0 0)">
      <rect width="100%" height="200" y="0" fill="red" />
      <rect width="100%" height="200" y="200" fill="orange" />
      <rect width="100%" height="200" y="400" fill="yellow" />
      <rect width="100%" height="200" y="600" fill="green" />
      <rect width="100%" height="200" y="800" fill="blue" />
      <rect width="100%" height="200" y="1000" fill="purple" />
      <rect width="100%" height="200" y="1200" fill="black" />
    </g>
    <animateTransform attributeName="transform" type="translate" values="0 0;0 -200;0 -400;0 -600;0 -800;0 -1000;0 -1200" repeatCount="indefinite" fill="freeze" dur="1" calcMode="discrete" end="click" />
  </g>
  <g>
    <g>
      <rect width="100%" height="200" fill="white" />
      <text x="100" y="115" font-size="30" text-anchor="middle">Click to start</text>
    </g>
    <animateTransform attributeName="transform" type="translate" values="-200 0" calcMode="discrete" fill="freeze" begin="click" restart="never" />
  </g>
</svg>
```

<svg height="200" viewBox="0 0 200 200">
  <g transform="translate(0 -1200)">
    <g transform="translate(0 0)">
      <rect width="100%" height="200" y="0" fill="red" />
      <rect width="100%" height="200" y="200" fill="orange" />
      <rect width="100%" height="200" y="400" fill="yellow" />
      <rect width="100%" height="200" y="600" fill="green" />
      <rect width="100%" height="200" y="800" fill="blue" />
      <rect width="100%" height="200" y="1000" fill="purple" />
      <rect width="100%" height="200" y="1200" fill="black" />
    </g>
    <animateTransform attributeName="transform" type="translate" values="0 0;0 -200;0 -400;0 -600;0 -800;0 -1000;0 -1200" repeatCount="indefinite" fill="freeze" dur="1" calcMode="discrete" end="click" />
  </g>
  <g>
    <g>
      <rect width="100%" height="200" fill="white" />
      <text x="100" y="115" font-size="30" text-anchor="middle">Click to start</text>
    </g>
    <animateTransform attributeName="transform" type="translate" values="-200 0" calcMode="discrete" fill="freeze" begin="click" restart="never" />
  </g>
</svg>

When it comes to pictures instead of `svg` shapes, the implementation is similar. The code extracted from the link above is placed in the `src` folder.