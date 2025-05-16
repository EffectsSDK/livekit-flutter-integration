# Effects SDK flutter LiveKit integration

Tested for **LiveKit v2.4.6** and **webRTC v0.14.0**

## Instructions

1. Clone flutter [WebRTC](https://github.com/flutter-webrtc/flutter-webrtc) and [LiveKit](https://github.com/livekit/client-sdk-flutter) repositories
```bash
git clone https://github.com/flutter-webrtc/flutter-webrtc.git
git clone https://github.com/livekit/client-sdk-flutter.git
```
2. Download patch([WebRTC](webRTC-patch.diff) and [livekit](livekit-patch.diff)) from this repository and apply it
```bash
cd flutter-webrtc
git apply path/to/webRTC-patch.diff

cd client-sdk-flutter
git apply path/to/livekit-patch.diff
```
3. Download effects SDK release for your platform ([iOS] [android](https://github.com/EffectsSDK/android-integration-sample/releases))
4. Add Effects SDK as webRTC dependency (to **flutter-webrtc/android/libs** catalog)
5. Add LiveKit files as dependency to your project

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
1. [iOS]
2. [android](https://github.com/EffectsSDK/android-integration-sample)


## Technical details

All additional LocalVideoTrack methods was implemented as extension functions. Also,
we create custom CameraVideoCapturer instance for Effects SDK camera pipeline(android).
You can modify our solution as you need or try another way for integration (for example with custom VideoProcessor).
Also you can replace CameraPipeline to lite version of it.

## Additional links

1. Platform documentation (iOS, [android](https://github.com/EffectsSDK/android-integration-sample))
2. Effects SDK [site](https://effectssdk.ai/)

