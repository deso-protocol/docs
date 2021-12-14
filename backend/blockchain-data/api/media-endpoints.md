---
description: >-
  Description of endpoints used to manage media uploads for posts on the DeSo
  blockchain
---

# Media Endpoints

{% swagger method="post" path="" baseUrl="/api/v0/upload-image" summary="Upload Image" %}
{% swagger-description %}
Uploads an image to be included in a post and returns the URL where the image is stored. This endpoint also handles the resizing of the image.

Note that the request body should have `multipart/form-data` as the content type.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/media.go#L111).

Example usages in frontend:\
&#x20; \- Make request to [Upload Image](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L825)\
&#x20; \- Use UploadImage to [upload an image when a user is making a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/feed/feed-create-post/feed-create-post.component.ts#L279)
{% endswagger-description %}

{% swagger-parameter in="body" name="UserPublicKeyBase58Check" type="String" required="true" %}
Public key of the user uploading the image.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="JWT" type="String" required="true" %}
JWT of the user uploading the image.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="file" type="File" required="true" %}
image file to upload. Must be gif, jpeg, png, or webp file. Must be less than 10 MB
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully uploaded image and response has URL at which image can be found" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "ImageURL": "https://images.deso.org/675fc5d13f397d6ce7801b0a76ca928822a768b606d16df1eb015b2e84ed81e5.gif"
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
| Name     | Type   | Description                                  |
| -------- | ------ | -------------------------------------------- |
| ImageURL | String | URL at which the uploaded image can be found |
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/upload-video" summary="Upload Video" %}
{% swagger-description %}
UploadVideo creates a one-time tokenized URL that can be used to upload larger video files using the tus protocol. The client uses the Location header in the response from this function to upload the file. The client uses the Stream-Media-Id header in the response from cloudflare to understand how to access the file for streaming.&#x20;

For more details, see the Cloudflare documentation on direct creator uploads [here](https://developers.cloudflare.com/stream/uploading-videos/direct-creator-uploads#using-tus-recommended-for-videos-over-200mb)

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/media.go#L306).

For an example of uploading a video using this endpoint and the tus protocol, see the [implementation in frontend](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/feed/feed-create-post/feed-create-post.component.ts#L291).\
\
After the upload finishes, you can check if the video is ready to be streamed by hitting the [#get-video-status](media-endpoints.md#get-video-status "mention")endpoint
{% endswagger-description %}

{% swagger-parameter in="header" name="Upload-Length" type="Number" required="true" %}
Length of video to be uploaded in bytes
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Upload-Metadata" type="JSON" %}
Arbitrary metadata values - see 

[cloudflare documentation for more details](https://developers.cloudflare.com/stream/uploading-videos/upload-video-file#supported-options-in-upload-metadata)


{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully created one-time tokenized URL that can be used to upload a video" %}
The Location header specifies the one-time tokenized URL. The Stream-Media-Id header is the ID used to stream the video from cloudflare after uploading the video.
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="/api/v0/get-video-status/{videoId}" summary="Get Video Status" %}
{% swagger-description %}
Get Video Status queries cloudflare's API to see if a video is ready to be streamed. This is useful in showing a preview of an uploaded video to an end-user when they are creating a post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/media.go#L372).

Example usage in frontend:\
&#x20; \- Make request to [Get Video Status](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L2228)\
&#x20; \- Use GetVideoStatus to [poll and see if a video is ready to be streamed after a user finished uploading it.](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/lib/services/stream/cloudflare-stream-service.ts#L31)
{% endswagger-description %}

{% swagger-parameter in="path" name="videoId" type="String" required="true" %}
videoId retrieved from the 

`stream-media-id`

 header when uploading a video
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully queried Cloudflare for the status of the video" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "ReadyToStream": true // If true, video is ready to stream. If false, video is not ready to strea
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
| Name          | Type    | Description                                                                                            |
| ------------- | ------- | ------------------------------------------------------------------------------------------------------ |
| ReadyToStream | Boolean | If true, the video is ready to be streamed. If false, the video is still being processed by cloudflare |
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-full-tiktok-url" summary="Get Full TikTok URL" %}
{% swagger-description %}
Given a short video ID of a TikTok, find the URL that can be used to embed this video. The short URL users get when copying a link to a TikTok from TikTok's mobile app isn't embeddable, so this endpoint allows us to find the desktop version of the URL from which we can construct an embeddable version of the URL.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/media.go#L244).

Example usages in frontend:\
&#x20; \- Make request to [Get Full TikTok URL](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1962)\
&#x20; \- Use GetFullTikTokURL to [get an embeddable URL for the short form TikTok url](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/lib/services/embed-url-parser-service/embed-url-parser-service.ts#L147)
{% endswagger-description %}

{% swagger-parameter in="body" name="TikTokShortVideoID" type="String" required="true" %}
Video ID found at the end of a URL copied from the TikTok mobile app.&#x20;



For example, `TTPd2Eobq3` is the VideoID in https://vm.tiktok.com/TTPd2Eobq3/\`
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved the embeddable version of the mobile TikTok URL provided" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "FullTikTokURL": "https://m.tiktok.com/v/7037137657872403718.html?_d=secCgYIASAHKAESPgo8T1WVnwCQv6PNczjlfPqZ%2BVrGtkECbrVIwDlSfs8Eubr5IYCCt7sen3HRJwDN44tt0IeLho5JoaUMWgAnGgA%3D&checksum=be13618b6e8d0eacdf95a8abd952ca14e997a5af0126908fb195dca7ab5082d5&language=en&preview_pb=0&sec_user_id=MS4wLjABAAAACRLophxm1bvJ6oYFi4m52AIzepq8Naslxs3ATZs1YCLXomDfhhDOvxsW9DemYFYU&share_app_id=1233&share_item_id=7037137657872403718&share_link_id=2E462ECA-A6B5-44CF-9A9A-BD3F25E5F6FF&source=h5_m&timestamp=1638557967&tt_from=copy&u_code=djcd82ge94k75m&user_id=6980837453575472134&utm_campaign=client_share&utm_medium=ios&utm_source=copy"
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
| Name          | Type   | Description                                                                                          |
| ------------- | ------ | ---------------------------------------------------------------------------------------------------- |
| FullTikTokURL | String | Desktop version of the mobile TikTok URL provided in the request body that can be embedded in a post |
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}
