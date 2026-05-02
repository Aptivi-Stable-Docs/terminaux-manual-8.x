---
description: Information in a box
icon: square-info
---

# Informational Boxes

When you want to present information to the user in a form of a dialog box, you'll want to use this feature that presents you with a wide variety of informational box styles to choose from, depending on the purpose.

***

## <mark style="color:$primary;">Types of info boxes</mark>

Some informational box styles are modal, while others are non-modal.

Modal infoboxes are informational boxes that are displayed on the terminal, waiting for user input. However, non-modal infoboxes are boxes that are just displayed for informational purposes, such as reporting a progress non-deterministically. Selection-based inputs can be searched using regular expressions compatible with the .NET syntax.

The following informational box styles are presented in this way:

<table><thead><tr><th width="109.6666259765625">Modality</th><th width="159.6666259765625">Type</th><th>Description</th></tr></thead><tbody><tr><td>Modal</td><td>Buttons</td><td>Info box that tells users to choose one of three buttons</td></tr><tr><td></td><td>Input</td><td>Info box that tells users to write a string in clear text</td></tr><tr><td></td><td>Input (password)</td><td>Info box that tells users to write a string with masking</td></tr><tr><td></td><td>Input (character)</td><td>Info box that tells users to write a single character</td></tr><tr><td></td><td>Multi-input</td><td>Info box that tells users to provide multiple inputs</td></tr><tr><td></td><td>Modal dialog box</td><td>Info box that tells users to read text in it before exiting</td></tr><tr><td></td><td>Slider</td><td>Info box that tells users to select an integral value visually</td></tr><tr><td></td><td>Selection</td><td>Info box that tells users to select an option</td></tr><tr><td></td><td>Multi selection</td><td>Info box that tells users to select multiple options</td></tr><tr><td>Non-modal</td><td>Dialog box</td><td>Info box that shows users a text box</td></tr><tr><td></td><td>Progress</td><td>Info box that shows users a text box with progress</td></tr></tbody></table>

***

## <mark style="color:$primary;">Info box configuration</mark>

You can configure the infoboxes and how they appear and/or behave using the `InfoBoxSettings` class. You can access the global settings using the `GlobalSettings` property.

This class contains the following properties:

<table><thead><tr><th width="159.99993896484375">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>Title</code></td><td>A short title to describe the infobox</td></tr><tr><td><code>BorderSettings</code></td><td>Controls how the borders are rendered</td></tr><tr><td><code>ForegroundColor</code></td><td>Foreground color of the infobox (including border, title, and content)</td></tr><tr><td><code>BackgroundColor</code></td><td>Background color of the infobox</td></tr><tr><td><code>UseColors</code></td><td>Whether to use the colors or not (default is <code>true</code>)</td></tr><tr><td><code>RadioButtons</code></td><td>Whether to use the radio buttons or not (for single-choice selection infoboxes)</td></tr><tr><td><code>Positioning</code></td><td>Determines the positioning of the infobox</td></tr><tr><td><code>UsePopover</code></td><td>Uses the popover for multi-input selection infoboxes (input modules should handle popovers)</td></tr></tbody></table>

{% hint style="info" %}
Until Terminaux 7.0, you can still use the argument-based overloads when making a new infobox. Those overloads, however, are deprecated and you should use the `InfoBoxSettings` instance instead.
{% endhint %}

***

## <mark style="color:$primary;">Info box class</mark>

All informational boxes, modal or non-modal, utilizes the `InfoBox` class to specify how to write the informational box to the console, such as positioning, text, and settings.

Non-modal informational boxes can also be used as a pseudo-cyclic writer because it has a function called `Render()`, which behaves similarly to that of a cyclic writer by returning a buffer that would get printed to a console as a string.

This function allows you to provide the following arguments:

<table><thead><tr><th width="139.99993896484375">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>increment</code></td><td>Incrementation rate for paged text in the text area (usually passed initialized to 0)</td></tr><tr><td><code>currIdx</code></td><td>Current index of text line in the text area</td></tr><tr><td><code>drawBar</code></td><td>Whether to draw the slider bar for the text area or not</td></tr><tr><td><code>writeBinding</code></td><td>Whether to write the key bindings in the upper right corner of the box or not</td></tr></tbody></table>

You can also erase them manually if you have an info box instance using the `Erase()` function. Non-modal informational boxes can also be erased manually, because the functions return the info box instance, which allows you to erase the infobox using the `Erase()` function. You'll need to print the result to the console, though, using the raw console writer.

{% hint style="info" %}
If you provided increment as a ref variable, all the arguments that come after it will be required.
{% endhint %}

***

## <mark style="color:$primary;">Modal informational boxes</mark>

Informational box styles of this nature are modal. They require either a keyboard input or a mouse input from the end user in order to present information to the user with action. These boxes are common in interactive console applications where information is to be conveyed to the user.

<details>

<summary>Buttons</summary>

<figure><img src="../../../.gitbook/assets/image (175).png" alt=""><figcaption></figcaption></figure>

This style utilizes choice-based input to render the buttons inside the informational box of this style, which makes them look like a conventional dialog box.

This style can render up to three buttons from the right to the left, which means three instances of `InputChoiceInfo`. You can use this style with the `InfoBoxButtonsColor` class.

</details>

<details>

<summary>Input</summary>

<figure><img src="../../../.gitbook/assets/image (176).png" alt=""><figcaption></figcaption></figure>

This style uses the input reader to tell the user to write something, based on the informational box contents.

For example, if this informational box tells an end-user to write a hostname of an SFTP server, the user will have to write the hostname.

You can use this style with `InfoBoxInputColor`.

</details>

<details>

<summary>Input (password)</summary>

<figure><img src="../../../.gitbook/assets/image (177).png" alt=""><figcaption></figcaption></figure>

This style uses the input reader masked with the password to tell the user to write something, based on the informational box contents.

For example, if this informational box tells an end-user to write a password of an account when logging in to the SFTP server, the user will have to write the password.

You can use this style with `InfoBoxInputColor` with specifying the `InfoBoxInputType` as `Password`.

</details>

<details>

<summary>Input (character)</summary>

<figure><img src="../../../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

This style uses the input reader that takes the first character from the input string, based on the informational box contents.

For example, if this informational box tells an end-user to write a character to specify a box border character, the user will have to write it.

You can use this style with `InfoBoxInputColor` with specifying the `InfoBoxInputType` as `Character`.

</details>

<details>

<summary>Multi-input</summary>

<figure><img src="../../../.gitbook/assets/image (179).png" alt=""><figcaption></figcaption></figure>

This style uses the input modules to describe multiple ways to present input to the user using different modules, such as having a text box and a combo box at the same time.

You can use this style with `InfoBoxMultiInputColor`.

{% hint style="info" %}
To learn more about input modules, consult the page below:

<a href="../../input-reader/other-input/input-modules.md" class="button primary">Input Modules</a>
{% endhint %}

</details>

<details>

<summary>Normal modal info box</summary>

<figure><img src="../../../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>

This style only prints information that the end user needs to read. This is suitable for information that doesn't need any action.

You can use this style with the `InfoBoxModalColor` class.

</details>

<details>

<summary>Slider</summary>

<figure><img src="../../../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

This style allows you to define the minimum, current, and maximum value of an integral value that is surrounded by the specified range.

<p align="center"><span class="math">x_{min} \le value \le x_{max}</span></p>

For example, a slider that has a minimum value of 5 and a maximum value of 10 can only accept values of between 5 and 10, which means 5, 6, 7, 8, 9, and 10.

You can use this style with the `InfoBoxSliderColor` class.

### <mark style="color:$primary;">Controls</mark>

You can use left arrow to decrement a value by 1, and you can use right arrow to increment a value by 1.

If you're holding the SHIFT key while pressing the mentioned arrow keys, you'll decrement or increment a value by 100.

You can use the spacebar to manually enter the value, in case using arrow keys is unfeasible even with SHIFT.

</details>

<details>

<summary>Selection styles</summary>

These are the informational boxes that use choice-style input styles.

## <mark style="color:$primary;">Infobox styles</mark>

There are two styles in this category:

* Single selection
* Multiple selection

### <mark style="color:$primary;">Single selection informational box</mark>

<figure><img src="../../../.gitbook/assets/image (181).png" alt=""><figcaption></figcaption></figure>

This info box style uses the choice-based input to define choices that the end user will have to select one of them. Plus, you can search for a choice using the `F` key to initiate a regex-based search for maximum flexibility.

You can use this style with the `InfoBoxSelectionColor` class.

{% hint style="info" %}
You can additionally enable radio buttons using the `RadioButtons` property in the infobox settings.

<img src="../../../.gitbook/assets/image (182).png" alt="" data-size="original">
{% endhint %}

### <mark style="color:$primary;">Multiple selection informational box</mark>

<figure><img src="../../../.gitbook/assets/image (183).png" alt=""><figcaption></figcaption></figure>

This info box style uses the choice-based input to define multiple choices that the end user will have to select one of them. Plus, you can search for a choice using the `F` key to initiate a regex-based search for maximum flexibility.

You can use this style with the `InfoBoxSelectionMultipleColor` class.

## <mark style="color:$primary;">Defining choices</mark>

For informational boxes that use choice-based input styles, you can discover how to define choices by visiting this page:

<a href="../../input-reader/other-input/choice-based-inputs.md" class="button primary">Choice-based inputs</a>

</details>

{% hint style="info" %}
Every single modal informational box style contain their own help page which you can access via the `K` key. They support both keyboard and mouse.
{% endhint %}

{% hint style="info" %}
If you want to create a custom modal infobox not covered in any of the below pre-defined infobox styles, you'll have to refer to their [source code](https://github.com/Aptivi/Terminaux/tree/main/public/Terminaux/Inputs/Styles/Infobox) and create the style handler yourself.
{% endhint %}

***

## <mark style="color:$primary;">Non-modal informational boxes</mark>

In addition to the modal informational boxes, we also have non-modal informational boxes that don't wait for user input, but gets rendered once per call.

<details>

<summary>Normal infobox</summary>

<figure><img src="../../../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

This is used to convey information to the end user that some progress is being made without any percentage. Such progress is called indeterminate progress. However, this can be used to display a disclaimer or any other information within a limited time.

</details>

<details>

<summary>Progress</summary>

<figure><img src="../../../.gitbook/assets/image (184).png" alt=""><figcaption></figcaption></figure>

This is used to tell the end user through an informational box with a progress bar that something is happening, and that the progress is being done all the way to 100%. This infobox needs to be in a loop while progress is being made.

</details>

***

## <mark style="color:$primary;">Infobox positioning</mark>

All informational boxes support positioning, because they expose a writable property called `Positioning` in the `InfoBoxSettings` class. It can be configured globally using the `GlobalSettings` property found in the `InfoBoxPositioning` class.

The following properties can be used to position the informational box however you want:

<table><thead><tr><th width="130.6666259765625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>Autofit</code></td><td>If turned on, the informational box will be automatically placed in the middle of the screen, growing according to the textual content. If turned off, the informational box will be rendered based on the below positioning values.</td></tr><tr><td><code>Left</code></td><td>Zero-based left position of the infobox</td></tr><tr><td><code>Top</code></td><td>Zero-based top position of the infobox</td></tr><tr><td><code>Width</code></td><td>Width of the infobox (excluding the extra width) (default: <code>50</code>)</td></tr><tr><td><code>Height</code></td><td>Height of the infobox (excluding the extra height) (default: <code>5</code>)</td></tr><tr><td><code>ExtraWidth</code></td><td>Reserved width for the informational box that may store wider elements than the text itself (keep zero for text only)</td></tr><tr><td><code>ExtraHeight</code></td><td>Reserved height for elements that will be placed after the information text (keep zero for text only)</td></tr></tbody></table>

Turning off autofit for infoboxes, while keeping default configuration, will result in this:

<figure><img src="../../../.gitbook/assets/image (174).png" alt=""><figcaption></figcaption></figure>
