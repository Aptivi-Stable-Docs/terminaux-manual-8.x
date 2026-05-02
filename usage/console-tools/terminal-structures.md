---
description: What do they represent?
icon: table-tree
---

# Terminal Structures

Terminaux provides some useful structures that help you make console applications seamlessly and without repetition, such as coordinates, margins, and padding.

They are found in the `Terminaux.Base.Structures` namespace, and they use structs to represent values as they are informational.

***

## <mark style="color:$primary;">Types of structures</mark>

Terminaux provides different types of structures that are useful for your console applications. Expand one of the sections to learn more.

<details>

<summary>Coordinates</summary>

A structure that defines a coordinate relative to the console that starts with the upper left corner (0, 0) is called `Coordinate`. It contains two properties:

<table><thead><tr><th width="109.6666259765625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>X</code></td><td>Specifies the left position as specified by the X axis of the console</td></tr><tr><td><code>Y</code></td><td>Specifies the top position as specified by the Y axis of the console</td></tr></tbody></table>

This allows you to store coordinate information in one variable without having to resort to tuples or multiple variables.

</details>

<details>

<summary>Size</summary>

A structure that defines a width and a height of an element is called `Size`. It contains two properties:

<table><thead><tr><th width="109.6666259765625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>Width</code></td><td>Width of the element (horizontal length)</td></tr><tr><td><code>Height</code></td><td>Height of the element (vertical length)</td></tr></tbody></table>

This allows you to store size information in one variable without having to resort to tuples or multiple variables.

</details>

<details>

<summary>Padding</summary>

A structure that defines padding information related to the element, such as the console, is called `Padding`.

The default padding is (0, 0, 0, 0) which represents the left, the top, the right, and the bottom padding.

It contains the following properties:

<table><thead><tr><th width="109.6666259765625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>Left</code></td><td>Left padding starting from 0</td></tr><tr><td><code>Top</code></td><td>Top padding starting from 0</td></tr><tr><td><code>Right</code></td><td>Right padding starting from 0</td></tr><tr><td><code>Bottom</code></td><td>Bottom padding starting from 0</td></tr></tbody></table>

This allows you to store padding information in one variable without having to resort to tuples or multiple variables.

{% hint style="info" %}
This structure is a group of two separate structures that describe both horizontal and vertical padding, which will be described below.
{% endhint %}

### <mark style="color:$primary;">Vertical Padding</mark>

A structure that describes vertical padding (that is, top and bottom padding) is called `VerticalPad`, which the `Padding` structure uses internally.

The default padding is (0, 0) which represents the top and the bottom padding.

It contains the following properties:

<table><thead><tr><th width="109.6666259765625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>Top</code></td><td>Top padding starting from 0</td></tr><tr><td><code>Bottom</code></td><td>Bottom padding starting from 0</td></tr></tbody></table>

### <mark style="color:$primary;">Horizontal Padding</mark>

A structure that describes horizontal padding (that is, left and right padding) is called `HorizontalPad`, which the `Padding` structure uses internally.

The default padding is (0, 0) which represents the left and the right padding.

It contains the following properties:

<table><thead><tr><th width="109.6666259765625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>Left</code></td><td>Left padding starting from 0</td></tr><tr><td><code>Right</code></td><td>Right padding starting from 0</td></tr></tbody></table>

</details>

<details>

<summary>Margin</summary>

A structure that defines the margin of an element is called `Margin`. It specifies the width and the height of the element, which makes this struct a measurement structure. It measures the margin values according to the padding values given.

It contains the following properties:

<table><thead><tr><th width="109.6666259765625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>Width</code></td><td>Processed width with margin padding applied</td></tr><tr><td><code>Height</code></td><td>Processed height with margin padding applied</td></tr></tbody></table>

This allows you to store margin information in one variable without having to resort to tuples or multiple variables.

{% hint style="info" %}
This structure is a group of two separate structures that describe both horizontal and vertical margins, which will be described below.
{% endhint %}

### <mark style="color:$primary;">Vertical Margin</mark>

A structure that describes vertical margin is called `VerticalPad`, which the `Margin` structure uses internally. It's a measurement structure that returns a total height when both the top and the bottom margins are applied.

It contains the following properties:

<table><thead><tr><th width="109.6666259765625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>Height</code></td><td>Processed height with margin padding applied</td></tr></tbody></table>

### <mark style="color:$primary;">Horizontal Margin</mark>

A structure that describes horizontal margin is called `HorizontalPad`, which the `Margin` structure uses internally. It's a measurement structure that returns a total width when both the left and the right margins are applied.

It contains the following properties:

<table><thead><tr><th width="109.6666259765625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>Width</code></td><td>Processed width with margin padding applied</td></tr></tbody></table>

</details>

<details>

<summary>Rectangle</summary>

A structure that defines the rectangle and its coordinates for each corner is called `Rect`. It specifies the four coordinates for each corner, as well as the rectangle width and height, starting from the upper left corner.

For example, if a rectangle is located at (2,1), and the size is 8x5, the following properties will be set:

* `Coordinate`: 2,1
* `CoordinateEnd`: 10,6
* `CoordinateUpperRight`: 10,1
* `CoordinateLowerLeft`: 2,6
* `Size`: 8x5

This helps in determining the positions of the corners without having to perform complex calculations.

</details>
