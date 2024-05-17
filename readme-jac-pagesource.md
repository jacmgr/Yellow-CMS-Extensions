---
Title: Readme - jacPageSource 
Project: Yellow CMS - jac Extensions
Purpose: Drafting of a Project README File 
sequence: readme-10
lastupdate: 2024-03-30 12:41:30
tag: readme, functionality
version: 0.0.9
---

[Yellow CMS]: https://github.com/datenstrom/yellow

# jacPageSource 0.0.9
[Yellow CMS] Extension to provide the markdown source of the current page IN the current page. Useful for debugging, and for copy and paste to other pages.

[--more--]
![jacPageSource a Yellow CMS Extension](screenshot-jacpagesource.png)

## Description

This is just a proof of concept type extension.  I saw other extensions for displaying source. I wanted an inline and showing not only the main content but also the MetaData.  Show the actual raw contents of the file that defines the page. 
I also used a GET Request instead of a POST. You could modify however you want.

TO DO:   
1. make a user callable function from layout that can return a source text only....Not all the formatting and extra buttons.   
2. Allow a template like file insterad of hardcodeing the form.   
3. Add config options.   

### Dependencies

There are no additional dependencies that need to be downloaded or installed.   
- yellow cms: You already got that or you wouldn't be here!

### Download and Install
1. Download the [Zip] File and extract into your yellow extensions folder.   
2. The structure of the files should be as shown. You will have to copy **jacfilterpages.php** to the top extensions folder.   
Move files as needed to create this folder structure.   

```
   +---workers
   |   |   jacpagesource.php
   |   |      
   |   +---jac
   |   |   |---+---other jac extensions
   |   |   |       
   |   |   |---+---jacpagesource
   |   |   |       jacpagesource-ext.php
   |   |   |       jacpagesource.php
   |   |   |       
   |   |   |---+---other jac extensions
   |   |   |          
```

1. Note the file **workers/jacpagesource.php** is simply a method that allows me to choose where to put my extensions.  
I like to keep my stuff grouped together.

```php
<?php
require_once 'jac/jacpagesource/jacpagesource-ext.php';
```

### Executing program

SAMPLE USE acts on a per page basis, with code placed in the page or content layouts. The extension provides the button link to close the page source.
  
1. Create a link to trigger a show source of page link/button   
	a. In Layout File   
> 		<a href="?source=true" class="btn" >Source</a>
> 		<a href="?source=false" class="btn" >Source Off</a>

	b. In Content (*.md) pages   
>		[show source](?source=true)

2. Show the Source on page using your LAYOUT php	

```php
	<?php //  ==================   PAGE SOURCE SHOW IF REQUESTED =======================
		if ($this->yellow->extension->get("jacpagesource")->checkRequest()) { 
			echo $this->yellow->extension->get("jacpagesource")->getJacpagesource($this->yellow->page);
		}
		//  ==================   PAGE SOURCE SHOW IF REQUESTED ======================= 
	?>
```

>	When the GET exists, this extension will gather the raw content file and put that nice wrapper around it and send it back to your layout.

This is what an entire layout might look like:

~~~php
<?php $this->yellow->layout("header") ?>
<div class="content">
<div class="main" role="main">
<h1><?php echo $this->yellow->page->getHtml("titleContent") ?></h1>
	<?php		
		//  ============ Put a button or some way to ask to see the source ==========
		$toggle = 'true';
		if (isset($_GET['source']) && $_GET['source'] == 'true') $toggle = 'false';
		?>
		<a href="?source=<?php echo $toggle ?>" class="btn" >Source</a>
		<?php
		//  ==================   PAGE SOURCE SHOW IF REQUESTED =======================
		if ($this->yellow->extension->get("jacpagesource")->checkRequest()) { 
			echo $this->yellow->extension->get("jacpagesource")->getJacpagesource($this->yellow->page);
		}
		//  ==================   PAGE SOURCE SHOW IF REQUESTED ======================= 
	?>
<?php echo $this->yellow->page->getContentHtml() ?>
</div>
</div>
<?php $this->yellow->layout("footer") ?>
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

Learned some from Steffen Schultz and his [existing pagesource extension](https://github.com/schulle4u/yellow-extensions-schulle4u). His approach is differnt but useful. Didn't use it here, but learned how to make a popup page too.

Thanks to [Datenstrom Yellow CMS team](https://datenstrom.se/yellow/).  It really is FUN!
