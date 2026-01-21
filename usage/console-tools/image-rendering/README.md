---
description: Images in the terminal!
icon: image
---

# Image Rendering

In addition to all of the features that Terminaux provides, it also provides an extra library that your terminal applications can use, called `Terminaux.Images`, that supports Windows, macOS, and Linux. It doesn't use [`System.Drawing`](https://learn.microsoft.com/en-us/dotnet/core/compatibility/core-libraries/6.0/system-drawing-common-windows-only) due to its exclusivity to Windows.

With the help of ImageMagick, you can render the images to your console, as long as your console's dimensions is greater than the requested width and height of the image. Using the `ImageProcessor` class, you can either use a file path, a byte array, a MagickImage instance, or a stream that contains image data that ImageMagick can process, which you can find a list of suppored extensions [here](https://imagemagick.org/script/formats.php).

Here are the examples of how to render a picture to column 5 row 3 of the console with the image width of 40 and height of 20:

{% code title="Using a file path" lineNumbers="true" %}
```csharp
string rendered = ImageProcessor.RenderImage("path/to/file", 40, 20, 4, 2);
TextWriterRaw.WriteRaw(rendered);
```
{% endcode %}

{% code title="Using a byte stream" lineNumbers="true" %}
```csharp
byte[] bytes = File.ReadAllBytes("path/to/file");
string rendered = ImageProcessor.RenderImage(bytes, 40, 20, 4, 2);
TextWriterRaw.WriteRaw(rendered);
```
{% endcode %}

{% code title="Using a stream" lineNumbers="true" %}
```csharp
var imageStream = File.OpenRead("path/to/file");
string rendered = ImageProcessor.RenderImage(imageStream, 40, 20, 4, 2);
TextWriterRaw.WriteRaw(rendered);
```
{% endcode %}

{% code title="Using a MagickImage instance" lineNumbers="true" %}
```csharp
MagickImage myMagickImage = (...)
string rendered = ImageProcessor.RenderImage(myMagickImage, 40, 20, 4, 2);
TextWriterRaw.WriteRaw(rendered);
```
{% endcode %}

{% hint style="info" %}
You don't need to install ImageMagick to your system; it's embedded in the Magick.NET package!

You can also use the `OpenImage()` function, which returns a `MagickImage` instance. It accepts either a file path, an array of bytes, or a stream.
{% endhint %}

You can demonstrate this by running the `Terminaux.Console` app (build from source) and running the `RenderImage` test fixture:

<figure><img src="../../../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
In some cases, you don't actually need to specify the left and the top position of the upper left corner of the image, such as interactive TUIs that print text and informational boxes.
{% endhint %}

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

## Image Viewer TUI

<figure><img src="../../../.gitbook/assets/image (165).png" alt=""><figcaption></figcaption></figure>

The image viewer TUI can be used to give your console applications an image preview interface that allows you to peek in a specific image, just like conventional image viewers in your PC, your phone, and so on.&#x20;

In order to be able to use this feature, you must open an image to get the `MagickImage` object instance containing data about your picture before being able to use this viewer. A simple example of doing this, assuming that you have a stream of some picture called `stream`, is:

```csharp
var image = ImageProcessor.OpenImage(stream);
ImageViewInteractive.OpenInteractive(image);
```

{% hint style="info" %}
You can use the arrow keys to move around the image, page up/down keys to move up and down quickly, and home/end keys to go to the beginning and the ending, respectively. Those keys are enabled only if you turn on the "Scaled" mode using the <kbd>F</kbd> key.
{% endhint %}
