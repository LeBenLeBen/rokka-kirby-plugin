# Kirby Rokka

## WARNING

This is more a proof of concept, than something production ready. Use it at your own risk and extend it to make it very useful.

## Requirements

- [PHP 7.0](https://php.net) 
- [**Kirby**](https://getkirby.com/) 2.5+ 
- [Rokka API key](https://rokka.io/en/signup/) (trial available).

## Installation

### [Kirby CLI](https://github.com/getkirby/cli)

FIXME: Not done yet ;)

```
kirby plugin:install rokka-io/rokka-kirby-plugin
```

### Git Submodule


```
$ git submodule add https://github.com/rokka-io/rokka-kirby-plugin.git site/plugins/rokka
```

### Copy and Paste

1. [Download](https://github.com/rokka-io/rokka-kirby-plugin/archive/master.zip) the contents of this repository as ZIP-file.
2. Rename the extracted folder to `rokka` and copy it into the `site/plugins/` directory in your Kirby project.

### Composer install

This is needed for all the libraries rokka needs. Do this in your kirby directory.
```
composer require rokka/client
```

## Usage

In your `site/config.php` activate the plugin and set the [ROKKA API key](https://rokka.io/en/signup/) .

```php
c::set('plugin.rokka.enabled', true); 
c::set('plugin.rokka.organisation', 'YOUR_ORG_NAME_HERE'); 
c::set('plugin.rokka.apikey', 'YOUR_API_KEY_HERE');
```

The following is also recommended (see below in "Defining Stacks"):

```
c::set('plugin.rokka.stacks', [
    'noop' => 'www_noop',
    'resize' => 'www_resize',
    'raw' => 'www_raw'
]);
```

The plugin adds a `$myFile->rokkaCropUrl($width, $height, $format = "jpg")` and `
$myFile->rokkaResizeUrl($width, $height, $format = "jpg")` function to [$file objects](https://getkirby.com/docs/cheatsheet#file).


```php
// get any image/file object
$myFile = $page->file('image.jpg');

// get url (on your webserver) for optimized thumb
$url = $myFile->rokkaCropUrl(500,500);

```

There's also `$myFile->rokka($stackname, $extension)` (returning an html img tag) and
`$myFile->rokkaGetHash()` for getting the rokka hash of an image.

## Defining stacks

Rokka has a concept of [stacks](https://rokka.io/documentation/references/stacks.html), which allow to have 
nicer URLs.

You can configure some stacks with the `plugin.rokka.stacks` configure option. If you for example use certain sizes a lot, you should use a stack. For example, if you do `$myFile->rokkaCropUrl(200,200)` and `$myFile->rokkaResizeUrl(300,300)`, then define a stack with 

```
c::set('plugin.rokka.stacks', [
    `crop-200x200' => 'www_thumbnail',
    `resize-300x300' => 'www_resized',
```

The value of the array (in this example www_thumbnail) can be an ascii text, you can use there whatever you want.

The `noop`, `resize` and `raw` keys have a special meaning, especially if you want to use SVG files, you should set the `raw` key.

After you defined your stacks, go to your panel and click the "Create Rokka Stacks" links on the box on the right side, this will create your stacks on the Rokka server.

You can also set stack options for those stacks with eg.

```
c::set('plugin.rokka.stacks.options', [
    'resize-300x300' => ['webp.quality' => 80], 
    'crop-200x200' => ['jpg.quality' => 85] // small avatar images may profit from a little bit higher jpg quality
]);
```

All available Stack options can be found on the [rokka documentation](https://rokka.io/documentation/references/stacks.html).


## Retina images

To be more documented. 

Get html attribute snippets with 
`Rokka::srcAttributes($url)`
`Rokka::backgroundImageStyle($url)`
for `srcset` enabled tags with retina resolutions.

### kirbytext

This plugin overwrites the kirbytext `image` tag and serves pictures from rokka if that is used.

## Disclaimer

This plugin is provided "as is" with no guarantee. Use it at your own risk and always test it yourself before using it in a production environment. If you find any issues, please [create a new issue](https://github.com/rokka/kirby-rokka/issues/new).

## License

[MIT](https://opensource.org/licenses/MIT)
