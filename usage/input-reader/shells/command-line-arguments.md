---
description: Arguments for the entry point
icon: square-code
---

# Command-Line Arguments

An application can be invoked either without the command-line arguments or with such arguments, depending on the application used. Some of them require arguments, and some do not. All arguments that are to be parsed will have to be prepended with a prefix, `--`. For example, if you want to pass `noparse` as an argument, use `--noparse`. Those arguments will be parsed upon calling the argument parser.

The argument parser can be found in the `ArgumentParse` class with the `ParseArguments()` function. You can typically call that in the application entry point, but you'll have to specify the list of arguments with their `ArgumentInfo` class instances. Here's when argument structure comes into play.

## Creating the argument info class

To make a dictionary of arguments, which will be required by several command-line argument tools, you'll have to first create a list of arguments, just like this:

```csharp
private static readonly Dictionary<string, ArgumentInfo> arguments = new()
{
    { "verbose", new("verbose", "Enables verbose mode", new VerboseArgument()) }
};
```

To create an `ArgumentExecutor` class, you'll have to create a deriving class first, with the minimal structure being:

```csharp
internal class VerboseArgument : ArgumentExecutor, IArgument
{
    public override void Execute(ArgumentParameters parameters)
    {
        // Some argument code
    }
}
```

After defining this class, you can create a new instance out of it and add it to the dictionary as shown above. ArgumentInfo allows you to use the two constructors defined below to define a new instance of the argument info class:

```csharp
public ArgumentInfo(string Argument, string HelpDefinition, ArgumentExecutor? ArgumentBase, bool Obsolete = false)
public ArgumentInfo(string Argument, string HelpDefinition, CommandArgumentInfo[]? ArgArgumentInfo, ArgumentExecutor? ArgumentBase, bool Obsolete = false)
```

Specify an argument without the `--` marks, a help definition, and a new instance of your newly-defined argument executor class. In addition to that, you can optionally specify an array of argument information classes, whose structure is already explained in the below page:

{% content-ref url="shell-structure/command-information.md" %}
[command-information.md](shell-structure/command-information.md)
{% endcontent-ref %}

## Help system

Command-line arguments also have a help system built in, because users may need a quick reference to the available arguments for a specific program. For this purpose, the help system is available at `ArgumentHelpPrint`.

{% hint style="info" %}
If you are distributing an application which contains arguments, it would be wise to implement a `--help` argument where it allows users to get a quick overview of available arguments. Optionally, you can add an argument to show a help entry for an argument that prints the name, the description, and the rendered usages.
{% endhint %}
