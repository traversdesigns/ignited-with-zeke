# IGNITED with Zeke: protected video recording development

## Production safety
- The live `main/index.html` is the working version. Do not edit it while prototyping.
- All recording experiments happen on branch `development/video-recorder`.
- Preserve Zeke's existing Bluefy BLE connection, Live Mode, AI conversations, Bluetooth speaker playback (including jaw movement), and arm/torso gesture scheduling.
- The user must confirm a full end-to-end test before promoting any changes to main.

## Recording acceptance criteria
1. One iPhone running Bluefy and IGNITED; no second device or paid native app.
2. One continuous camera video containing multiple visitor questions and Zeke's replies.
3. Final video has synchronized audio from BOTH visitor and Zeke, plus visible jaw and body movements.
4. Zeke continues to hear and answer second, third and later questions during recording.
5. Stop Recording works and produces a playable, shareable file on iPhone.
6. No changes to Bluetooth audio routing; Zeke's voice continues from his speaker and his jaw keeps moving during and after recording.
7. Recordings remain on-device unless user explicitly chooses to export/share.

## What prior tests established
- Requesting a second microphone with camera capture redirected voice to the iPhone and disrupted jaw behavior.
- Video-only camera capture worked alongside Zeke's speech and movements, but recorded silent video.
- A live Web Audio / MediaRecorder audio-mixing experiment interfered with Stop Recording and subsequent conversation. It was reverted.
- The production version currently uses camera video only, with the AI/BLE functions preserved.

## Implementation direction (unverified)
- Avoid simultaneous new microphone requests and live AudioContext routing while Zeke speaks.
- Observe and retain the existing visitor audio clips used for transcription and AI voice response blobs; collect monotonic timestamps tied to a single recording start.
- Record video-only using the established camera path.
- Assemble audio/video *after* capture, ideally in a worker or offline browser processing step. Assess Bluefy codec, memory, iOS backgrounding, timing, and export constraints before promising results.
- Include a clear error/fallback if Bluefy cannot mux audio and video locally.
- Build/test on the development branch, never directly on main.

## Test checklist
- Bluetooth connected and Live Mode on
- Speaker route and jaw work before camera
- Camera starts without microphone routing change
- Ask three distinct questions; Zeke answers each
- Arm/torso gestures remain active
- Stop Recording responds promptly
- Saved video plays with both voices synchronized
- Zeke continues answering after camera closes

## Interface cleanup, later
- Keep existing controls; group Everyday Conversation, Record Videos, Events/TikTok, Advanced.
- Do not redesign production UI during recording work.
