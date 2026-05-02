---
description: Click, click, click!
icon: waveform
---

# Audio Cues

Audio cues on Terminaux give your terminal applications an ability to emit audible feedback when a key is pressed. This makes sure that your terminal applications notify you what kind of key you pressed.

The audio cues functionality is located in the `Input` class where such functionality is located, and you can turn the audio cues on by setting the `KeyboardCues` property to `true`.

***

## <mark style="color:$primary;">Audio cues settings</mark>

You can use the following properties to change how the audio cues work, such as the audio being played when a key is pressed and the volume.

<table><thead><tr><th width="200">Property</th><th></th></tr></thead><tbody><tr><td><code>CueVolume</code></td><td>Keyboard cue volume</td></tr><tr><td><code>CueVolumeBoost</code></td><td>Whether to boost the keyboard cue volume up to 300%</td></tr><tr><td><code>CueEnter</code></td><td>A keyboard cue stream that contains MP3 data that will be played when <kbd>ENTER</kbd> is pressed.</td></tr><tr><td><code>CueRubout</code></td><td>A keyboard cue stream that contains MP3 data that will be played when <kbd>Backspace</kbd> is pressed.</td></tr><tr><td><code>CueWrite</code></td><td>A keyboard cue stream that contains MP3 data that will be played when a key is pressed.</td></tr><tr><td><code>BassBoomLibraryPath</code></td><td>Path to BassBoom library path (set before turning the audio cues on)</td></tr></tbody></table>

The terminal reader overrides the above settings and uses its own settings, depending on the settings instance. For more information, you can consult [this page here](../reader-settings.md).

{% hint style="info" %}
Make sure that your computer has the sound drivers installed. For more information, you can consult [this page](https://app.gitbook.com/s/izAJoIbtQw1BdIlE4DBz/installation).

If an audio cue failed to play, you'll not be able to play further audio cues, and you'll have to use the `ForceRecheck()` function to tell Terminaux to check BassBoom again when an audio cue plays for the next time.
{% endhint %}

***

## <mark style="color:$primary;">Independent audio cues</mark>

Additionally, you can play an audio cue manually and independently by calling the `PlayKeyAudioCue()` function from the `ConsoleAcoustic` class. It allows you to select an audio cue to play, while you can optionally specify a terminal reader settings instance.
