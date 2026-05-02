---
description: What have you run last time?
icon: hourglass-end
---

# Shell History

The shell history is another MESH shell feature that allows you to recall the last commands that you've executed in the current and in the previous sessions. This is accompanied by a file that shows you a history of commands executed in all the sessions.

***

## <mark style="color:$primary;">Structure</mark>

That file is called `<ShellName>.json` under the configuration directory:

* Windows: `%LOCALAPPDATA%\Aptivi\Terminaux\Histories`
* Linux and macOS: `$HOME/.config/Aptivi/Terminaux/Histories`

The file has this format: (Note that this varies based on your input)

```json
{
  "HistoryName": "General",
  "HistoryEntries": [
    "Shell",
    "yes",
    "yes",
    "yes",
    "ye",
    "yes",
    "yes"
  ]
}
```

***

## <mark style="color:$primary;">Manipulating with shell history</mark>

Accessing the shell history is easy; just press the up arrow button on your keyboard to recall the previous command executed. You can also press the down arrow button to recall the next command in the whole history.

In the `ShellManager` class, you can enable input history using the `InputHistoryEnabled` property by setting it to `true`. To disable it, just set it to `false`. The shell automatically loads and saves the history upon opening and closing the shell.

When `GetLine()` is called, it sets Terminaux's reader history to point to the shell's history list. After it's done getting the input, it reverts back to the `General` history buffer.

{% hint style="info" %}
You can force a reload on the history by using the `loadhistories` command across all the shells.

You can manually save the history list for all the shells using the `savehistories` command.
{% endhint %}

***

## <mark style="color:$primary;">History tools</mark>

Shells internally use the terminal reader's history feature to increase flexibility. Shells automatically create a history entry in case it doesn't exist. You can consult the below page to learn more about the terminal reader history.

{% content-ref url="../../" %}
[..](../../)
{% endcontent-ref %}
