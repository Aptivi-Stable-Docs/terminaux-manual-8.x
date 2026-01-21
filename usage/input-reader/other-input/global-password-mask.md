---
description: Set the password mask everywhere!
icon: unlock
---

# Global Password Mask

Terminaux allows you to set the global password mask for all inputs that are passwords. This lets you choose a character that will be shown in the console when you write a letter to this kind of input. This is set globally across the entire session, unless it's overridden by either the [terminal reader settings](../reader-settings.md) or the [infobox settings](../../console-tools/console-writers/informational-boxes.md).

You can set the global password mask for all input (except the inputs that override it) by changing the value of the `PasswordMaskChar` property in the Input class.
