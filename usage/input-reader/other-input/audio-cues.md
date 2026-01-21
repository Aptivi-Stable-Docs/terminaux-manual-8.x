---
description: Click, click, click!
icon: waveform
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/G0KrE9Uk2AiblqjWtpAo/usage/input-reader/other-input/audio-cues
---

# Audio Cues

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

## Independent audio cues

Additionally, you can play an audio cue manually and independently by calling the `PlayKeyAudioCue()` function from the `ConsoleAcoustic` class. It allows you to select an audio cue to play, while you can optionally specify a terminal reader settings instance.

{% hint style="info" %}
Since this feature uses BassBoom, you'll need to make sure that your computer has sound drivers installed. If an audio cue failed to play, you'll not be able to play further audio cues, and you'll have to use the `ForceRecheck()` function to tell Terminaux to check BassBoom again when an audio cue plays for the next time.
{% endhint %}
