# PLACE this code in template.php file replacing 'phptemplate' by template name

/**
 * OVERRIDES IMAGES TEMPLATE TO TAKE CARE OF EMPTY ALT
 */

function phptemplate_imagefield_formatter_image_plain($element) {
  // Inside a view $element may contain null data. In that case, just return.
  if (empty($element['#item']['fid'])) {
    return '';
  }

  $field = content_fields($element['#field_name']);
  $item = $element['#item'];

  $item['data']['alt'] = isset($item['data']['alt']) && !empty($item['data']['alt']) ? $item['data']['alt'] : _phptemplate_image_alt($element);
  $item['data']['title'] = isset($item['data']['title']) ? $item['data']['title'] : NULL;

  $class = 'imagefield imagefield-'. $field['field_name'];
  return  theme('imagefield_image', $item, $item['data']['alt'], $item['data']['title'], array('class' => $class));
}

function phptemplate_imagecache_formatter_default($element) {
  // Inside a view $element may contain NULL data. In that case, just return.
  if (empty($element['#item']['fid'])) {
    return '';
  }

  // Extract the preset name from the formatter name.
  $presetname = substr($element['#formatter'], 0, strrpos($element['#formatter'], '_'));
  $style = 'linked';
  $style = 'default';

  $item = $element['#item'];
  $item['data']['alt'] = isset($item['data']['alt']) && !empty($item['data']['alt']) ? $item['data']['alt'] : _phptemplate_image_alt($element);
  $item['data']['title'] = isset($item['data']['title']) ? $item['data']['title'] : NULL;

  $class = "imagecache imagecache-$presetname imagecache-$style imagecache-{$element['#formatter']}";
  return theme('imagecache', $presetname, $item['filepath'], $item['data']['alt'], $item['data']['title'], array('class' => $class));
}

function phptemplate_imagecache_formatter_linked($element) {
  // Inside a view $element may contain NULL data. In that case, just return.
  if (empty($element['#item']['fid'])) {
    return '';
  }

  // Extract the preset name from the formatter name.
  $presetname = substr($element['#formatter'], 0, strrpos($element['#formatter'], '_'));
  $style = 'linked';

  $item = $element['#item'];
  $item['data']['alt'] = isset($item['data']['alt']) && !empty($item['data']['alt']) ? $item['data']['alt'] : _phptemplate_image_alt($element);
  $item['data']['title'] = isset($item['data']['title']) ? $item['data']['title'] : NULL;

  $imagetag = theme('imagecache', $presetname, $item['filepath'], $item['data']['alt'], $item['data']['title']);
  $path = empty($item['nid']) ? '' : 'node/'. $item['nid'];
  $class = "imagecache imagecache-$presetname imagecache-$style imagecache-{$element['#formatter']}";
  return l($imagetag, $path, array('attributes' => array('class' => $class), 'html' => TRUE));
}

function phptemplate_imagecache_formatter_imagelink($element) {
  // Inside a view $element may contain NULL data. In that case, just return.
  if (empty($element['#item']['fid'])) {
    return '';
  }

  // Extract the preset name from the formatter name.
  $presetname = substr($element['#formatter'], 0, strrpos($element['#formatter'], '_'));
  $style = 'imagelink';

  $item = $element['#item'];
  $item['data']['alt'] = isset($item['data']['alt']) && !empty($item['data']['alt']) ? $item['data']['alt'] : _phptemplate_image_alt($element);
  $item['data']['title'] = isset($item['data']['title']) ? $item['data']['title'] : NULL;

  $imagetag = theme('imagecache', $presetname, $item['filepath'], $item['data']['alt'], $item['data']['title']);
  $path = file_create_url($item['filepath']);
  $class = "imagecache imagecache-$presetname imagecache-$style imagecache-{$element['#formatter']}";
  return l($imagetag, $path, array('attributes' => array('class' => $class), 'html' => TRUE));
}

function _phptemplate_image_alt($element) {
	$item = $element['#item'];
	if(empty($item['data']['alt'])) {
		if(isset($item['data']['description']) && !empty($item['data']['description'])) {
			$item['data']['alt'] = $item['data']['description'];
		}
		else if (isset($item['data']['title']) && !empty($item['data']['title'])) {
			$item['data']['alt'] = $item['data']['title'];
		}
		else if(isset($element['#node']->title)) {
			$item['data']['alt'] = $element['#node']->title;
		}
		else if(isset($element['#node']->node_title)) {
			$item['data']['alt'] = $element['#node']->node_title;
		}
	}
	return $item['data']['alt'];
}