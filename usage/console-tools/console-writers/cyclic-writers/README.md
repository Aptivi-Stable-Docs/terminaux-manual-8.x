---
description: Such writers render repeatedly with or without some movement
icon: arrow-rotate-right
---

# Cyclic Writers

Cyclic writers are dynamic writers that can be rendered individually by making a new class instance of a renderable class that implements the base cyclic writer class, `CyclicWriter`, and one of the typed cyclic writer classes that you can implement in your renderable class.

Such writers can either be animated or static, and can be rendered either by calling their individual `Render()` function one by one or by putting renderable classes to a container and calling that container's `WriteContainer()` function from the `ContainerTools` class.

***

## <mark style="color:$primary;">Implementation</mark>

In cyclic writers, the implementation usually implements the base cyclic writer class and one of the following classes:

| Class                   | Description                                                                                                                        |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `GraphicalCyclicWriter` | This cyclic writer utilizes positioning and supports widths and heights for graphical terminal applications                        |
| `SimpleCyclicWriter`    | This cyclic writer simply writes the rendered output to the console and is suitable for simple and scrolling terminal applications |

{% hint style="info" %}
Most of the renderables also support colors, in which you can set with `UseColors` and their appropriate properties.
{% endhint %}

***

## <mark style="color:$primary;">Available cyclic writers</mark>

The following built-in cyclic writers are available:

<details>

<summary>Shapes</summary>

Shapes are one of the cyclic writers that are available. The following shapes are available:

<table><thead><tr><th width="149.66668701171875">Class</th><th>Description</th></tr></thead><tbody><tr><td><code>Circle</code></td><td>Renders a full circle to the console</td></tr><tr><td><code>Arc</code></td><td>Renders an arc (pie, semicircle, ellipsis, etc.) to the console</td></tr><tr><td><code>Ellipsis</code></td><td>Renders a full ellipsis to the console</td></tr><tr><td><code>Parallelogram</code></td><td>Renders a parallelogram to the console</td></tr><tr><td><code>Rectangle</code></td><td>Renders a rectangle to the console</td></tr><tr><td><code>Square</code></td><td>Renders a square to the console</td></tr><tr><td><code>Trapezoid</code></td><td>Renders a trapezoid to the console</td></tr><tr><td><code>Triangle</code></td><td>Renders a triangle to the console</td></tr><tr><td><code>Line</code></td><td>Renders a line to the console</td></tr></tbody></table>

To learn more about geometric shapes, consult the page below:

<a href="geometric-shapes.md" class="button primary">Geometric Shapes</a>

</details>

<details>

<summary>Charts</summary>

Charts are one of the cyclic writers that are available. The following charts are available:

<table><thead><tr><th width="149.66668701171875">Class</th><th>Description</th></tr></thead><tbody><tr><td><code>BreakdownChart</code></td><td>Renders a breakdown chart to the console</td></tr><tr><td><code>BarChart</code></td><td>Renders a bar chart to the console</td></tr><tr><td><code>StickChart</code></td><td>Renders a stick chart to the console</td></tr><tr><td><code>StemLeafChart</code></td><td>Renders a stem leaf chart to the console</td></tr><tr><td><code>LineChart</code></td><td>Renders a line chart to the console</td></tr><tr><td><code>LinesChart</code></td><td>Renders a chart of lines to the console</td></tr><tr><td><code>PieChart</code></td><td>Renders a pie chart to the console</td></tr><tr><td><code>WinsLosses</code></td><td>Renders a wins and losses chart to the console</td></tr></tbody></table>

To learn more about charts, consult the page below:

<a href="charts.md" class="button primary">Charts</a>

</details>

<details>

<summary>Text</summary>

Text renderers are one of the cyclic writers that are available. The following text renderers are available:

<table><thead><tr><th width="175">Class</th><th>Description</th></tr></thead><tbody><tr><td><code>AlignedFigletText</code></td><td>Renders an aligned figlet text to the console</td></tr><tr><td><code>AlignedText</code></td><td>Renders an aligned text to the console</td></tr><tr><td><code>AnimatedText</code></td><td>Renders an animated text to the console</td></tr><tr><td><code>BoundedText</code></td><td>Renders a bounded text to the console</td></tr><tr><td><code>FigletText</code></td><td>Renders a figlet text to the console</td></tr><tr><td><code>PowerLine</code></td><td>Renders a PowerLine segment to the console</td></tr><tr><td><code>TextMarquee</code></td><td>Renders a text marquee to the console</td></tr><tr><td><code>Decoration</code></td><td>Renders a decoration to the console</td></tr><tr><td><code>SyntaxText</code></td><td>Renders a syntax highlighed text to the console</td></tr><tr><td><code>TextPath</code></td><td>Renders a decorated text path to the console</td></tr></tbody></table>

To learn more about text renderers, consult the page below:

<a href="text.md" class="button primary">Text</a>

</details>

<details>

<summary>Artistic</summary>

Artistic renderers are one of the cyclic writers that are available. The following artistic renderers are available:

<table><thead><tr><th width="149.66668701171875">Class</th><th>Description</th></tr></thead><tbody><tr><td><code>Border</code></td><td>Renders a border to the console</td></tr><tr><td><code>Box</code></td><td>Renders a box to the console</td></tr><tr><td><code>BoxFrame</code></td><td>Renders a box with border to the console</td></tr><tr><td><code>Canvas</code></td><td>Renders a canvas to the console</td></tr><tr><td><code>AnimatedCanvas</code></td><td>Renders an animated canvas to the console</td></tr></tbody></table>

To learn more about artistic renderers, consult the page below:

<a href="artistic.md" class="button primary">Artistic</a>

</details>

<details>

<summary>Progress Bars</summary>

Progress bars are one of the cyclic writers that are available. The following progress bars are available:

<table><thead><tr><th width="179.66668701171875">Class</th><th>Description</th></tr></thead><tbody><tr><td><code>ProgressBar</code></td><td>Renders a progress bar with text and percentage to the console</td></tr><tr><td><code>ProgressBarNoText</code></td><td>Renders a progress bar with percentage to the console</td></tr><tr><td><code>SimpleProgress</code></td><td>Renders a progress bar to the console</td></tr><tr><td><code>Slider</code></td><td>Renders a slider to the console</td></tr><tr><td><code>Spinner</code></td><td>Renders a spinner to the console</td></tr></tbody></table>

To learn more about progress bars, consult the page below:

<a href="progress-bars.md" class="button primary">Progress Bars</a>

</details>

<details>

<summary>Lists and Calendars</summary>

Lists and calendars are one of the cyclic writers that are available. The following lists and calendars are available:

<table><thead><tr><th width="170.3333740234375">Class</th><th>Description</th></tr></thead><tbody><tr><td><code>Table</code></td><td>Renders a simple table to the console</td></tr><tr><td><code>Calendars</code></td><td>Renders a calendar table to the console</td></tr><tr><td><code>Listing</code></td><td>Renders a list of elements to the console</td></tr><tr><td><code>ListEntry</code></td><td>Renders a list element to the console</td></tr><tr><td><code>Selection</code></td><td>Renders an active selection element to the console</td></tr><tr><td><code>PassiveSelection</code></td><td>Renders a passive selection element to the console</td></tr></tbody></table>

To learn more about lists and calendars, consult the page below:

<a href="lists-and-calendars.md" class="button primary">Lists and Calendars</a>

</details>

<details>

<summary>Miscellaneous</summary>

Miscellaneous renderers are one of the cyclic writers that are available. The following renderers are available:

<table><thead><tr><th width="170.3333740234375">Class</th><th>Description</th></tr></thead><tbody><tr><td><code>Eraser</code></td><td>Erases part of the screen in the console</td></tr><tr><td><code>Keybindings</code></td><td>Renders a list of keybindings in one line to the console</td></tr><tr><td><code>KeyShortcut</code></td><td>Renders a key shortcut part in one line to the console</td></tr><tr><td><code>Emoji</code></td><td>Renders an emoji to the console</td></tr><tr><td><code>Kaomoji</code></td><td>Renders a Kaomoji to the console</td></tr><tr><td><code>NerdFonts</code></td><td>Renders a Nerd Fonts typeface to the console</td></tr></tbody></table>

To learn more about those renderers, consult the page below:

<a href="miscellaneous.md" class="button primary">Miscellaneous</a>

</details>

***

## <mark style="color:$primary;">Renderables and Containers</mark>

While a single cyclic writer class instance represents a single writer, containers store a group of cyclic writer class instances.

<details>

<summary>Renderables</summary>

Each of the cyclic writers listed above are classes that can be created to a new variable. Those classes are called renderables.

{% hint style="info" %}
A renderable can be rendered either to the string (for buffering using `RenderRenderable()`) or to the console (with no buffering using `WriteRenderable()`) in the `RendererTools` class, depending on the context.
{% endhint %}

</details>

<details>

<summary>Containers</summary>

When you create a group of renderables, they are called containers. They can be rendered or written directly to the console.

{% hint style="info" %}
A container can be rendered either to the string (for buffering using `RenderContainer()`) or to the console (with no buffering using `WriteContainer()`) in the `ContainerTools` class, depending on the context.
{% endhint %}

You can define a container by creating a new instance of the `Container` class and adding some of the renderables that can be identified by their name.

In addition to that, you can manipulate with a renderable using the following functions:

| Function                  | Description                                                     |
| ------------------------- | --------------------------------------------------------------- |
| `SetRenderablePosition()` | Sets the renderable position                                    |
| `SetRenderableSize()`     | Sets the renderable size                                        |
| `IsRegistered()`          | Checks to see if a renderable with this ID is registered or not |
| `RemoveRenderable()`      | Removes a renderable with this ID                               |
| `GetRenderable()`         | Gets a renderable instance from this ID                         |
| `GetRenderablePosition()` | Gets a renderable instance position from this ID                |
| `GetRenderableSize()`     | Gets a renderable instance size from this ID                    |
| `GetRenderableNames()`    | Gets an array of renderable IDs                                 |

{% hint style="info" %}
Some of the renderables may override the position variable for a renderable and may use the values from the renderables' properties.

You can, however, control this behavior by setting any of the `ignoreSetPositions` (ignores the renderable positions and sets them to 0,0) and `ignoreSetSize` (ignores the renderable sizes and sets them to the maximum console dimensions) arguments.
{% endhint %}

</details>

***

## <mark style="color:$primary;">Customization</mark>

Some of the console writers allow customization, such as the bordered boxes or progress bars.

<details>

<summary>Borders</summary>

You can customize the borders for some of the console writers that support borders by editing properties found in the global settings of the borders, which can be found in the `BorderSettings.GlobalSettings` property.

You can pass your own instance of `BorderSettings` with your own customizations to the border and its properties to the supported writers and all the informational boxes.

The following writers support editing the border settings:

* `BorderColor`
* `BorderTextColor`
* `BoxFrameColor`
* `ProgressBarColor`
* `ProgressBarVerticalColor`
* `SliderColor`
* `SliderVerticalColor`
* ...and much more

{% hint style="info" %}
Pre-defined borders can be accessed by calling one of the properties found in the `PredefinedBorders` class.
{% endhint %}

</details>

<details>

<summary>Padding and margins</summary>

For margins, in all cyclic writers that derive from the `GraphicalCyclicWriter` class, you can use the following properties:

* `Padding`: Object padding to create blank space around the selected areas in cells.
* `Margin`: Object margin to reduce the width and the height based on the margin values.

All graphical cyclic writers use the `SetMargin` property to set the appropriate position and size in accordance to the two above property values.

</details>
