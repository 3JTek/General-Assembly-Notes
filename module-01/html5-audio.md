# HTML 5 Audio

In the beginning there was HTML and it was good. Except that it had no support for streaming media like audio or video, so we had to resort to using Flash. The main problem with Flash was that it required the user to install it. Lost of time was spent on coming up with workarounds for users who either hadn't installed it, or didn't know what it was. Then Apple decided they didn't like it and it slowly died.

Then HTML5 came along with shiny new tags like:

* `<audio>`
* `<video>`

Each browser now had its own built-in audio and video player. As developers, we just need to set them up correctly, and our work is done.

## `audio`

The `audio` tag is relatively simple to set up:

```html
<audio controls src="http://soundbible.com/mp3/Audience_Applause-Matthiew11-1206899159.mp3"></audio>
```

The `controls` attribute displays controls allowing the user to play/pause and scrub the audio. Without it, the player is hidden from sight.

### Common attributes

| **Attribute** | **Value** | **Description** |
|---------------|-----------|-----------------|
| `autoplay` | N/A | Audio will start playing as soon as it is ready |
| `controls` | N/A | Controls should be displayed |
| `loop` | N/A | Audio will start again when finished |
| `muted` | N/A | Output is muted |
| `preload` | `audio` / `metadata` / `none` | How the audio should be loaded. `audio` will load the audio file immediately on page load, `metadata` will load the audio track's info but not the actual audio file. |
| `src` | URL | A link to the audio file |

You can also set it up slightly differently offering a fallback if certain audio formats are not supported, or if the browser does not support audio at all:


```html
<audio controls>
  <!-- This will be displayed if audio is not supported -->
  Your browser does not support audio, please consider switching to a modern standards compliant browser, like Google Chrome.
  <!-- The audio files to play, the browser will play the first one that it is able -->
  <source src="http://soundbible.com/mp3/Audience_Applause-Matthiew11-1206899159.mp3" type="audio/mp3" />
</audio>
```

### Browser support

Browser support for different audio formats are as follows:

| **Format** | Chrome | Internet Explorer | Edge | Firefox | Safari |
|--------|--------|-------------------|------|---------|--------|
| **WAV** | Yes | No | Yes | Yes | Yes |
| **MP3** | Yes | Yes | Yes | No | Yes |
| **MP4** | Yes | Yes | Yes | No | Yes |
| **Ogg** | Yes | No | No | Yes | No |
| **WebM** | Yes | No | No | Yes | No |
| **FLAC** | Yes | No | Yes | Yes | No |

Generally MP3 plus WAV or Ogg is the way to go to support all browsers.

## Adding JavaScript

We can interact with the audio player with the following properties an methods:

| **Property / Method** | **Description** |
|-----------------------|-----------------|
| `audio.src` | Get or set the `src` of the audio |
| `audio.load()` | Reload the audio file |
| `audio.play()` | Start playback |
| `audio.pause()` | Pause playback |
| `audio.currentTime` | Get or set the current playback position |
| `audio.paused` | Indicates whether playback is currently paused |
| `audio.onended` | Triggered when audio has ended |

## Further reading

* [HTMLMediaElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement)
* [HTML - Audio](https://tutorialehtml.com/en/html-tutorial-embed-audio/)
* [Create a Customized HTML5 Audio Player](https://webdesign.tutsplus.com/tutorials/create-a-customized-html5-audio-player--webdesign-7081)
