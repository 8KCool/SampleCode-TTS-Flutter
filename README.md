<div align="center">
    <img src="https://media.giphy.com/media/tPjlmJzj9Z99vwF5dV/giphy.gif" width="50" align="right"/> 
    <img src="https://github.com/devicons/devicon/blob/master/icons/flutter/flutter-original.svg" width="50" align="left"/> 
    <h3>Text To Speech using Flutter</h3>
</div>

## Usage

## macOS

```bash
OSX version: 10.15
```

[Example App](https://github.com/dlutton/flutter_tts/tree/macOS_app) from the macOS_app branch

## Web

[Website](https://dlutton.github.io/flutter_tts) from the example directory.

## Android

Change the minimum Android sdk version to 21 (or higher) in your `android/app/build.gradle` file.

```java
minSdkVersion 21
```

Apps targeting Android 11 that use text-to-speech should declare TextToSpeech.Engine.INTENT_ACTION_TTS_SERVICE in the queries elements of their manifest.

### Pausing on Android

Android TTS does not support the pause function natively, so we have implemented a work around. We utilize the native `onRangeStart()` method to determine the index of start when `pause` is invoked. We use that index to create a new text the next time `speak` is invoked.  Due to using `onRangeStart()`, pause works on SDK versions >= 26.  Also, if using `start` and `end` offsets inside of `setProgressHandler()`, you'll need to keep a track of them if using `pause` since they will update once the new text is created when `speak` is called after being paused.

```dart
await flutterTts.pause()
```

## iOS

There's a known issue with integrating plugins that use Swift into a Flutter project created with the Objective-C template. [Flutter#16049](https://github.com/flutter/flutter/issues/16049)

[Example](https://github.com/dlutton/flutter_tts/blob/master/example/lib/main.dart)

To use this plugin :

- add the dependency to your [pubspec.yaml](https://github.com/dlutton/flutter_tts/blob/master/example/pubspec.yaml) file.

```yaml
  dependencies:
    flutter:
      sdk: flutter
    flutter_tts:
```

- instantiate FlutterTts

```dart
FlutterTts flutterTts = FlutterTts();

```

To set shared audio [instance](https://developer.apple.com/documentation/avfoundation/avaudiosession/1616504-sharedinstance) (iOS only):

```dart
await flutterTts.setSharedInstance(true);
```

To set audio [category and options](https://developer.apple.com/documentation/avfoundation/avaudiosession) with optional [mode](https://developer.apple.com/documentation/avfaudio/avaudiosession/mode) (iOS only). The following setup allows background music and in-app audio session to continue simultaneously:

```dart
await flutterTts.setIosAudioCategory(IosTextToSpeechAudioCategory.ambient,
     [
          IosTextToSpeechAudioCategoryOptions.allowBluetooth,
          IosTextToSpeechAudioCategoryOptions.allowBluetoothA2DP,
          IosTextToSpeechAudioCategoryOptions.mixWithOthers
     ],
     IosTextToSpeechAudioMode.voicePrompt
);
```

To await speak completion.

```dart
await flutterTts.awaitSpeakCompletion(true);
```

To await synthesize to file completion.

```dart
await flutterTts.awaitSynthCompletion(true);
```

### speak, stop, getLanguages, setLanguage, setSpeechRate, setVoice, setVolume, setPitch, isLanguageAvailable, setSharedInstance

```dart
Future _speak() async{
    var result = await flutterTts.speak("Hello World");
    if (result == 1) setState(() => ttsState = TtsState.playing);
}

Future _stop() async{
    var result = await flutterTts.stop();
    if (result == 1) setState(() => ttsState = TtsState.stopped);
}

List<dynamic> languages = await flutterTts.getLanguages;

await flutterTts.setLanguage("en-US");

await flutterTts.setSpeechRate(1.0);

await flutterTts.setVolume(1.0);

await flutterTts.setPitch(1.0);

await flutterTts.isLanguageAvailable("en-US");

// iOS, Android and Web only
//see the "Pausing on Android" section for more info
await flutterTts.pause();

// iOS, macOS, and Android only
await flutterTts.synthesizeToFile("Hello World", Platform.isAndroid ? "tts.wav" : "tts.caf");

await flutterTts.setVoice({"name": "Karen", "locale": "en-AU"});

// iOS only
await flutterTts.setSharedInstance(true);

// Android only
await flutterTts.setSilence(2);

await flutterTts.getEngines;

await flutterTts.getDefaultVoice;

await flutterTts.isLanguageInstalled("en-AU");

await flutterTts.areLanguagesInstalled(["en-AU", "en-US"]);

await flutterTts.setQueueMode(1);

await flutterTts.getMaxSpeechInputLength;
```

### Listening for platform calls

```dart
flutterTts.setStartHandler(() {
  setState(() {
    ttsState = TtsState.playing;
  });
});

flutterTts.setCompletionHandler(() {
  setState(() {
    ttsState = TtsState.stopped;
  });
});

flutterTts.setProgressHandler((String text, int startOffset, int endOffset, String word) {
  setState(() {
    _currentWord = word;
  });
});

flutterTts.setErrorHandler((msg) {
  setState(() {
    ttsState = TtsState.stopped;
  });
});

flutterTts.setCancelHandler((msg) {
  setState(() {
    ttsState = TtsState.stopped;
  });
});

// Android, iOS and Web
flutterTts.setPauseHandler((msg) {
  setState(() {
    ttsState = TtsState.paused;
  });
});

flutterTts.setContinueHandler((msg) {
  setState(() {
    ttsState = TtsState.continued;
  });
});

// Android only
flutterTts.setInitHandler(() {
  setState(() {
    ttsState = TtsState.initialized;
  });
});

```

## Getting Started

For help getting started with Flutter, view our online
[documentation](https://flutter.dev/).

For help on editing plugin code, view the [documentation](https://flutter.dev/platform-plugins/#edit-code).

Copyright © 2018 BlackPandaLancer
