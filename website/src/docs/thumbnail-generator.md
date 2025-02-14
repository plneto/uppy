---
type: docs
order: 2
title: "Thumbnail Generator"
module: "@uppy/thumbnail-generator"
permalink: docs/thumbnail-generator/
category: "Miscellaneous"
tagline: "generate preview thumbnails for images to be uploaded"
---

`@uppy/thumbnail-generator` generates proportional thumbnails (file previews) for images that are added to Uppy.

This plugin is included by default with the [Dashboard](/docs/dashboard) plugin, and can also be useful to display image previews in a custom UI.

```js
import ThumbnailGenerator from '@uppy/thumbnail-generator'

uppy.use(ThumbnailGenerator, {
  thumbnailWidth: 200,
  // thumbnailHeight: 200 // optional, use either width or height,
  waitForThumbnailsBeforeUpload: false,
})
```

> Now, the `file.preview` property will contain a URL to the thumbnail and `thumbnail:generated` event will be emitted, see below for details.

## Installation

This plugin is published as the `@uppy/thumbnail-generator` package.

Install from NPM:

```shell
npm install @uppy/thumbnail-generator
```

In the [CDN package](/docs/#With-a-script-tag), the plugin class is available on the `Uppy` global object:

```js
const { ThumbnailGenerator } = Uppy
```

## Options

The `@uppy/thumbnail-generator` plugin has the following configurable options:

```js
uppy.use(ThumbnailGenerator, {
  id: 'ThumbnailGenerator',
  thumbnailWidth: 200,
  thumbnailHeight: 200,
  thumbnailType: 'image/jpeg',
  waitForThumbnailsBeforeUpload: false,
})
```

### `id: 'ThumbnailGenerator'`

A unique identifier for this plugin. It defaults to `'ThumbnailGenerator'`.

### `locale: {}`

<!-- eslint-disable no-restricted-globals, no-multiple-empty-lines -->

```js
export default {
  strings: {
    generatingThumbnails: 'Generating thumbnails...',
  },
}

```

### `thumbnailWidth: 200`

Width of the resulting thumbnail. Default thumbnail dimension is 200px. Thumbnails are always proportional and not cropped. If width is provided, height is calculated automatically to match ratio.

If both width and height are given, only width is taken into account.

> uppy.use(ThumbnailGenerator, { thumbnailWidth: 300 }) will produce a 300px width thumbnail with calculated height to match ratio.

### `thumbnailHeight: null`

Height of the resulting thumbnail. Default thumbnail dimension is 200px. Thumbnails are always proportional and not cropped. If height is provided, width is calculated automatically to match ratio.

If both width and height are given, only width is taken into account.

> uppy.use(ThumbnailGenerator, { thumbnailHeight: 300 }) will produce a 300px height thumbnail with calculated width to match ratio.
>
> uppy.use(ThumbnailGenerator, { thumbnailWidth: 300, thumbnailHeight: 300 }) will produce a 300px width thumbnail with calculated height to match ratio (and ignore the given height).
>
> See issue [#979](https://github.com/transloadit/uppy/issues/979) and [#1096](https://github.com/transloadit/uppy/pull/1096) for details on this feature.

### `thumbnailType: 'image/jpeg'`

MIME type of the resulting thumbnail. Default thumbnail MIME type is `image/jpeg`. This is useful if you want to support transparency in your thumbnails by switching to `image/png`.

### `waitForThumbnailsBeforeUpload: false`

Whether to wait for all thumbnails to be ready before starting the upload. If set to `true`, Thumbnail Generator will invoke Uppy’s internal processing stage and wait for `thumbnail:all-generated` event, before proceeding to the uploading stage.

This is useful because Thumbnail Generator also adds EXIF data to images, and if we wait until it’s done processing, this data will be available on the server after the upload.

## Event

`thumbnail:generated` event is emitted with `file` and `preview` local url as arguments:

```js
uppy.on('thumbnail:generated', (file, preview) => {
  const img = document.createElement('img')
  img.src = preview
  img.width = 100
  document.body.appendChild(img)
})
```
