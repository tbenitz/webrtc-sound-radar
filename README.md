# webrtc-sound-radar

Real-time sound source localization and radar map using two microphones via WebRTC/PeerJS. Estimates distance/direction with TDOA (time difference of arrival) and supports calibration pings.

## Live Demo

Enable GitHub Pages (Settings → Pages → Deploy from branch `main` / root) then visit:

**https://tbenitz.github.io/webrtc-sound-radar/**

## How to Use

1. Open the site on **two devices** that have microphones (phones, laptops, tablets).
2. On one device click **Host Session**. Copy the Peer ID.
3. On the other device click **Join Session**, paste the Peer ID, and Connect.
4. Allow microphone access on both.
5. Place the devices a known distance apart and enter that distance in the **Baseline** field (meters).
6. Make noises (clap, talk, drop something). The radar canvas shows estimated source locations as fading blips.
7. Use **Send Calibration Ping** (plays a short beep) to help sync clocks between devices.

## How It Works

- PeerJS handles the WebRTC data channel for exchanging audio chunks and control messages.
- Each device captures its local microphone (AGC/noise suppression disabled when possible).
- Short audio buffers are sent peer-to-peer.
- The host (or either peer) runs a simple cross-correlation to estimate the sample lag between the two microphones.
- Lag is converted to a distance difference using the speed of sound (~343 m/s).
- Combined with the known baseline distance, an approximate source position is plotted on the radar map.

## Limitations & Tips for Better Accuracy

- Browser microphone processing (echo cancellation, AGC, noise suppression) can distort timing. The app requests them off, but support varies.
- Network latency and jitter affect the absolute timing; the calibration ping helps estimate clock offset.
- Works best in a relatively quiet room with clear, impulsive sounds (claps work better than continuous noise).
- Two microphones give a hyperbola of possible locations; the visualization is a simplified approximation.
- For more precision you would need more microphones or tighter clock synchronization.

## Tech

- Pure HTML/CSS/JS (single file)
- [PeerJS](https://peerjs.com/) for easy WebRTC
- Web Audio API for capture and processing
- Canvas for the radar display

Created as a proof-of-concept for real-time acoustic localization in the browser.