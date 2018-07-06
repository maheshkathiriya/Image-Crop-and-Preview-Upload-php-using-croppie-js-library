# Image-Crop-and-Preview-Upload-php-using-croppie-js-library
Image Crop and Preview Upload php using croppie js library, Image Crop Preview &amp; Upload using JQuery with PHP Ajax



Canvas Based Image Cropping Library For jQuery - Croppie

Image Crop Preview & Upload using JQuery with PHP Ajax


Image Crop and Preview Upload php using croppie js library



Croppie is an Html5 canvas based image cropping library that lets you create a square or circular viewport permitting to visually resize an image while preserving aspect ratio and perform a crop. Also can be used as a jQuery plugin.

Basic usage:

1. Include the croppie library and other resources in your html file.

1 <link rel="stylesheet" href="croppie.css">
2 <script src="//code.jquery.com/jquery-2.1.4.min.js"></script>
3 <script src="croppie.js"></script>
2. Create an empty container for the image cropper.

1 <div id="demo"></div>
3. Enable the image cropper using jQuery method.

1 $('#demo').croppie();
4. Available options to customize your image cropper.
<pre>
<script>
    $(document).ready(function () {

        $image_crop = $('#image_demo').croppie({
            enableExif: true,
            viewport: {
                width: 500,
                height: 300,
                type: 'square' //circle
            },
            boundary: {
                width: 600,
                height: 400
            }
        });

        $('#upload_image').on('change', function () {

            var reader = new FileReader();

            reader.onload = function (event) {

                result = event.target.result;
                arrTarget = result.split(';');
                tipo = arrTarget[0];

                if (tipo == 'data:image/jpeg' || tipo == 'data:image/png') {
                    //alert('valid image');
                    $('#uploadimageModal').modal('show');
                } else {
                    // Setup the clear functionality
                    var control = $("#upload_image");
                    control.replaceWith(control.val('').clone(true));
                    alert('Accept only .jpg or .png image types');
                }

                $image_crop.croppie('bind', {
                    url: event.target.result
                }).then(function () {
                    console.log('jQuery bind complete');
                });
            }
            reader.readAsDataURL(this.files[0]);
        });

        $('.crop_image').click(function (event) {
            $image_crop.croppie('result', {
                type: 'canvas',
                size: 'viewport'
            }).then(function (response) {
                $.ajax({
                    url: "upload.php",
                    type: "POST",
                    data: {"image": response},
                    success: function (data)
                    {
                        $('#uploadimageModal').modal('hide');
                        $('#uploaded_image').html(data);
                    }
                });
            })
        });

        $image_crop.on('update.croppie', function (ev, data) {
            //console.log('jquery update', ev, data);
            $image_crop.croppie('result', {
                type: 'rawcanvas',
                circle: false,
                // size: { width: 300, height: 300 },
                format: 'png'
            }).then(function (canvas) {
                $('#prev-img').attr('src', canvas.toDataURL());
                //console.log(canvas.toDataURL());
            });
        });

        $('.js-main-image').on('click', function (ev) {
            $image_crop.croppie('result', {
                type: 'rawcanvas',
                circle: false,
                // size: { width: 300, height: 300 },
                format: 'png'
            }).then(function (canvas) {
                $('#prev-img').attr('src', canvas.toDataURL());
                //console.log(canvas.toDataURL());
            });
        });

    });
</script>
</pre>
Download on github

