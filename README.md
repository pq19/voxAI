# voxAI

A sound-activated audio recorder with support for uploading audio to [OpenMHz](https://www.openmhz.com), [Broadcastify Calls](https://www.broadcastify.com/calls/) and [rdio-scanner](https://github.com/chuot/rdio-scanner). For Windows and Raspberry Pi.

![Screenshot](images/voxcall_screenshot.png)

## Recent Updates

### 4/25/2026 Paul Qiu
- **Audio Device Management**: Changed from device indices to device names for better reliability when audio devices are added/removed from the system
- **Configuration Section**: Renamed `[Section1]` to `[System]` in config.cfg
- **Audio Folder**: Renamed `audio_file_folder` to `audio_save_folder` in configuration
- **Device Detection**: Added warning dialogs when configured audio devices are not found on startup
- **Fallback Logic**: Automatically selects first available device if configured device is missing

### 4/23/2026 Paul Qiu
- Added configurable playback audio buttons via `config.cfg` under a new `[Playback_Audio]` section
- Playback buttons now support multiple rows and display up to 3 buttons per row
- Playback `.wav` files are now stored in the `audio/` subfolder
- Added GUI spacing so playback buttons and the `Save & Exit` button appear as separate sections

### 3/28/2026 Paul Qiu
- Changed name to voxAI
- Added configuration for saving folder
- Changed file name from EPOCH to YYYYMMDD-HHMMSS

## Operation

### Basic Setup
- Connect a single-channel radio receiver to the sound card input on the computer. If audio will be uploaded to Broadcastify Calls, the receiver should not be scanning multiple frequencies.
- Set the Audio Squelch using the slider. Audio above the level of the slider will trigger recording. The current audio level is shown adjacent to the slider. Set the level of the slider while testing with the radio squelched and unsquelched.

### Audio Device Configuration
- **Input Device**: Select your audio input device from the dropdown. The application now uses device names instead of indices, making it resilient to device changes.
- **Output Device**: Select your audio output device for playback buttons. Default device is marked with "(Default)".
- **Channel Selection**: Choose mono, left, or right channel for stereo inputs.

### Upload Services Configuration

#### Broadcastify Calls
- API key goes in the API key field
- SID goes in the System ID field
- Slot goes in the Slot ID field (default to 1 if no Slot ID is provided)

#### rdio-scanner
- URL of the rdio-scanner api (for example: 192.168.1.138:3000/api/call-upload)
- rdio-scanner API key (create using the rdio-scanner admin interface)
- System ID
- Talkgroup ID

#### OpenMHz
- OpenMHz API key
- ShortName of the System
- Talkgroup ID (for conventional systems, simply choose any integer number for this field)
- A Radio Frequency must be entered in the General Settings area

### Recording and Upload Process
- When audio is detected above the Audio Squelch level, audio will be recorded until two seconds of silence is detected. Once the recording ends, an MP3 file will be created.
  - If valid Broadcastify Calls credentials are entered, the MP3 file will be uploaded to that system.
  - If valid rdio-scanner credentials are entered, the MP3 file will be uploaded to that system.
  - If valid OpenMHz credentials are entered, the MP3 file will be uploaded to that system.
  - If the "Save Audio Files" option is selected, the recordings will be saved to the configured audio save folder. Otherwise, the MP3 will be deleted. The filename will be in YYYYMMDD-HHMMSS format.
- There is a two-minute timeout timer. If a recording exceeds two minutes (stuck squelch, noise, etc.) recording will stop, an error will be displayed, and no further activity will take place until the input audio goes below the Audio Squelch threshold, at which time normal operation will resume.

### Playback Buttons
- Configure up to 11 playback buttons in the `[Playback_Audio]` section of config.cfg
- Each button can play a different .wav file from the `audio/` folder
- Buttons are arranged in rows of up to 3 buttons each

### Multiple Instances
- Multiple instances can be run at the same time to capture audio from multiple receivers. Create a different directory for each instance. Each directory must have a config.cfg file.
- On Windows, each directory should also have a copy of ffmpeg.exe or ffmpeg.exe must be added to the system path.

### Troubleshooting
- Something not working? Check the log.txt file for errors and create an Issue here if needed.
- If audio devices change, the application will show warning dialogs and automatically select available devices.

## Configuration File (config.cfg)

### [System] Section
```
audio_input_device_name = Microphone (2- USB Audio Device)
audio_output_device_name = Speakers (2- USB Audio Device)
record_threshold = 75
vox_silence_time = 2
in_channel = mono
bcfy_systemid =
radiofreq = 3
bcfy_apikey =
bcfy_slotid = 1
saveaudio = 1
rdio_apikey =
rdio_apiurl =
rdio_system =
rdio_tg =
openmhz_api_key =
openmhz_short_name =
openmhz_tgid =
audio_save_folder = G:\My Drive\voxAIsave\
```

### [Playback_Audio] Section
```
num_playback_audios = 11
button1_text = Roger
button1_file = audio/roger.wav
button2_text = Who Is Calling
button2_file = audio/whoiscalling.wav
...
```

## Windows EXE
[ZIP Download](https://github.com/aaknitt/voxcall/releases/download/1.1.0/voxcall.zip.-.Windows.EXE.zip)
- Uncompress the downloaded ZIP file
- Run the EXE

## Raspberry Pi executable binary (compiled for Raspbian Buster)
[TGZ Download](https://github.com/aaknitt/voxcall/releases/download/1.1.0/voxcall.tgz.-.Raspberry.Pi.32.bit.OS.tgz)
- Use a cheap USB sound card as the audio input - the Pi does not come with an audio input
- Download using link above or via `curl -O https://radioetcetera.site/radioetcetera/files/voxcall.tgz`
- `tar zxf voxcall.tgz` to uncompress
- Install pulseaudio:
  - `sudo apt-get install pulseaudio`
- If ffmpeg isn't already installed, install it:
  - `sudo apt-get install ffmpeg`
- To run:
  - `/home/pi/voxcall/voxcall`




