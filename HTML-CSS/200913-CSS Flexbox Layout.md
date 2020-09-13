# CSS Flexbox Layout

![my-img](img/200913-1.png)

The two most important elements in flexbox layout are flex containers and flex items.

The flex container sets the context for flexbox layout; it contains flex items, the actual elements you layout using flexbox.

Every direct child of a flex container is called a flex item; there can be any number of flex items inside a flex container.

Once the children are flex items, you can take advantage of flexbox's powerful alignment properties.

Flexbox follows two axes that determine the layout direction of flex items: main axis and cross axis.

The main axis is the primary axis along which flex items are laid out; it defines the direction of flex items in the flex container.

The cross axis runs perpendicular to the main axis.

Each axis has a start side and an end side.

The default main-start and main-end direction of the main axis is left-to-right.

The the default direction of the cross axis is top-to-bottom.

---

Before you can use any flexbox property, you need to define a flex container in your layout.

You create a flex container by setting the display property of an element to one of the flexbox layout values: flex or inline-flex.

By default, flex items are laid out horizontally on the main axis from left to right.

By default, flex items stretch to fill the flex container's height.

display: flex; creates a block-level flex container.

display: inline-flex; creates an inline flex container.

Any element outside a flex container is not affected by the flexbox layout.

Only children of a flex container automatically become flex items.

---

Some flexbox properties apply to the flex container only, while some apply only to the flex items.

The flex-direction property applies to the flex container only.

The default value for flex-direction is row.

To reverse the direction flex items in a row, use the value row-reverse.

The value column rotates the main axis so that flex items are laid out vertically.

Like the row-reverse property, you can swap the top-to-bottom direction of a column with the value column-reverse.

---

The flex-wrap property is for flex containers only.

The flex container lays out flex items on a single line called a flex line.

The flex container tries to fit all items on one flex line, even if causes its contents to overflow.

The flex container can break flex items into multiple flex lines and allow them to wrap as needed.

With the flex-wrap property, you can control whether the flex container is a single-line or multi-line layout.

The value wrap breaks the flex items into multiple lines.

---

You apply the justify-content property to flex containers only.

The justify-content property lets you control the position and alignment of flex Items on the main axis and how space should be distributed in a flex container.

The default value for justify-content is flex-start, which places items towards the start of each flex line.

To place items at the end of the flex line, set justify-content to flex-end.

The value center places flex items in the center of the line, with equal amounts of empty space between the line's start edge and the first item.

The value space-between displays equal spacing between flex items.

For equal spacing around every flex item, use the value space-around.

A margin set to auto will absorb any extra space around a flex item and push other flex items into different positions.

```css
.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}
.item-1 {
  margin-right: auto;
}
```

---

The order property applies to flex items only.

We can use the order property to change the order of any flex item.

You can structure an HTML document for SEO or accessibility first, then rearrange the content without ever editing the HTML.

The default order of all flex items is 0.

order places flex items relative to the other items' order values.

To place a flex item before another item, it needs to have a lower order value than the item.

To place a flex item after another item, it needs to have a higher order value than the item.

---

The flex-grow property applies to flex items only.

flex-grow determines how much of the available space inside the flex container an item should take up; it assigns more or less space to flex items.

A flex-grow value of 1 expands flex items to take up the full space of a line.

The higher the flex-grow value, the more an item grows relative to the other items.

---

flex and flex-basis apply to flex items only.

flex-basis specifies the initial main size of a flex item.

You set the initial size you want the flex items to be, then flexbox evenly distributes the free space according that size.

flex is the shorthand for flex-grow, flex-basis and flex-shrink.

Using only one number value for flex sets the flex-grow value of an item.

The second and third values are optional in the flex shorthand.

Setting only one number value for flex automatically sets the flex-basis value to 0.

---

The align-items property applies to flex containers only.

The align-self property applies to flex items only.

align-items aligns flex items vertically in the flex container.

To align all flex items to the start of the cross axis, use the align-items: flex-start;.

align-items: flex-end; packs the items toward the end of the cross axis.

align-items: center; perfectly centers items along the cross axis.

align-self: flex-start; aligns a flex item to the start of the cross axis.

align-self: flex-end; aligns a flex item to the end of the cross axis.

align-self: center; aligns a flex item to the center of the cross axis.

---

Three ways to center-align elements using flexbox

Set the flex container's justify-content and align-items values to center:

```css
.container {
  justify-content: center;
  align-items: center;
}
```

Set the flex container's justify-content value to center, while setting the flex item's align-self value to center:

```css
.container {
  justify-content: center;
}
.item {
  align-self: center;
}
```

Set the flex item's margin value to auto:

```css
.item {
  margin: auto;
}
```

---

It's possible for an element to be both a flex item and a flex container.

Giving .main-header a justify-content: space-between; declaration positions the site name on the left side of the header and the navigation menu on the right:

```css
.main-header {
  justify-content: space-between;
}
```

You can assign an equal amount of space to columns with the flex-grow and flex properties.

You can use flexbox to make responsive column calculations less complex.

Use the flex-basis property to set the initial size you want the column items to be.

It's possible for an element to be both a flex item and a flex container.

A margin value of auto has an effect on flex item alignment because it absorbs any extra space around a flex item and pushes other flex items into different positions.

---

When you make body a flex container, it lays out all its direct children horizontally on a single line.

Setting the flex-direction of body to column stacks its flex items vertically.

1vh is equal to 1/100th or 1% of the viewport height.

Sticky footer snippet

```css
body {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.row {
  flex: 1;
}
```

Alternate sticky footer method

```css
body {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.main-footer {
  margin-top: auto;
}
```
