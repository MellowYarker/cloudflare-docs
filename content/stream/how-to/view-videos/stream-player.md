---
title: View with the Stream Player
pcx-content-type: concept
weight: 1
meta:
  title: View with the Stream Player
---

# View with the Stream Player

The Stream Player can be placed on a web page in an iframe element with the video UID or [signed token](/stream/how-to/secure-your-stream) by replacing `$VIDEOID` in the example below. The Stream Player is also available as a [React](https://www.npmjs.com/package/@cloudflare/stream-react) or [Angular](https://www.npmjs.com/package/@cloudflare/stream-angular) component.

```html
<iframe
  src="https://iframe.videodelivery.net/$VIDEOID"
  style="border: none;"
  height="720"
  width="1280"
  allow="accelerometer; gyroscope; autoplay; encrypted-media; picture-in-picture;"
  allowfullscreen="true"
></iframe>
```

## Player Size

### Fixed Dimensions

Change the `height` and `width` attributes on the `iframe` to change the pixel value dimensions of the iframe displayed on the host page.

```html
<iframe
  src="https://iframe.videodelivery.net/$VIDEOID"
  style="border: none;"
  height="400"
  width="400"
  allow="accelerometer; gyroscope; autoplay; encrypted-media; picture-in-picture;"
  allowfullscreen="true"
></iframe>
```

### Responsive

To make an iframe responsive, enforce an aspect ratio by setting the `iframe` to `position: absolute;` and having it fill a container that uses a calculated `padding-top` percentage.

```html
<!-- padding-top calculation is height / width (assuming 16:9 aspect ratio) -->
<div style="position: relative; padding-top: 56.25%;">
  <iframe
    src="https://iframe.videodelivery.net/$VIDEOID"
    style="border: none; position: absolute; top: 0; height: 100%; width: 100%;"
    allow="accelerometer; gyroscope; autoplay; encrypted-media; picture-in-picture;"
    allowfullscreen="true"
  ></iframe>
</div>
```

## Basic Options

Player options are configured with querystring parameters in the iframe's `src` attribute. For example:

`https://iframe.videodelivery.net/$VIDEOID?autoplay=true&muted=true`

{{<definitions>}}

- `autoplay` {{<prop-meta>}}default: `false`{{</prop-meta>}}

  - Attempts to autoplay the video when the `autoplay` flag is included as a querystring parameter. If you do not want the video to autoplay, set the flag to `autoplay=false`. Note that mobile browsers generally do not support this attribute, and users must tap the screen to begin video playback. We encourage you to consider mobile users or users with Internet usage limits, as some users do not have unlimited Internet access.

    {{<Aside type="note">}}

  Some browsers now prevent videos with audio from playing automatically. You can set `muted` to `true` to allow your videos to autoplay. For more information, refer to [New &lt;video> policies for iOS](https://webkit.org/blog/6784/new-video-policies-for-ios/).

    {{</Aside>}}

- `controls` {{<prop-meta>}}default: `true`{{</prop-meta>}}

  - Shows video controls such as buttons for play/pause and volume controls.

- `defaultTextTrack`

  - Initializes the player with the specified language code's text track enabled. The value should be the BCP-47 language code that was used to [add captions](/stream/how-to/add-captions). If the specified language code has no captions available, the player behaves as though no language code was provided.

    {{<Aside type="note">}}

  The `defaultTextTrack` only works once during initialization. After initialization, the user has full control over their text track settings.

    {{</Aside>}}

  - `letterboxColor` {{<type>}}string{{</type>}}

    - Any valid [CSS color value](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value) provided will be applied to the letterboxing/pillarboxing of the player's UI. This can be set to `transparent` to avoid letterboxing/pillarboxing when not in fullscreen mode.

- `loop` {{<prop-meta>}}default: `false`{{</prop-meta>}}

  - Automatically seeks back to the start upon reaching the end of the video when enabled.

- `muted` {{<prop-meta>}}default: `false`{{</prop-meta>}}

  - Initially silences audio when set.

- `preload` {{<prop-meta>}}default: `none`{{</prop-meta>}}

  - Provides a hint to the browser about what the author thinks will lead to the best user experience. You can specify the value `preload="auto"` to preload the beginning of the video. Excluding the option or using `preload="metadata"` will load the metadata needed to start video playback when requested.

    {{<Aside type="note">}}

  The `<video>` element does not force the browser to follow the value of this option; it is a mere hint. Even though the `preload="none"` option is a valid HTML5 option, the Stream Player will always load some metadata to initialize the player. The amount of data loaded in this case is negligible.

    {{</Aside>}}

- `poster` {{<prop-meta>}}defaults to the first frame of the video{{</prop-meta>}}

  - A URL for an image shown before the video is started or while the video is downloading. If this attribute is not specified, a thumbnail image of the video is shown.

    {{<Aside type="note">}}

  Like all query string parameters, this value must be URI encoded. For example, the thumbnail at `https://videodelivery.net/5d5bc37ffcf54c9b82e996823bffbb81/thumbnails/thumbnail.jpg?time=68s&height=270` can be encoded using JavaScript's `encodeURIComponent()` function to `https%3A%2F%2Fvideodelivery.net%2F5d5bc37ffcf54c9b82e996823bffbb81%2Fthumbnails%2Fthumbnail.jpg%3Ftime%3D68s%26height%3D270`.

   {{</Aside>}}

- `primaryColor`

  - Any valid [CSS color value](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value) applied to certain elements of the player's UI.

    {{<Aside type="note">}}

  Like all query string parameters, this value must be URI encoded. For example, the color value `hsl(120 80% 95%)` can be encoded using JavaScript's `encodeURIComponent()` function to `hsl(120%2080%25%2095%25)`.

    {{</Aside>}}

- `src`

  - The video ID from the video you uploaded to Cloudflare Stream.

- `startTime`

  - A timestamp that specifies the time when playback begins. If a plain number is used, such as `?startTime=123`, the number is interpreted as `123` seconds. More human-readable timestamps can also be used, such as `?startTime=1h12m27s` for `1 hour, 12 minutes, and 27 seconds`.

- `ad-url`

  - The Stream Player supports VAST Tags to insert ads such as prerolls. If you have a VAST tag URI, you can pass it to the Stream Player by setting the `ad-url` parameter. The URI must be encoded using a function like JavaScript's `encodeURIComponent()`.  

{{</definitions>}}

## Debug Info
  The Stream player Debug menu can be shown and hidden using the key combination `Shift-D` while the video is playing.