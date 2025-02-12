Color Thief PHP
==============

[![Latest Stable Version](https://img.shields.io/packagist/v/ksubileau/color-thief-php.svg?style=flat-square)](https://packagist.org/packages/ksubileau/color-thief-php)
[![Build Status](https://img.shields.io/travis/ksubileau/color-thief-php.svg?style=flat-square)](https://travis-ci.org/ksubileau/color-thief-php)
[![GitHub issues](https://img.shields.io/github/issues/ksubileau/color-thief-php.svg?style=flat-square)](https://github.com/ksubileau/color-thief-php/issues)
[![HHVM](https://img.shields.io/hhvm/ksubileau/color-thief-php.svg?style=flat-square)](https://travis-ci.org/ksubileau/color-thief-php)
[![Packagist](https://img.shields.io/packagist/dm/ksubileau/color-thief-php.svg?style=flat-square)](https://packagist.org/packages/ksubileau/color-thief-php)
[![License](https://img.shields.io/packagist/l/ksubileau/color-thief-php.svg?style=flat-square)](https://packagist.org/packages/ksubileau/color-thief-php)

A PHP class for **grabbing the color palette** from an image. Uses PHP and GD or Imagick libraries to make it happen.

It's a PHP port of the [Color Thief Javascript library](http://github.com/lokesh/color-thief), using the MMCQ (modified median cut quantization) algorithm from the [Leptonica library](http://www.leptonica.com/).

[**See examples**](http://www.kevinsubileau.fr/projets/color-thief-php?utm_campaign=github&utm_term=color-thief-php_readme)

## Requirements

- PHP >8.0
- One or more PHP extensions for image processing:
  - GD >= 2.0
  - Imagick >= 2.0 (but >= 3.0 for CMYK images)
  - Gmagick >= 1.0
- Supports JPEG, PNG and GIF images.

## How to use
### Install via Composer
The recommended way to install Color Thief is through
[Composer](http://getcomposer.org):
```bash
composer require ksubileau/color-thief-php
```

### Get the dominant color from an image
```php
require_once 'vendor/autoload.php';
use ColorThief\ColorThief;
$dominantColor = ColorThief::getColor($sourceImage);
```
The `$sourceImage` variable must contain either the absolute path of the image on the server, a URL to the image, a GD resource containing the image, an [Imagick](http://www.php.net/manual/en/class.imagick.php) image instance, a [Gmagick](http://www.php.net/manual/en/class.gmagick.php) image instance, or an image in binary string format.

```php
ColorThief::getColor($sourceImage[, $quality=10, $area=null])
returns array(r: num, g: num, b: num)
```

This function returns an array of three integer values, corresponding to the RGB values (Red, Green & Blue) of the dominant color.

You can pass an additional argument (`$quality`) to adjust the calculation accuracy of the dominant color. 1 is the highest quality settings, 10 is the default. But be aware that there is a trade-off between quality and speed/memory consumption !
If the quality settings are too high (close to 1) relative to the image size (pixel counts), it may **exceed the memory limit** set in the PHP configuration (and computation will be slow).

You can also pass another additional argument (`$area`) to specify a rectangular area in the image in order to get dominant colors only inside this area. This argument must be an associative array with the following keys :
- `$area['x']` : The x-coordinate of the top left corner of the area. Default to 0.
- `$area['y']` : The y-coordinate of the top left corner of the area. Default to 0.
- `$area['w']` : The width of the area. Default to the width of the image minus x-coordinate.
- `$area['h']` : The height of the area. Default to the height of the image minus y-coordinate.


### Build a color palette from an image

In this example, we build an 8 color palette.

```php
require_once 'vendor/autoload.php';
use ColorThief\ColorThief;
$palette = ColorThief::getPalette($sourceImage, 8);
```

Again, the `$sourceImage` variable must contain either the absolute path of the image on the server, a URL to the image, a GD resource containing the image, an [Imagick](http://www.php.net/manual/en/class.imagick.php) image instance, a [Gmagick](http://www.php.net/manual/en/class.gmagick.php) image instance, or an image in binary string format.

```php
ColorThief::getPalette($sourceImage[, $colorCount=10, $quality=10, $area=null])
returns array(array(num, num, num), array(num, num, num), ... )
```

The `$colorCount` argument determines the size of the palette; the number of colors returned. If not set, it defaults to 10.

The `$quality` and `$area` arguments work as in the previous function.

## Credits and license

### Author
by Toby Powell-Blyth

Based on the fabulous work done by Lokesh Dhakar
[kevinsubileau.fr](http://www.kevinsubileau.fr) 
[lokeshdhakar.com](http://www.lokeshdhakar.com)
[twitter.com/lokesh](http://twitter.com/lokesh)

### Thanks
Kevin Subileau - For [porting to PHP 8](https://github.com/ksubileau/color-thief-php)
* Lokesh Dhakar - For creating the [original project](http://github.com/lokesh/color-thief).
* Nick Rabinowitz - For creating quantize.js.

### License
Licensed under the [Creative Commons Attribution 2.5 License](http://creativecommons.org/licenses/by/2.5/)

* Free for use in both personal and commercial projects.
* Attribution requires leaving author name, author homepage link, and the license info intact.
