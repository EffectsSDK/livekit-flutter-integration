# Effects SDK flutter LiveKit integration

> **⚠️ Important:** This repository has moved!  
> Please see the new location: [livekit-client-sdk-flutter](https://github.com/EffectsSDK/livekit-client-sdk-flutter)


Tested for **LiveKit v2.4.6** and **flutter-webrtc v0.14.0**

## Instructions

1. Clone flutter [WebRTC](https://github.com/flutter-webrtc/flutter-webrtc) and [LiveKit](https://github.com/livekit/client-sdk-flutter) repositories
```bash
git clone https://github.com/flutter-webrtc/flutter-webrtc.git
git clone https://github.com/livekit/client-sdk-flutter.git
```
2. Download patch([flutter-webrtc](flutter-webrtc-effects-sdk.patch) and [livekit](livekit-client-flutter-effects-sdk.patch)) from this repository and apply it
```bash
cd flutter-webrtc
git switch -d v0.14.0
curl https://raw.githubusercontent.com/EffectsSDK/livekit-flutter-integration/refs/heads/main/flutter-webrtc-effects-sdk.patch | git apply

cd client-sdk-flutter
git switch -d v2.4.6
curl https://raw.githubusercontent.com/EffectsSDK/livekit-flutter-integration/refs/heads/main/livekit-client-flutter-effects-sdk.patch | git apply
```
3. Add LiveKit files as dependency to your project
```yaml
dependencies:
  livekit_client:
    path: /path/to/client-sdk-flutter
```

### Android

4. Download the Video Effects SDK release for android platform. [Releases](https://github.com/EffectsSDK/android-integration-sample/releases)
5. Add the Video Effects SDK as flutter-webrtc dependency (to **flutter-webrtc/android/libs** catalog)

Make sure all repository path in pubspec.yaml set correctly

## LiveKit demo app

We made some changes in **client-sdk-flutter/example** app, so you can build it as is from patched repository

## How to use

1. Create cameraVideoTrack by using LocalVideoTrack.createCameraTrack() with effectsEdkRequired flag
2. Call auth() method with your customer key
3. Set Effects SDK parameters for your video track

```dart
_videoTrack = await LocalVideoTrack.createCameraTrack(
  CameraCaptureOptions(
      deviceId: _selectedVideoDevice!.deviceId,
      params: _selectedVideoParameters,
      effectsSdkRequired: true,
  )
);
await _videoTrack!.start();
AuthStatus status = await _videoTrack!.auth('YOUR_CUSTOMER_KEY');
switch (status){
    case AuthStatus.active:
      _videoTrack!.setPipelineMode(PipelineMode.blur);
      _videoTrack!.setBlurPower(0.99);
      break;
    case AuthStatus.expired:
      // TODO: Handle this case.
      break;
    case AuthStatus.inactive:
      // TODO: Handle this case.
      break;
    case AuthStatus.unavailable:
      // TODO: Handle this case.
      break;
}

```

You can manage all sdk parameters without VideoTrack recreation.

## EffectsSDK methods

Check platform specifications:
1. [iOS](https://github.com/EffectsSDK/swift-video-effects-sdk)
2. [android](https://github.com/EffectsSDK/android-integration-sample)

### Effects SDK Image

class EffectsSdkImage - image proxy for iOS/android compatibility.

Can be created from:

1. raw data
2. file
3. encoded data
4. rgb color

## Technical details

All additional LocalVideoTrack methods was implemented as extension functions. Also,
we create custom CameraVideoCapturer instance for Effects SDK camera pipeline(android).
You can modify our solution as you need or try another way for integration (for example with custom VideoProcessor).
Also you can replace CameraPipeline to lite version of it.

## Additional links

1. Platform documentation ([iOS](https://effectssdk.ai/sdk/ios/documentation/tsvb), [android](https://github.com/EffectsSDK/android-integration-sample))
2. Effects SDK [site](https://effectssdk.ai/)

