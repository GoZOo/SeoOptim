# PLACE this code in template.php file replacing 'phptemplate' by template name

/**
 * OVERRIDES IMAGES TEMPLATE TO TAKE CARE OF EMPTY ALT
 */

function phptemplate_preprocess_image_formatter(&$variables) {

  $item = $variables['item'];
  if(empty($item['alt']) && is_array($variables['path']) && isset($variables['path']['options']['entity'])) {
    $variables['item']['alt'] = $variables['path']['options']['entity']->title;
  }
  elseif(empty($item['alt'])) {
    $variables['item']['alt'] = $variables['item']['filename'];
  }

}