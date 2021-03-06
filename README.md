# flutter_audio_recorder
<p align="left">
  <a href="https://pub.dartlang.org/packages/flutter_audio_recorder"><img alt="pub version" src="https://img.shields.io/pub/v/flutter_audio_recorder.svg?style=flat-square"></a>
  <a href="https://github.com/Solido/awesome-flutter">
   <img alt="Awesome Flutter" src="https://img.shields.io/badge/Awesome-Flutter-blue.svg?longCache=true&style=flat-square" />
</a>
</p>

English | [简体中文](./README-zh_CN.md)

Flutter Audio Record Plugin that supports `Record` `Pause` `Resume` `Stop` and provide access to audio level metering properties `average power` `peak power`(currently for iOS only)

<img src="https://user-images.githubusercontent.com/10917606/64927086-b2bcda00-d838-11e9-9ab8-bad78a95f02c.gif" width="30%" height="30%" />

## Installation
add `flutter_audio_recorder` to your `pubspec.yaml`

## iOS Permission 
add usage description to plist
```
<key>NSMicrophoneUsageDescription</key>
<string>Can We Use Your Microphone Please</string>
```

### note: iOS Deployment Target is 8.0

## Usage

#### Init (run this before `start`, so we could check if file with given name already exists)
```
var recorder = FlutterAudioRecorder("file_path.mp4"); // .wav .aac .m4a
await recorder.initialized;
```

or 

```
var recorder = FlutterAudioRecorder("file_path", audioFormat: AudioFormat.AAC); // or AudioFormat.WAV
await recorder.initialized;
```

##### Sample Rate
```
var recorder = FlutterAudioRecorder("file_path", audioFormat: AudioFormat.AAC, sampleRate: 22000); // sampleRate is 16000 by default
await recorder.initialized;
```

##### Audio Extension and Format Mapping
| Audio Format  | Audio Extension List |
| ------------- | ------------- |
| AAC  | .m4a .aac .mp4  |
| WAV  | .wav  |

#### Start recording
```
await recorder.start();
var recording = await recorder.current(channel: 0);
```

#### Get recording details
```
var current = await recording.current(channel: 0);
// print(current.status);
```
You could use a timer to access details every 50ms(simply cancel the timer when recording is done)
```
new Timer.periodic(tick, (Timer t) async {
        var current = await recording.current(channel: 0);
        // print(current.status);
        setState(() {
        });
      });
```

##### Recording
| Name  | Description |
| ------------- | ------------- |
| path  | String  |
| extension  | String  |
| duration  | Duration  |
| audioFormat  | AudioFormat  |
| metering  | AudioMetering  |
| status  | RecordingStatus  |

##### Recording.metering
| Name  | Description |
| ------------- | ------------- |
| peakPower  | double  |
| averagePower  | double  |
| isMeteringEnabled  | bool  |

##### Recording.status
`Unset`,`Initialized`,`Recording`,`Paused`,`Stopped`


#### Pause
```
await recorder.pause();
```

#### Resume
```
await recorder.resume();
```

#### Stop (after `stop`, run `init` again to create another recording)
```
var result = await recorder.stop();
File file = widget.localFileSystem.file(result.path);
```

## Example
Please check example app using Xcode.


## Getting Started

This project is a starting point for a Flutter
[plug-in package](https://flutter.dev/developing-packages/),
a specialized package that includes platform-specific implementation code for
Android and/or iOS.

For help getting started with Flutter, view our 
[online documentation](https://flutter.dev/docs), which offers tutorials, 
samples, guidance on mobile development, and a full API reference.
