# Assignment 4.11: Audio

## Requirements

- I've collected some sound effects Download sound effects to save you some time. (Courtesy of [FreeSound.org](https://www.freesound.org)).
- Integrate an audio library into your engine. Here are some:
  - [XAudio2](https://learn.microsoft.com/en-us/windows/win32/xaudio2/xaudio2-apis-portal?redirectedfrom=MSDN)
    - [Tutorial](https://learn.microsoft.com/en-us/windows/win32/xaudio2/getting-started?redirectedfrom=MSDN)
  - [OpenAL](https://www.openal.org/)
  - [FMOD](https://www.fmod.com/)
    - [Tutorial](https://www.gamedev.net/tutorials/programming/general-and-gameplay-programming/a-quick-guide-to-fmod-r2098/)
  - [Wwise](https://www.audiokinetic.com/en/wwise/overview/)

- Initialize the audio device
- Create an audio playback interface
- 2D Sound Effects
  - The player's team has picked up the opponent's flag.
  - The opposing team has picked up the player's team's flag.
  - Either flag has been reset.
  - Player's team scores a point.
  - Opposing team scores a point.
- 3D Sound Effects - these sounds change volume depending on how close the "listener" is to the sound source.
  - Footsteps on 2 different surfaces
    - The footsteps of the network peer player
  - A player is sprinting. (panting)
    - The sprinting of the network peer player
  - Ambient world sounds.
- Music
  - Pick any music you like, but remember... you'll hate it after awhile.
- Volume Adjustment
  - Provide separate volume adjustment for sound effects and Music, in game. (Use Debug Menu sliders if you'd like)
  - Provide a command-line option or config file setting to disable music, sound effects, or both.  If both are disabled, you should skip audio initialization entirely.
- Submissions should be in video format... make sure you are connected across the network and demonstrate the remote peer's footsteps and sprinting.
[Game Audio - Introduction to Audio API's, and Comparison between Xact, FMOD, Wwise, And Unreal - YouTube](https://www.youtube.com/watch?v=B_CrykyrAnw)