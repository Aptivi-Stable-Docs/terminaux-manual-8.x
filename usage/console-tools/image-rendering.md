---
description: Images in the terminal!
icon: image
---

# Image Rendering

In addition to all of the features that Terminaux provides, it also provides an extra library that your terminal applications can use, called `Terminaux.Images`, that supports Windows, macOS, and Linux. It doesn't use [`System.Drawing`](https://learn.microsoft.com/en-us/dotnet/core/compatibility/core-libraries/6.0/system-drawing-common-windows-only) due to its exclusivity to Windows.

{% hint style="info" %}
You don't need to install ImageMagick to your system; it's embedded in the Magick.NET package!

You can also use the `OpenImage()` function, which returns a `MagickImage` instance. It accepts either a file path, an array of bytes, or a stream.
{% endhint %}

***

## <mark style="color:$primary;">Rendering images to the console</mark>

With the help of ImageMagick, you can render the images to your console, as long as your console's dimensions is greater than the requested width and height of the image.

Using the `ImageProcessor` class, you can either use a file path, a byte array, a `MagickImage` instance, or a stream that contains image data that ImageMagick can process, which you can find a list of supported extensions [here](https://imagemagick.org/script/formats.php).

Here are the examples of how to render a picture to column 5 row 3 of the console with the image width of 40 and height of 20 using various methods:

<details>

<summary>Using a file path</summary>

{% code title="Using a file path" lineNumbers="true" %}
```csharp
string rendered = ImageProcessor.RenderImage("path/to/file", 40, 20, 4, 2);
TextWriterRaw.WriteRaw(rendered);
```
{% endcode %}

</details>

<details>

<summary>Using a byte stream</summary>

{% code title="Using a byte stream" lineNumbers="true" %}
```csharp
byte[] bytes = File.ReadAllBytes("path/to/file");
string rendered = ImageProcessor.RenderImage(bytes, 40, 20, 4, 2);
TextWriterRaw.WriteRaw(rendered);
```
{% endcode %}

</details>

<details>

<summary>Using a stream</summary>

{% code title="Using a stream" lineNumbers="true" %}
```csharp
var imageStream = File.OpenRead("path/to/file");
string rendered = ImageProcessor.RenderImage(imageStream, 40, 20, 4, 2);
TextWriterRaw.WriteRaw(rendered);
```
{% endcode %}

</details>

<details>

<summary>Using a <code>MagickImage</code> instance</summary>

{% code title="Using a MagickImage instance" lineNumbers="true" %}
```csharp
MagickImage myMagickImage = (...)
string rendered = ImageProcessor.RenderImage(myMagickImage, 40, 20, 4, 2);
TextWriterRaw.WriteRaw(rendered);
```
{% endcode %}

</details>

You can demonstrate this by running the `Terminaux.Console` app (build from source) and running the `RenderImage` test fixture:

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
In some cases, you don't actually need to specify the left and the top position of the upper left corner of the image, such as interactive TUIs that print text and informational boxes.
{% endhint %}

***

## <mark style="color:$primary;">Extra features</mark>

There are some of the extra features that Terminaux brings:

<details>

<summary>Obtaining individual colors each pixel</summary>

You can get the individual colors each pixel uses using the `GetColorsFromImage()` function which returns a two-dimensional array of Terminaux's `Color` instance that represents each pixel, where the first dimension specifies the column, and the second one specifies the row. You can also optionally specify a width and a height to override the built-in image dimensions.

If you don't want to provide an image file, stream, or array of bytes, or if you're making a UI concept, you can use the placeholder graphic renderer that can be easily be used like this:

{% code title="Rendering a placeholder" lineNumbers="true" %}
```csharp
string rendered = ImageProcessor.RenderImage(40, 20, 4, 2);
TextWriterRaw.WriteRaw(rendered);
```
{% endcode %}

{% hint style="info" %}
You can optionally specify a background color that the image renderer will use to render a transparent image with the background color that you've specified by passing a `Color` instance to the last argument of the `RenderImage()` functions. If not specified, it'll use your current background color.
{% endhint %}

</details>

<details>

<summary>Rendering icons to the terminal</summary>

As of Terminaux 4.2.0, it provides a method to render icons. They represent symbols that describe something, such as computer peripherals and books, to describe a thing or two using just the icons. The `IconsManager` class provides you with icon management tools that allow you to render the icons and get their names.

{% hint style="warning" %}
They're not suitable for icons inside the text, because it uses pictures. If you still want to use them, use the text-based emojis instead.
{% endhint %}

In order to be able to use the icons in your interactive applications, you can get the value from the `RenderIcon()` function, specifying the icon name, the width and the height of your icon, and the position of where do you want your icon to be rendered. This value can then be printed using the conventional `TextWriterRaw.WriteRaw()` function.

You can choose one of the two following properties:

* Color: Colored or Black

The icon selector allows you to interactively select an icon while showing it to you at the same time to review your icon choice. This can be accessed in the `IconsSelector` class.

</details>

<details>

<summary>Using the image viewer TUI</summary>

<figure><img src="../../.gitbook/assets/image (165).png" alt=""><figcaption></figcaption></figure>

The image viewer TUI can be used to give your console applications an image preview interface that allows you to peek in a specific image, just like conventional image viewers in your PC, your phone, and so on.&#x20;

In order to be able to use this feature, you must open an image to get the `MagickImage` object instance containing data about your picture before being able to use this viewer. A simple example of doing this, assuming that you have a stream of some picture called `stream`, is:

```csharp
var image = ImageProcessor.OpenImage(stream);
ImageViewInteractive.OpenInteractive(image);
```

{% hint style="info" %}
You can use the arrow keys to move around the image, page up/down keys to move up and down quickly, and home/end keys to go to the beginning and the ending, respectively. Those keys are enabled only if you turn on the "Scaled" mode using the <kbd>F</kbd> key.
{% endhint %}

</details>
