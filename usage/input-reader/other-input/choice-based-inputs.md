---
description: Select your choice
icon: pen
---

# Choice-based inputs

Terminaux provides a choice-based input style group that has several styles that present the choices in different shapes, such as informational boxes and selection styles. These inputs simplify the way you present your choices for applications to perform a specific action based on a selected choice.

{% hint style="info" %}
You can group instances of `InputChoiceInfo` classes using an array of `InputChoiceCategoryInfo` instances that provide the category name and a list of choice group instances, `InputChoiceGroupInfo`, that also provide the group name and a list of input choices.
{% endhint %}

***

## <mark style="color:$primary;">Choices</mark>

<figure><img src="../../../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

Input choices are required to make use of all choice-based input methods that use the `InputChoiceInfo` class instances, which you can define them yourself.

You can define them using the following variables in a constructor:

* Required variables
  * Choice name: Name of the choice, which is usually an index number.
  * Choice title: Title of the choice that summarizes the choice.
* Optional variables
  * Choice description: Description of the choice that describes even further about the choice and what it represents.
  * Choice default: Determines whether this choice is a default choice or not.
  * Choice default selected: Determines whether the choice is a selected choice by default for multiple choice selections or not.
  * Choice disabled: Determines whether this choice is a disabled choice or not.

{% hint style="warning" %}
There may be only one default choice, but in case you have defined multiple choices as default, the first choice that has been set as default is selected as default, ignoring all the other defaults. A disabled choice may not be a default choice.
{% endhint %}

***

## <mark style="color:$primary;">Array of input choices</mark>

In general, if you have an array of choices that is represented either as a tuple of both the choice name and the choice title, or as an array of choice title names, you can use the `InputChoiceTools` class to generate an array of `InputChoiceInfo` instances quickly.

* `GetInputChoices(string[] Answers)`
* `GetInputChoices((string, string)[] Answers)`

***

## <mark style="color:$primary;">All choice-based inputs</mark>

All inputs that use the `InputChoiceInfo` instances are mentioned here:

<details>

<summary>Normal choice style</summary>

This is the simplest choice style that doesn't use any kind of screen or fancy aesthetics to provide you with the choices. This style is found in the `ChoiceStyle` class, but it only supports single answer. This style presents the choices in the following forms:

<table><thead><tr><th width="119.6666259765625">Type</th><th>Description</th></tr></thead><tbody><tr><td>Single line</td><td>Presents you with a question and a list of choices in a single line.</td></tr><tr><td>Multi line</td><td>Presents you with a question and a list of choices in two lines.</td></tr><tr><td>Modern</td><td>Presents you with a question and a list of choices in a modern way.</td></tr></tbody></table>

### <mark style="color:$primary;">Answer methods</mark>

This style supports either answers that require a single letter to be pressed or answers that require more than a letter, which is what `PressEnter` in a `ChoiceStyleSettings` instance does.

### <mark style="color:$primary;">Choice name display methods</mark>

Here's how will the normal choice style use the `InputChoiceInfo` instance in terms of required variables:

* Choice names represent the answers that you'll have to answer with.
* Choice titles are shown only in the modern choice style.

### <mark style="color:$primary;">Choice customization</mark>

You can customize the choice style using an instance of `ChoiceStyleSettings`, which you can easily make an instance of. This class comes with the following settings:

| Settings           |                                                                                                                                                                                                                         |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Choice output type | One line, two lines, or modern.                                                                                                                                                                                         |
| Press Enter        | Enable if your choice names have multiple letters. It allows the input to consist of multiple characters.                                                                                                               |
| Color settings     | <p>Consists of properties that allow you to change the style colors:</p><p></p><ul><li>Question color</li><li>Input color</li><li>Option color</li><li>Alternative option color</li><li>Disabled option color</li></ul> |

{% hint style="info" %}
Alternative answers are separate from the main answers, and are completely optional.
{% endhint %}

</details>

<details>

<summary>Choice-based infoboxes</summary>

You can access information from here:

<a href="../../console-tools/console-writers/informational-boxes.md" class="button primary">Informational boxes</a>

</details>

<details>

<summary>Selection style</summary>

<figure><img src="../../../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

Selection style input method uses choices to present you with a full-screen interactive choice selector powered by the [textual UI](../../console-tools/textual-ui/) feature. This style allows you to select a choice interactively. This style is found in the `SelectionStyle` and the `SelectionMultipleStyle` classes for both the single-choice selection style and the multiple-choice selection style, respectively.

{% hint style="info" %}
All selection style answers return the following:

* A zero-based answer index if it's a single selection in Terminaux 8.1 or higher, and a one-based answer number if it's a single selection in Terminaux 8.0 or lower.
* A zero-based array of answer numbers if it's a multiple selection.

Write your code accordingly to avoid errors.
{% endhint %}

### <mark style="color:$primary;">Customization</mark>

Your selection style is customizable, because these classes contain a settings argument that uses `SelectionStyleSettings`. You can easily make a class instance out of it to customize the selection style to match your application's aesthetics. This class contains a singleton property that serves as a global settings for the selection input settings.

This class currently contains the following settings:

| Property              | Description                                          |
| --------------------- | ---------------------------------------------------- |
| `RadioButtons`        | Enables the radio buttons for single-selection style |
| `Title`               | Selection style border title                         |
| `QuestionColor`       | Color of the question text                           |
| `SliderColor`         | Color of the slider                                  |
| `InputColor`          | Color of the input text                              |
| `OptionColor`         | Color of the option text                             |
| `AltOptionColor`      | Color of the alternative option text                 |
| `SelectedOptionColor` | Color of the selected option text                    |
| `DisabledOptionColor` | Color of the disabled option text                    |
| `SeparatorColor`      | Color of the separator                               |
| `TextColor`           | Color of the text                                    |

### <mark style="color:$primary;">Tips for selection input</mark>

Here are some tips that apply to selection style inputs:

* The overloads that contain a settings argument let you pass your custom settings to the current selection style invocation, while the overloads that don't contain this parameter use the global settings.
* In multiple choice selections, you can press `A` to select all the choices. You can also press `F` to initiate a regular expression search for long choice selections to point you quickly to the desired choice.
* You can press `P` to show the current choice selection count and the current page count, as well as total choices and pages.
*   If you want to get more information about an item that you've selected, either press `TAB`, or open the sidebar using the `S` key and navigate through it using the `E` and the `D` keys.

    <img src="../../../.gitbook/assets/image (39).png" alt="" data-size="original">

</details>

{% hint style="danger" %}
All input styles that use choices must require that at least one of the choices is enabled by default. Otherwise, an exception will be thrown.
{% endhint %}
