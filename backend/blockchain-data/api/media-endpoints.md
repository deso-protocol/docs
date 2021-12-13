# Media Endpoints

## Upload Image

```
POST /api/v0/upload-image
```

Uploads an image to be included in a post and returns the URL where the image is stored. This endpoint also handles the resizing of the image.

Note that the request body should have `multipart/form-data` as the content type.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/media.go#L111).

Example usages in frontend:

* Make request to [Upload Image](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L825)
* Use UploadImage to [upload an image when a user is making a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/feed/feed-create-post/feed-create-post.component.ts#L279)

**Parameters**

| Name                     | Type   | Description                                                                         | Required | Restrictions            |
| ------------------------ | ------ | ----------------------------------------------------------------------------------- | -------- | ----------------------- |
| JWT                      | string | JWT of the user uploading the image.                                                | y        |                         |
| UserPublicKeyBase58Check | string | Public key of the user uploading the image.                                         | y        |                         |
| file                     | File   | image file to upload. Must be gif, jpeg, png, or webp file. Must be less than 10 MB | y        | Must be less than 10 MB |

**Response**

```json5
{
  "ImageURL": "https://images.deso.org/675fc5d13f397d6ce7801b0a76ca928822a768b606d16df1eb015b2e84ed81e5.gif"
}
```

## Upload Video

```
POST /api/v0/upload-video
```

UploadVideo creates a one-time tokenized URL that can be used to upload larger video files using the tus protocol. The client uses the Location header in the response from this function to upload the file. The client uses the Stream-Media-Id header in the response from cloudflare to understand how to access the file for streaming.&#x20;

For more details, see the Cloudflare documentation on direct creator uploads [here](https://developers.cloudflare.com/stream/uploading-videos/direct-creator-uploads#using-tus-recommended-for-videos-over-200mb)

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/media.go#L306).

For an example of uploading a video using this endpoint and the tus protocol, see the [implementation in frontend](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/feed/feed-create-post/feed-create-post.component.ts#L291).

**Request Headers**

| Name            | Description                                                                                                                                                                                   | Type |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- |
| Upload-Length   | Length of video to be uploaded in bytes                                                                                                                                                       |      |
| Upload-Metadata | Arbitrary metadata values - see [cloudflare documentation for more details](https://developers.cloudflare.com/stream/uploading-videos/upload-video-file#supported-options-in-upload-metadata) |      |

## Get Video Status

```
GET /api/v0/get-video-status/{videoId}
```

Get Video Status queries cloudflare's API to see if a video is ready to be streamed. This is useful in showing a preview of an uploaded video to an end-user when they are creating a post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/media.go#L372).

Example usage in frontend:

* Make request to [Get Video Status](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L2228)
* Use GetVideoStatus to [poll and see if a video is ready to be streamed after a user finished uploading it.](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/lib/services/stream/cloudflare-stream-service.ts#L31)

**URL Parameters**

| Name    | Type   | Description                                                                |
| ------- | ------ | -------------------------------------------------------------------------- |
| videoId | string | videoId retrieved from the `stream-media-id` header when uploading a video |

**Response**

```json5
{
  "ReadyToStream": true // If true, video is ready to stream. If false, video is not ready to strea
}
```

## Get Full TikTok URL

```
POST /api/v0/get-full-tiktok-url
```

Given a short video ID of a TikTok, find the URL that can be used to embed this video. The short URL users get when copying a link to a TikTok from TikTok's mobile app isn't embeddable, so this endpoint allows us to find the desktop version of the URL from which we can construct an embeddable version of the URL.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/media.go#L244).

Example usages in frontend:

* Make request to [Get Full TikTok URL](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1962)
* Use GetFullTikTokURL to [get an embeddable URL for the short form TikTok url](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/lib/services/embed-url-parser-service/embed-url-parser-service.ts#L147)

**Parameters**

| Name               | Type   | Description                                                                                                                                                                          | Required | Restrictions |
| ------------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------- | ------------ |
| TikTokShortVideoID | string | <p>Video ID found at the end of a URL copied from the TikTok mobile app. </p><p></p><p>For example, <code>TTPd2Eobq3</code> is the VideoID in https://vm.tiktok.com/TTPd2Eobq3/`</p> | y        |              |

**Response**

```json5
{
  "FullTikTokURL": "https://m.tiktok.com/v/7037137657872403718.html?_d=secCgYIASAHKAESPgo8T1WVnwCQv6PNczjlfPqZ%2BVrGtkECbrVIwDlSfs8Eubr5IYCCt7sen3HRJwDN44tt0IeLho5JoaUMWgAnGgA%3D&checksum=be13618b6e8d0eacdf95a8abd952ca14e997a5af0126908fb195dca7ab5082d5&language=en&preview_pb=0&sec_user_id=MS4wLjABAAAACRLophxm1bvJ6oYFi4m52AIzepq8Naslxs3ATZs1YCLXomDfhhDOvxsW9DemYFYU&share_app_id=1233&share_item_id=7037137657872403718&share_link_id=2E462ECA-A6B5-44CF-9A9A-BD3F25E5F6FF&source=h5_m&timestamp=1638557967&tt_from=copy&u_code=djcd82ge94k75m&user_id=6980837453575472134&utm_campaign=client_share&utm_medium=ios&utm_source=copy"
}
```
