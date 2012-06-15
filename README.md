Codeigniter-Image-Helper
========================

Dynamic Generate an Image Thumbnail.


Example 
--------

<?php

// load Image Lib, it not required if image library is set on ./config/autoload.php

$this->load->library('image_lib');

// retrun image thumbnail path. with Base url.

$thumb_img = image_thumb(array("width"=>"200","height"=>"200","media/image/photo.jpg");

// if base path is not needed then add last parameter false

// ex. image_thumb(array("width"=>"200","height"=>"200","media/image/photo.jpg",false);


// with html helper

// echo img($thumb_img);

// html

// <img src="<?php echo $thumb_img;?> " alt="" />


