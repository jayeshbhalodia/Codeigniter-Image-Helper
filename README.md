Codeigniter-Image-Helper
========================

Dynamic Generate an Image Thumbnail.


------------------------------------------------------
@code
------------------------------------------------------
<?php

if (!defined('BASEPATH'))
    exit('No direct script access allowed');

/**
 * @author Jayesh Bhalodia {jayeshbhalodia@ymail.com}
 * 
 *  image manipulation helper
 *  it should be used for generate dynamic thumbnail with its associated parameters. 
 */

/**
 * 
 * create thumbnail 
 * 
 * @author Jayesh Bhalodia
 * @param type $size array like array('width'=>'128','height'=>'128');
 * @param type $file_source original file source path.
 * @see 
 *    - image_lib library should be set on autoload or manually loaded before this(image_thumb()).
 *    - gd2 enable on server.
 * @return string file path. 
 */
function image_thumb($size, $file_source) {
    // get master object
    $CI = & get_instance();

    // Image configuration 
    $config = array();

    $file_path_explode = explode('/', $file_source);
    $file_name = end($file_path_explode);

    // remove last element of array
    array_pop($file_path_explode);

    $new_direct_path = implode('/', $file_path_explode);
    $new_direct_path .= '/' . $size['width'] . 'x' . $size['height'];

    $file_path = $new_direct_path . '/' . $file_name;

    // if file has been exists then directly return that path.
    if (file_exists($file_path)) {
        return $file_path;
    }

    // create directory if not exists
    // directory name would be (widht)x(height) ex. 128x128
    if (!is_dir($new_direct_path)) {
        if (!mkdir($new_direct_path)) {
            if (ENVIRONMENT == 'dev') {
                exit('directory create permission is denied !');
            } else {
                // exit(0);
            }
        }
    }

    // set image configuration.
    $config['image_library'] = 'gd2';
    $config['source_image'] = $file_source;
    $config['create_thumb'] = TRUE;
    $config['maintain_ratio'] = TRUE;
    $config['new_image'] = $file_path;
    $config['thumb_marker'] = '';
    $config['quality'] = '100%';
    $config['width'] = $size['width'];
    $config['height'] = $size['height'];

    // init a configuration of image
    $CI->image_lib->__construct($config);

    if (!$CI->image_lib->resize()) {
        echo $CI->image_lib->display_errors();
    } else {
        return $file_path;
    }
    // clear last configuration.
    $CI->image_lib->clear();
}