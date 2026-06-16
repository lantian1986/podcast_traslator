# Podcast Translator MVP

A tiny first version for:

```text
Podcast RSS -> choose episode -> download audio -> streaming translated parts -> full WAV
```

The app is intentionally small:

- one Python file
- no Python package dependencies
- no bundled Whisper/TTS/LLM models
- GLM API for ASR, translation, and TTS
- output files stored under `data/jobs/`

You also need `ffmpeg` installed on the machine. It is used to split long
podcast audio into small ASR chunks and concatenate TTS chunks.

## Run

Set your GLM API key:

```bash
export GLM_API_KEY="..."
```

Then start the web app:

```bash
python3 server.py --host 127.0.0.1 --port 8080
```

Open:

```text
http://127.0.0.1:8080
```

## Built-in sample

Click `Run built-in sample` on the home page.

The sample skips RSS download and ASR. It uses a short built-in English
transcript, then runs:

```text
sample transcript -> GLM translation -> GLM TTS -> downloadable WAV
```

This lets you quickly check the flow and hear the output without installing
`ffmpeg`.

## Long podcast playback

Real podcast episodes are split into listening parts before processing. The
default is 300 seconds, or 5 minutes.

As soon as the first part is translated and converted to speech, the page shows
a player and starts playback. Later parts continue processing in the background
and are added to the playlist as they become ready.

When all parts are done, the app also creates one full WAV download.

## API settings

Optional settings:

```bash
export GLM_BASE_URL="https://open.bigmodel.cn/api"
export GLM_ASR_MODEL="glm-asr-2512"
export GLM_TRANSLATE_MODEL="glm-4.7-flash"
export GLM_TTS_MODEL="glm-tts"
export GLM_TTS_VOICE="tongtong"
export GLM_ASR_CHUNK_SECONDS="25"
export LISTEN_PART_SECONDS="300"
```

`glm-asr-2512` currently accepts short audio chunks, so this MVP uses `ffmpeg`
to split each podcast episode before transcription.

## Notes

This MVP keeps the project small by using remote services for heavy work.
Large local ASR/TTS models can be added later behind the same steps, but should
not be part of the first mobile-friendly version.
