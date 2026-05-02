---
description: Inner workings of the help system
icon: question
---

# Help System

The help system is what every shell uses, invoked by the unified `help` command available for every type of shell. The function responsible for showing help entries or command usage and some extra help is `HelpSystem.ShowHelp()` in the `Terminaux.Shell.Commands` namespace.

***

## <mark style="color:$primary;">General help</mark>

When the user requests the `help` command in any shell, the above function gets called, querying the current shell type (`Shell.CurrentShellType`) as discussed previously.

The function gets all the commands, including the unified ones, from the command type, but hides all the hidden commands (commands that are flagged with the `CommandFlags.Hidden` flag). It also takes care of other commands, such as aliases.

The help system then prints the list of commands to the console.

{% hint style="info" %}
Some of the shells implement too many commands, potentially requiring you to either wrap its output using the `wrap` command, or turn on the simplified help using the `-simplified` switch. You can also show hidden commands using the `-help` switch.
{% endhint %}

***

## <mark style="color:$primary;">Command help</mark>

If the user specified a command when calling the `help` command, the help system extracts the help definition and all the command usages.

It extracts from these values found in the `CommandInfo` class:

<table><thead><tr><th width="190">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>HelpDefinition</code></td><td>The brief summary of what the command does</td></tr><tr><td><code>CommandArgumentInfo</code></td><td>The command argument info that supplies the command argument information</td></tr></tbody></table>

The `CommandArgumentInfo` property supplies the following properties:

<table><thead><tr><th width="149.33331298828125">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>Arguments</code></td><td>Defines the command arguments</td></tr><tr><td><code>Switches</code></td><td>Defines the command switches</td></tr><tr><td><code>RenderedUsage</code></td><td>Defines the rendered usage to be printed for a command</td></tr></tbody></table>

The rendered usage has the following characteristics:

* For required switches, they're surrounded with the `<` and the `>` marks. Switches that are not required are surrounded with the `[` and the `]` marks instead.
* Switches that require values don't have the `[` and the `]` marks surrounding the `=value` part, indicating that the switch needs a value.
* For required arguments, they're surrounded with the `<` and the `>` marks. Arguments that are not required are surrounded with the `[` and the `]` marks instead.
* For numeric arguments, a marker is shown indicating that it only accepts numbers.
* For switches that conflict, they're put in a group, such as `[-switch1|-switch2]`.

***

## <mark style="color:$primary;">Exporting list of commands to Markdown</mark>

To export a list of commands to the Markdown table format, you'll need to use the `-markdown` switch with the help command, as long as you don't pass a command to it, in order to obtain a group of tables that contain a list of commands.

This is going to help you with documenting a list of commands in your project documentation.

{% hint style="info" %}
You can also programmatically export a list of commands to the Markdown group of tables using the `HelpExporter` class.
{% endhint %}
