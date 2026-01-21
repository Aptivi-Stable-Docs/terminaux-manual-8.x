---
description: What have you run last time?
icon: hourglass-end
---

# Shell History

The shell history is another MESH shell feature that allows you to recall the last commands that you've executed in the current and in the previous sessions. This is accompanied by a file that shows you a history of commands executed in all the sessions.

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

Accessing the shell history is easy; just press the up arrow button on your keyboard to recall the previous command executed. You can also press the down arrow button to recall the next command in the whole history.

In the `ShellManager` class, you can enable input history using the `InputHistoryEnabled` property by setting it to `true`. To disable it, just set it to `false`. The shell automatically loads and saves the history upon opening and closing the shell.

## History tools

Shells internally use the terminal reader's history feature to increase flexibility. Shells automatically create a history entry in case it doesn't exist. You can consult the below page to learn more about the terminal reader history.

{% content-ref url="../../" %}
[..](../../)
{% endcontent-ref %}
