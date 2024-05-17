---
Title: Readme - jacImageList 
Project: Yellow CMS - jac Extensions
Purpose: Drafting of a Project README File 
sequence: readme-40
lastupdate: 2024-05-12
tag: readme, functionality, media
version: 0.0.9
---
[Yellow CMS]: https://github.com/datenstrom/yellow
[Yellow CMS Image Extension]: https://github.com/annaesvensson/yellow-image
[jacImage]: readme-jacimage

# jacImage 0.0.9
[Yellow CMS] Extension to provide access to a image selector based on the media image folder. Triggered by link generated in the [jacImage] extension
function of identifying image that does not exist and linking to the functions in this extension.

[--more--]
![jacImageList a Yellow CMS Extension](screenshot-jacimage.jpg)

## Description jacImageList

>  UNFINISHED README.....  Add a screenshot and description.........


###  jacIMAGE

Works exactly the same as [ image] shortcut provided in the [Yellow CMS Image Extension].  It is actually a direct copy of that script with about 6 lines added to it.

I made 3 changes, they are numbered in comments in the php file
1. Use a config file to enter name of the desired "noimage" placeholder image. That image must exist under media/images. 
2. Changed extension and shortcut name to jacimage. (duh!!)
3. Inserted about 6 lines of code to swap the missing image name to the no image placeholder.

I am also fully convinced that the real programmers have a better and simpler way to do it; but suits my purpose for now.

The intention for now is standard image extension stays installed, and I use my jacimage alongside of it for now.   
Currently I also use the Yellow Gallery Extension a lot.   
Gallery requires the Image extension....so don't want to touch the mess with it (for now...).  

> I am fully aware that you can just use the Yellow Image shortcut and enter the name of your own existing placeholder image! This extension is just laying my groundwork for Future additional features.

## Future Todo

-1. If author is logged in, then make the image clickable link to popup for uploading an image/media and automatically use the `name` and `alt` information provided.

-2.  If author is logged in, provide an option in #1 above to View the exising media images in a gallery type popup, and pick it, rename it if they want to (i.e. make copy) 
I have no idea how to do that....yet!!

## Getting Started
Simply use a shortcut `[ jacimage ...arguments ]` on your page.  It takes identical Arguments and Defaults as the [Yellow CMS Image Extension].

The following arguments are available, all but the first argument are optional:

`Name` = file name
`Alt` = description of the image, wrap multiple words into quotes
`Style` = image style, e.g. left, center, right
`Width` = image width, pixel or percent
`Height` = image height, pixel or percent

> For jacImage the first argument IS OPTIONAL.  i.e using [ jacimage] as is will simply display the placeholder image.

You can configure the name of the placeholder file in the configuration file.

### Dependencies
- Yellow CMS Version 0.9.3: You already got that or you wouldn't be here!  
- There are no additional dependencies that need to be downloaded or installed.   No impact or interaction with the standard image shortcut. (Don't forget the Gallery Extension requires standard Image Extensionto be indtalled)


### Installing

1. Download the [Zip] File and extract into your yellow extensions folder.
2. The structure of the files should be as shown. You will have to copy **jacimage.php** to the top extensions folder.   
Move files as needed to create this folder structure.

```
   +---workers 
   |   |   jacimage.php
   |   |      
   |   +---jac
   |   |   |---+---other jac extensions
   |   |   |       
   |   |   |---+---jacimage
   |   |   |          jacimage-cfg.php
   |   |   |          jacpaimage-ext.php
   |   |   |          jacimage.php
   |   |   |       
   |   |   |---+---other jac extensions
   
```

Note the file **workers/jacimage.php** is simply a method that allows me to choose where to put my extensions.  
I like to keep my stuff grouped together.


## Help

Some Help from author on github discussions. 

## Authors

jacmgr@yahoo.com


## Version History

* 0.0.X 2024-04-05

## License

I have no idea about licenses and need no ackknowledgement for my work. Use it if it makes you have some fun.

This project is licensed under the [NAME HERE] License - see the LICENSE.md file for details

## Acknowledgments

Thanks to [Datenstrom Yellow CMS team](https://datenstrom.se/yellow/).  It really is FUN!

### Appendix - Code Changes

This is what I modified in image.php


~~~~php
.
.
.
     public function onLoad($yellow) {
        $this->yellow = $yellow;
		// =========================== 
		//1. jacmgr config for $this->config['replacementimage'] placeholder and future create links image
		include 'jacimage-cfg.php';  
		// END ========================== 
        $this->yellow->system->setDefault("imageUploadWidthMax", "1280");
.
.
.
.
    // Handle page content of shortcut
    public function onParseContentShortcut($page, $name, $text, $type) {
        $output = null;
		// 2. jacmgr  mod below, orig:  
		// if ($name=="image" && $type=="inline") {
        if ($name=="jacimage" && $type=="inline") {
            list($name, $alt, $style, $width, $height) = $this->yellow->toolbox->getTextArguments($text);
            if (!preg_match("/^\w+:/", $name)) {
                if (is_string_empty($alt)) $alt = $this->yellow->language->getText("imageDefaultAlt");
                if (is_string_empty($width)) $width = "100%";
                if (is_string_empty($height)) $height = $width;
                $path = $this->yellow->lookup->findMediaDirectory("coreImageLocation");
                list($src, $width, $height) = $this->getImageInformation($path. $name, $width, $height);
				// ======================================================================================
				// 2. jacmgr, this is an insertion, not modified lines....
				// discovered yellow does its thing and returns the urls and filenames it want;
				// but has not yet (or ever???) checked if the file exists
				//I'll check here if it exists and substitute the existing no image image
				//look carefully: this is basically verbatim of the previous 7 lines!! Hoo Hoo...easy... 
				if (!file_exists($src)) {
					list($name, $alt, $style, $width, $height) = $this->yellow->toolbox->getTextArguments($text);
					$name = $this->config['replacementimage']; // SUBSTITUTE IMAGE NAME
				            if (!preg_match("/^\w+:/", $name)) {
				                if (is_string_empty($alt)) $alt = $this->yellow->language->getText("imageDefaultAlt");
				                if (is_string_empty($width)) $width = "100%";
				                if (is_string_empty($height)) $height = $width;
				                $path = $this->yellow->lookup->findMediaDirectory("coreImageLocation");
				                list($src, $width, $height) = $this->getImageInformation($path. $name, $width, $height);
            				}
				}
				// jacmgr end of line insertioins
				// NO ADDITIONAL CHANGES BELOW.......
				// END =============================================================================================
            } else {
.
.
.

~~~~

## WHY ?
No problem, use it or not! Get inspired by it or not!

Most of my files are in galleries so not called by image shortcut. 
Picking out images slows me down. I want to just write, and clean up later.
The current image shortcut DOES ALL OF THAT; except displays brocken image icons on parsed page.
I just prefer to have a standard image fill in to give me feel for my text....

So I can put image placeholders where I want them in my writing. I can prename them here as I like in the tag.
Future: when logged in, click on image and upload the image without going into editor.

[ jacimage 2024-03-12-in-theorchard.jpg "At Sunnyvale Orchards in the Spring" right 50%]
[ jacimage 2024-03-12-sunrise.jpg "Looking East from our Hotel Room right" 64 64]

For now (not in future) Don't need jacimage for the same display effect if you have the nophoto.jpg in your media.
[ image nophoto.jpg Example right 320 200] same result [ jacimage nophoto.jpg Example right 320 200]

Then Someday I'll have a link on the image that will Upload the file, use the name here for uploaded file.

