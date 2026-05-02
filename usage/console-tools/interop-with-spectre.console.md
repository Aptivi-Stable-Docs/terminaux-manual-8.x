---
description: Can Terminaux and Spectre.Console be together?
icon: ghost
---

# Interop with Spectre.Console

If you are using [Spectre.Console](https://www.nuget.org/packages/Spectre.Console), you might notice that there are some features that are roughly similar to what Terminaux implements if we're talking about ideas and concepts. While Spectre.Console focuses on CLI, Terminaux focuses on both CLI and TUI. However, the graphical part of Spectre.Console, such as table rendering, is implemented differently in both libraries.

In a separate library, [Terminaux.Spectre](https://www.nuget.org/packages/Terminaux.Spectre/), you can perform translation operations.

<figure><img src="../../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Please note that, when installing this package, `Spectre.Console` is also installed as a dependency.
{% endhint %}

***

## <mark style="color:$primary;">Translation</mark>

You can translate Terminaux's renderables into Spectre.Console's renderables, but you can't translate the other way around due to how Spectre.Console implements them. The following translation methods are supported:

<table><thead><tr><th width="242.3333740234375">Source</th><th width="265">Direction</th><th>Target</th></tr></thead><tbody><tr><td><code>Mark</code></td><td>Terminaux -> Spectre.Console</td><td><a href="https://spectreconsole.net/markup"><code>Markup</code></a></td></tr><tr><td><code>BoxFrame</code></td><td>Terminaux -> Spectre.Console</td><td><a href="https://spectreconsole.net/widgets/panel"><code>Panel</code></a></td></tr><tr><td><code>Table</code></td><td>Terminaux -> Spectre.Console</td><td><a href="https://spectreconsole.net/widgets/table"><code>Table</code></a></td></tr><tr><td><code>BarChart</code></td><td>Terminaux -> Spectre.Console</td><td><a href="https://spectreconsole.net/widgets/barchart"><code>BarChart</code></a></td></tr><tr><td><code>BreakdownChart</code></td><td>Terminaux -> Spectre.Console</td><td><a href="https://spectreconsole.net/widgets/breakdownchart"><code>BreakdownChart</code></a></td></tr><tr><td><code>Calendars</code></td><td>Terminaux -> Spectre.Console</td><td><a href="https://spectreconsole.net/widgets/calendar"><code>Calendar</code></a></td></tr><tr><td><code>AlignedFigletText</code></td><td>Terminaux -> Spectre.Console</td><td><a href="https://spectreconsole.net/widgets/figlet"><code>FigletText</code></a></td></tr><tr><td><code>Canvas</code></td><td>Terminaux -> Spectre.Console</td><td><a href="https://spectreconsole.net/widgets/canvas"><code>Canvas</code></a></td></tr><tr><td><code>TextPath</code></td><td>Terminaux -> Spectre.Console</td><td><a href="https://spectreconsole.net/widgets/text-path"><code>TextPath</code></a></td></tr><tr><td><code>TextAlignment</code></td><td>Terminaux -> Spectre.Console</td><td><code>Justify</code></td></tr><tr><td><code>Justify</code></td><td>Spectre.Console -> Terminaux</td><td><code>TextAlignment</code></td></tr><tr><td><code>Color</code></td><td>Terminaux -> Spectre.Console</td><td><a href="https://spectreconsole.net/appendix/colors"><code>Color</code></a></td></tr><tr><td><a href="https://spectreconsole.net/appendix/colors"><code>Color</code></a></td><td>Spectre.Console -> Terminaux</td><td><code>Color</code></td></tr></tbody></table>

{% hint style="info" %}
You can't use Terminaux's writers to write the resulting Spectre's [`IRenderable`](https://spectreconsole.net/api/spectre.console.rendering/irenderable/). You'll have to use the [`AnsiConsole`](https://spectreconsole.net/api/spectre.console/ansiconsole/) class.
{% endhint %}
