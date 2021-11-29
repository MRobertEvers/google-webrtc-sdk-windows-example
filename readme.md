# WebRTC Peer Connection Example for Windows

This is a fixed peer connection example from Google's webrtc/examples/peerconnection.

Previously, their example was never starting the worker pump for the Task Queue, so callbacks were never getting called. I simply put the Socket stuff on a different thread than the Windows UI thread, then pass commands as tasks back to the Socket thread with the Task Queue.

## Usage

Build the WebRTC SDK as normal following the instructions here, https://webrtc.github.io/webrtc-org/native-code/development/.

Replace the following files in the example and build

```
main_wnd.cc
main_wnd.h
main.cc
```

Once the build is complete, start the peerconnection_server.exe in `out/Default/peerconnection_server.exe`.

Then you can start the `peerconnection_client.exe` in the same folder.

Once peerconnection_client has started, click connect, then open the `server_test.html` in `examples/peerconnection/server`. On the peerconnection_client, double click the peer name you just typed in the browser (default is `my_name`).

Back in the browser, make sure loopback is set, then type `Target peer id` to `1`, type any message, then click Send. The peerconnection_client should start your webcam and then start displaying your webcam to yourself.

## Debugging

You can debug the executables normally using Visual Studio. Simply open the .exe in Visual Studio and add the source files to the project.

## Hacks

I've hacked on this a bit for some testing.

I added a (LEAKS MEMORY!!) AudioTrackSink to the audio track and saved the data to a file.

(As of writing this hack, the code writes the audio frame data to `audio3.pcm` and frame metadata to `frame_data.txt`)

Notice, like the video frames, sometimes audio frames come in ad different parameters.

I simply opened the audio file in audacity (import -> Raw Data) with 1 channel, 48000 Hz, 16 bit signed.

```
# frame_data.txt
bps: 16 rate: 16000 channels: 1 bytes: 320
bps: 16 rate: 16000 channels: 1 bytes: 320
bps: 16 rate: 16000 channels: 1 bytes: 320
bps: 16 rate: 16000 channels: 1 bytes: 320
bps: 16 rate: 16000 channels: 1 bytes: 320
bps: 16 rate: 16000 channels: 1 bytes: 320
bps: 16 rate: 16000 channels: 1 bytes: 320
bps: 16 rate: 48000 channels: 1 bytes: 960
bps: 16 rate: 48000 channels: 1 bytes: 960
bps: 16 rate: 48000 channels: 1 bytes: 960
bps: 16 rate: 48000 channels: 1 bytes: 960
bps: 16 rate: 48000 channels: 1 bytes: 960
bps: 16 rate: 48000 channels: 1 bytes: 960
bps: 16 rate: 48000 channels: 1 bytes: 960
bps: 16 rate: 48000 channels: 1 bytes: 960
bps: 16 rate: 48000 channels: 1 bytes: 960
```
