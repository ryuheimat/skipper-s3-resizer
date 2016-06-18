# skipper-s3-resizer

This is extended **skipper-s3-resizer**  based on - [Original skipper-s3-resize](https://github.com/basicinception/skipper-s3-resize "Original")

## New features

### Adds optional configurations: 

 - `prefix` - S3 object prefix (Default: `''`)
 - `filename` - Custom function for naming files to be uploaded (Default: `<uuid>.<ext>`).
 - `stretch` - Whether to stretch or cover image (Default: `false`).

### Changed resizing technique:

- If width and height are not specified, image will be uploaded in original size
- If only one dimension is specified, downscale (keeps aspect-ratio)
- If both dimensions are specified
  - And `stretch == true`, downscale (breaking aspect-ratio)
  - And `stretch == false`, downscale ([cover] (https://github.com/paolochiodi/resizer#cover))

Based on SailsJS Skipper S3 adapter for receiving [upstreams](https://github.com/balderdashy/skipper#what-are-upstreams) with a twist: Images will be resized before uploading using GraphicMagick.

## Installation

1. Make sure you have graphicmagick installed.
2. Make sure you have skipper itself [installed as your body parser](http://beta.sailsjs.org/#/documentation/concepts/Middleware?q=adding-or-overriding-http-middleware).  This is the default configuration in [Sails](https://github.com/balderdashy/sails) as of v0.10.

3. Run `npm install skipper-s3-resizer --save`

## Usage
1. In Controller:
```javascript
  upload: function(req, res) {
    req.file('image').upload({
      adapter: require('skipper-s3-resizer'), // Required
      key: <YOUR_S3_ACCESS_KEY>, // Required
      secret: <YOUR_S3_SECRET_KEY>, // Required
      bucket: <YOUR_S3_BUCKET>, // Required
      prefix: 'prefixed',
      filename: function (base, ext) {
        return sails.shortid.generate().concat(ext);
      },
      resize: {
        stretch: false,
        width: <WIDTH>,
        height: <HEIGHT>
      }
    }, function(err, uploadedFiles) {
      if(err) return res.serverError(err);
        res.ok();
      });
    }
```

## References
1. [Original skipper-s3-resize](https://github.com/basicinception/skipper-s3-resize "Original")
2. [Skipper documentations.](https://github.com/balderdashy/skipper#uploading-files-to-s3)
