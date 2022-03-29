---
title: Download videos
pcx-content-type: how-to
weight: 6
meta:
  title: Download videos
---

# Download videos

Videos uploaded to Stream can be streamed using HLS/DASH, but for certain use cases (such as offline viewing), you may want to download the MP4. You can enable MP4 support on a per video basis by following the steps below.

1. Enable MP4 support by making a `POST` request to the `/downloads` endpoint.
2. Save the MP4 URL provided by the response to the `/downloads` endpoint. This MP4 URL will become functional when the MP4 is ready.
3. Poll the `/downloads` endpoint until the `status` field is set to `ready` to inform you when the MP4 is available. You can now use the saved MP4 URL.

{{<Aside type="note">}}

You cannot download the exact input file that you uploaded. However, depending on your use case, you can get encoded MP4s for use cases like offline viewing.

{{</Aside>}}

## Generate downloadable MP4 files

Enable downloads for an uploaded video after it is ready to view by making an HTTP request to the `/downloads` endpoint.

To get notified when a video is ready to view, refer to [Webhooks for process notifications](/stream/how-to/use-webhooks).

The downloads API response will include all available download types for the video, the download URL for each type, and the processing status of the download file.

<TableWrap>

<table>
  <thead>
  <tr>
   <th><strong>Command</strong>
   </th>
   <th><strong>Method</strong>
   </th>
   <th><strong>Endpoint</strong>
   </th>
  </tr>
  </thead>
  <tbody>
  <tr>
   <td><a href="https://api.cloudflare.com/#stream-mp4-downloads-create-downloads">Create downloads</a>
   </td>
   <td><Code>POST</Code>
   </td>
   <td><Code>accounts/:account_identifier/stream/:video_identifier/downloads</Code>
   </td>
  </tr>
  <tr>
   <td><a href="https://api.cloudflare.com/#stream-mp4-downloads-list-downloads">Get links to download videos</a>
   </td>
   <td><Code>GET</Code>
   </td>
   <td><Code>accounts/:account_identifier/stream/:video_identifier/downloads</Code>
   </td>
  </tr>
  </tbody>
</table>

</TableWrap>

## Retrieve the downloads

The generated MP4 download files can be retrieved via the link in the download API response.

```bash
curl -L https://videodelivery.net/$VIDEOID/downloads/default.mp4 > download.mp4
```

## Secure video downloads

If your video is public, the MP4 is also publicly accessible, but if your video is private and requires a signed URL for viewing, the MP4 is not publicly accessible. 

To access the MP4 for a private video, generate a signed URL just as you would for regular viewing with an additional flag called `downloadable` set to `true`. Download links will not work for videos which already require signed URLs if the `downloadable` flag is not present in the token.

For more information about using signed URLs with videos, refer to [Secure your Stream](/stream/how-to/secure-your-stream).

```json
---
highlight: [6]
---
{
    "sub": $VIDEOID,
    "kid": $KEYID,
    "exp": 1537460365,
    "nbf": 1537453165,
    "downloadable": true,
    "accessRules": [
      {
        "type": "ip.geoip.country",
        "action": "allow",
        "country": [
          "GB"
        ]
      },
      {
        "type": "any",
        "action": "block"
      }
    ]
  }
```

## Billing for MP4 Downloads

MP4 downloads are billed in the same way as video streams. You will be billed for the duration of the video each time the MP4 for the video is downloaded. For example, if you have a 10 minute video that is downloaded 100 times during the month, the downloads will count as 1000 minutes of minutes served.

You will not incur any additional cost for storage when you enable MP4s.