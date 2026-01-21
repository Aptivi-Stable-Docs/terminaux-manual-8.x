---
description: Click, click, click!
icon: waveform
---

# Audio cues

Audio cues on Terminaux give your terminal applications an ability to emit audible feedback when a key is pressed. This makes sure that your terminal applications notify you what kind of key you pressed.

The audio cues functionality is located in the `Input` class where such functionality is located, and you can turn the audio cues on by setting the `KeyboardCues` property to `true`. Once turned on, you can use the following properties to change how the audio cues work, such as the audio being played when a key is pressed and the volume.

* `CueVolume`: Keyboard cue volume
* `CueVolumeBoost`: Whether to boost the keyboard cue volume up to 300%
* `CueEnter`: A keyboard cue stream that contains MP3 data that will be played when <kbd>ENTER</kbd> is pressed.
* `CueRubout`: A keyboard cue stream that contains MP3 data that will be played when <kbd>Backspace</kbd> is pressed.
* `CueWrite`: A keyboard cue stream that contains MP3 data that will be played when a key is pressed.
* `BassBoomLibraryPath`: Path to BassBoom library path (set before turning the audio cues on)

{% hint style="info" %}
Make sure that your computer has the sound drivers installed. For more information, you can consult [this page](https://app.gitbook.com/s/izAJoIbtQw1BdIlE4DBz/installation).
{% endhint %}

The terminal reader overrides the above settings and uses its own settings, depending on the settings instance. For more information, you can consult [this page here](../reader-settings.md).
