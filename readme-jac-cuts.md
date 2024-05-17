---
Title: Readme - jacCuts 
Project: Yellow CMS - jac Extensions
Purpose: Drafting of a Project README File 
sequence: readme-50
lastupdate: 2024-03-25 12:41:30
tag: readme, functionality
version: 0.0.9
---
[WPformatguide]: https://wordpress.org/documentation/article/customize-date-and-time-format/
[Yellow CMS]: https://github.com/datenstrom/yellow

# jacCuts 0.0.9

[Yellow CMS] Extension to provide miscellaneous minor functionality. jacCuts = jac + ShortCuts

[--more--]

![jacCuts a Yellow CMS Extension](images/screenshot-jaccuts.jpg)

## Description

jacCuts is a miscellaneous collection of yellow cms shortcuts for custom page content.   
These are short uncomplicated snippets that don't warrant a whole extension on their own.  Currently includes:

- [datetimefmt](#datetimefmt) : Pass it a date on your MD page and it will format it and spit it back to your MD page. Also includes a function to use in layouts.
- more to come   
- more to come  

### Dependencies
There are no additional dependencies that need to be downloaded or installed.   
- Yellow CMS Version 0.9.3: You already got that or you wouldn't be here!  

### Download and Install

1. Download the [Zip] File and extract into your yellow extensions folder.
2. The structure of the files should be as shown. You will have to copy **jaccuts.php** to the top extensions folder.   
Move files as needed to create this folder structure.

```
   +---workers
   |   |   jaccuts.php
   |   |      
   |   +---jac
   |   |   |---+---other jac extensions
   |   |   |       
   |   |   |---+---jaccuts
   |   |   |          jaccuts-cfg.php
   |   |   |          jaccuts-ext.php
   |   |   |          jaccuts.php
   |   |   |       
   |   |   |---+---other jac extensions
   
```

Note the file **workers/jaccuts.php** is simply a method that allows me to choose where to put my extensions.  
I like to keep my stuff grouped together.

```php
<?php
require_once 'jac/jaccuts/jaccuts-ext.php';
```



## datetimefmt

Place the shortcode where you have a date in your content. The format is:

[ datetimefmt `date` `fmt` `relative` `days`]

The following arguments are available, all are optional.  
If no options are included, returns the current date and time according to format in configuration file.

`date`  - a textual date in any format. If there are spaces include in double quotes. The default format can be set in the config file.   
`fmt`   - optional,  If spaces in the format, include in double quotes  
`relative` can be blank or the word "relative" (no quotes needed!)   
`days` - is an integer number of days. should be blank if relative is blank (i.e. not used). Default can be set in config.   


#### Formats

Any php date() format is acceptable. Common ones are shown in the examples below.

#### Relative Dates

This is a [Yellow CMS][Yellow] feature.  `relative` will show the number of days/weeks/years relative to todays date. It will only show this relative within the `days` that you set. After that it reverts to a formatted date.

### datetimefmt Examples

> The party was scheduled for [ datetimefmt 2024-03-15], but it had to be cancelled and re-scheduled for [ datetimefmt "June 15 2024 15:30:00" "l, F jS, Y g:i a"].

Produces:

> The party was scheduled for 03-15-2024, but it had to be cancelled and re-scheduled for Saturday, June 15th, 2024 3:30 pm.

!Well, I probably could have typed the date in the format I wanted it faster than looking up those codes! That's true.   
I primarily use `datetimefmt` for dates that **MAY BE VARIABLE**.  The variables usually come from the extension "**jacPageVars**". If you use that extension, you can have this in your page.

> This document was published on [ datetimefmt "[%produced%]"] and last updated on [ datetimefmt "[%lastupdate%]" "F j, Y g:i a"] ([ datetimefmt "[%lastupdate%]" "" relative 365]).

Produces:

> his document was published on 10-12-2022 and last updated on March 25, 2024 12:41 pm (11 days ago).

! More useful in that scenario!!

> Finally, Just want to run the current/date and time.  just use [ datetimefmt]

Produces

>[datetimefmt] according to format in configuration file.

### Formats to Try


|     Short Cut Format              |         Page Output  |
| --------------------------------- | -------------------- |
| [ datetimefmt "06JUN2023 15:35:12"  "m/d/Y"]     |  06/06/2023   |   
| [ datetimefmt "06JUN2023 15:35:12"  "F j, Y"]     |  June 6, 2023  |   
| [ datetimefmt "06JUN2023 15:35:12"  "F Y"]     |   June 2023	 |   
| [ datetimefmt "06JUN2023 15:35:12"  "d F, Y"]     |  06 June, 2023   |   
| [ datetimefmt "06JUN2023 15:35:12"  "d F, Y (l)"]     |  06 June, 2023 (Tuesday)    |   
| [ datetimefmt "06JUN2023 15:35:12"  "h:i:s A"]     |  03:35:12 PM	   |   
| [ datetimefmt "06JUN2023 15:35:12"  "l, F j, Y"]     |  Tuesday, June 6, 2023	  |   
| [ datetimefmt "06JUN2023 15:35:12"  "l, F jS, Y"]     |   Tuesday, June 6th, 2023	  |   
| [ datetimefmt "06JUN2023 15:35:12"  "F j, Y g:i a"]     |  June 6, 2023 3:35 pm   |   
| [ datetimefmt "06JUN2023 15:35:12"  "l, h:i:s A"]     |  Tuesday, 03:35:12 PM   |   
| [ datetimefmt "06JUN2023 15:35:12"  "d F Y, h:i:s A"]     | 06 June 2023, 03:35:12 PM	 |   
| [ datetimefmt "06JUN2023 15:35:12"  "M j, Y @ G:i"]     | Jun 6, 2023 @ 15:35	    |   
| [ datetimefmt "06JUN2023 15:35:12"  "g:i A"]     |  3:35 PM	   |   
| [ datetimefmt "06JUN2023 15:35:12"  "g:i:s a"]     |  3:35:12 pm   |   


### Configurations

Sets the defaults for when you don't enter a parameter.

~~~
$this->config = array(
	'jaccutsdefaultdate' => date("l, F jS, Y g:i a"),
	'jaccutsdefaultdatefmt' => "m-d-Y",
	'jaccutsdefaultdaterelative' => false,
    'jaccutsdefaultdaterelativedays' => 365,
);
~~~

### datetimefmt In Layouts

~~~
<?php echo $this->yellow->extension->get("jaccuts")->getdatetimefmt('2023-06-15', "l r f d", "relative", 365); ?>
~~~


## Help

Some Help from author on github discussions. 

## Authors

[https://github.com/jacmgr]


## Version History

* 0.0.X 2024-04-05

## License

I have no idea about licenses and need no ackknowledgement for my work. Use it if it makes you have some fun.

This project is licensed under the [NAME HERE] License - see the LICENSE.md file for details

## Acknowledgments

Thanks to [Datenstrom Yellow CMS team](https://datenstrom.se/yellow/).  It really is FUN!
