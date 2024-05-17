---
Title: Readme - jacSelectPages 
Project: Yellow CMS - jac Extensions
Purpose: Drafting of a Project README File 
sequence: readme-15
lastupdate: 2024-04-01 12:41:30
tag: readme, functionality, media
version: 0.0.9
---
[Yellow CMS]: https://datenstrom.se/yellow/

# jacSelectPages 0.0.9

[Yellow CMS] Extension to provide a shortcode page markup that will search, filter, select, and display results on the current page.  
Useful for blog, News, or any folder with multiple files.  Includes two shortcodes:

- [ selectpages ...arguments...] [Returns a collection of pages and their final output based on filter](#filterpages).
- [ selecttags ...arguments....] [Returns a Tag Cloud suitable for sidebars](#filtertags)

[--more--]

![jacSelectPages a Yellow CMS Extension](images/screenshot-jacfilterpages.jpg)

## Description

To my knowledge, most of the filtering, searching, and display of search result happens in extensions and layouts in Yellow CMS.

By looking at a page, there are a lot of blank pages that get filled in later by the software behind it. 
I am not a programmer by trade or training; just a hobbiest who works on interesting little personal projects. 
Sometimes it takes me months or years to get back to a project and by then I don't quite remember as much as 
when I wrote it.  So I wanted a front end that made sence to me and I knew what it's purpose was and had a starting point for finding out how I screwed up this time.

These things usually come around when the hosting company decides to upgrade php versions, or the framework I was 
using doesn't keep up with new php, or they completely pivot on how a plugin/extension/addon system will work for their project.

In any case, I like seeing something on the page.  **jacSelectPage puts the query right there on the content page** that is usually blank.  It is a gateway into the search and filtering in the extensions and how it will be displayed on the page.  All stuff that I don't have a clue about 2 years later.

If you don't like this style, no worries!!!

### An example is probably better than a description.  

So here is a **page.md** for a folder or particular group of files, or for a home page.

~~~
---
Title: Issue Tracking
TitleNavigation: Issues
Project: Yellow CMS - DevDocs
Purpose: Entry Page for Issues and BUG Tracker
Author: jacmgr
Published: 2024-03-23 
Layout: default
LayoutNew: issues
---
[ selectpages 
	start=devdocs/issues  	
	xkeyword='xpublished' xkeywordvalue=* 	
	orderby='bugid' order=descend 	
	template=SummaryReports excerpts=true
	paginate=true count=15 ]
~~~

That's the whole page.md and I can tweak it at anytime in the fabulous Yellow Edit extension.  I can see from this page that 

- I am querying a specific start location; 
- I am not using keyword (x) search; 
- I am sorting by bugid with highest values first.
- The result will be displayed using a specific template (SummaryReports) as excerpts
- They will be paginated with 15 results per page.

That helps me lot 2 years from now when my site is broke for some reason!!

On the backend, the shortcut php is very similar to the blog filtering and searching. I stole it and modified it horribly.
I copied it, bastardized it, and pounded it into something that at least works for my needs. I am sure it breaks all kinds of coding standards.

>In any case, maybe someone who knows what they are doing and has a similar desire could put this kind of 
front end onto the search, or feed, or other similar extension.

I have used this for main content pages, sidebars, menus, etc.... on [most of my sites](https://www.urichip.com/sites/intro/). They are coming along great with Yellow. (You can look at the SOURCE MD on most of my pages.)

### Dependencies
- Yellow CMS Version 0.9.3: You already got that or you wouldn't be here!  
- There are no additional dependencies that need to be downloaded or installed. 
It does not interfere with Blog ot Wiki or other extensions. It also does not require those extensions to loaded.  I do not have those extensions loaded on my live sites.

### Download and Install

1. Download the [Zip] File and extract into your yellow extensions folder.
2. The structure of the files should be as shown. You will have to copy **jacfilterpages.php** to the top extensions folder.   
Move files as needed to create this folder structure.

```
   +---workers
   |   |   jacselectpages.php
   |   |      
   |   +---jac
   |   |   |---+---other jac extensions
   |   |   |       
   |   |   |---+---jacselectpages
   |   |   |       jacselectpages-cfg.php
   |   |   |       jacselectpages-ext.php
   |   |   |       jacselectpages.php  (copied up to /workers)
   |   |   |       layout-SummaryBlog.htm
   |   |   |       layout-SummaryItems.htm
   |   |   |       layout-SummaryReports.htm
   |   |   |       layout-TagList-space.htm
   |   |   |       layout-TagList.htm
   |   |   |       layout-TitleList.htm
   |   |   |       layout-TitleListLinks.htm
   |   |   |       layout-TitleListLinksWithDate.htm   
   |   |   |       
   |   |   |---+---other jac extensions
   |   |   |          
```

1. Note the file **workers/jacselectpages.php** is simply a method that allows me to choose where to put my extensions.  
I like to keep my stuff grouped together.

```php
<?php
require_once 'jac/jacselectpages/jacselectpages-ext.php';
```

## SelectPages 
### Using in your Site

- Just put the shortcode where you want searched content to appear on your page.  You can have other content before or after the search as well. 
- All parameters are NAMED. You must use "=" for all values. 
- You must use quotes if your arguments have spaces. 
- All parameters are optional. You don't have to include all of them, the extension makes reasonable default values.
- You can format as you like between the brackets
- You can omit arguments. I prefer to just change the argument name with a little "x". The extension will ignore any argument that does not match its predefined list

[ selectpages 
	start=devdocs/issues  	
	keyword='xpublished'
	keywordvalue=
	orderby=bugid
	order=descend 	
	template=SummaryReports 
	excerpts=true
	paginate=true 
	count=15 
]

`start` can be `top` | `auto` | `foldername` :  Top is entire content folder; Auto is from the folder where the page with the filtertag is located; Foldername is just what it say.   
`keyword` and `keywordvalue` :  Any of your Meta Data fields can be used. It just gets EXACT matches   
`orderby` : can be any of your Meta data.  Presumably you pick a field that all pages in the search would have. Could be a date or like the example above a text value representing the bugid.   
`order` can be `ascending` or `descending` : default is ascending   
`paginate` can be `true` or `false`   
`count` : is numeric number of result items per page.  If paginate is false, you get the first 'count' number of items.   
`template` is the pre-defined layout to format the results.  They are listed below. If your items use the [ --more-- ] tag then you can use `excerpts`.   

### Templates are Layouts

You can make your own following the examples in the folder.  It must be named starting with the prefix `layout-`. In the selecttag you list the name **without** the layout prefix.

layout-TitleList.htm
: Just a UL List of titles
:
~~~php
		echo '<ul>';
		foreach ($pages as $page):
			echo '<li>'. $page->getHtml("title") .'</li>';
		endforeach;
		echo '</ul>';
		if($textArgs['paginate'] == 'true'){
			$this->yellow->layout("pagination", $pages);
		}	
~~~
layout-TitleListLinks.htm:
: Same as above but just use a link to the page.  Nice in Sidebars or whever you want to have a list link.  Like an Archive of all pages with a high paginate count.

layout-TitleListLinksWithDate.htm:
: Same as above but includes published date.

layout-SummaryBlog.htm:
: Pretty much the yellow blog layout

layout-SummaryItems.htm:
:  Same as above. This is the one I use most frequently. It also accommodates "tag filters" with a banner telling you the filter in use. 

layout-SummaryReports.htm:
: Just another variation of above.


## SelectTags

This extension also includes a TAG Filtering system.  Uses the standard YELLOW tag on the URL syntax `/tag:tagvalue/`.  However, there are only 2 Arguments:

[ selecttags start=`top` | `auto` | `foldername` template=`template name`]


Thesea are applied the same as the **selectpages** options.

If you use `auto` or `foldername`  you will only have tags from the pages in that search group.  Usually on my toplevel home page I have all the tags; but within a folder or section I usually only show tags from that section.


If you use the **jacPageVars** extension you can also access the foldername with [ %parenttop%].  That lets the same sidebar.md shared page be customized to the currentpage.

>  [ selecttags start=[%parenttop%] template=TagList-space]

### Selecttag templates

There are 2 predefined templates for the tag cloud

layout-TagList-space.htm
: This is just a continuous group of links seperated by a space
layout-TagList.htm
: This is a typilcal UL/LI List

### Configurations

This extension does not have any configuration files or options.

### Addition Request Query 

See Sidebars

### Templates/Layouts

Note that there are lots of little partial layouts (templates) in the extension folder.  This extension uses these layouts to parse the search resuults. Many are just like the regular blog layouts.

### More complicated use.


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


## SAMPLES I've USED

Home Page
: Some Introductory text, changes periodically using Yellow's Fabulous EDIT extension.  And a Featured Filter shows 4 of them unpaginated.
~~~
---
Title: Home
TitleNavigation: Home
Published: 2024-03-12
---

Summer 2024 > Have the [Eclipse Trip](posts/eclipse-2024), The [Kentucky Derby Trip](posts/kentucky-derby-2024), a Vegas Trip, and another Korea Trip. Gonna be a busy Spring and Summer!

Summer 2022 > Gonna Try to keep the log for our Cross Country Driving Vacation. [Check it out!](xc2022).

Look over our [Blog](posts), [Stories](stories), [Reading Notes](readings), and little [Gallery](photos).  See everything in the [archives](archives).
[--more--]

##Featured Items

[ selectpages 
	start = top
	keyword='Featured' keywordvalue=true	
	orderby='published' order=descend  	
	template=SummaryItems excerpts=true
	paginate=false count=4]
~~~

SideBar Tag Cloud
: This is a shared sidebar-tags.md page.  It gets inserted in the default layout for pages with sidebar.  It showws tags based on the section it is in. 
~~~
---
Title: Tags
tag: widget
---
### Popular Tags in [%parenttop%]
<!-- 
can't use "auto" since this shared page has no idea what parent page is.
Using jacpagevars to set these vars for use in pages.
-->

[ selecttags start=[ %parenttop%] template=TagList-space]

~~~

A Little Dashboard
: 4 sections to the site.  It's fun to pick a tag on the side bar and watch the distribution of posts change.
~~~
---
Title: Dashboard
tag: widget
---
<!-- ### The Dashboard -->
<h3><i class="fa fa-star"></i> Issues</h3>
[ selectpages 
	start=devdocs/issues  
	orderby=published order=descend 	
	template=TitleListLinksWithDate excerpts=false
	paginate=false count=8 skip=0]
<h3><i class="fa fa-star"></i> Activity</h3>
[ selectpages 
	start=devdocs/daily  
	orderby=published order=descend 	
	template=TitleListLinksWithDate excerpts=false
	paginate=false count=8 skip=0]
<h3><i class="fa fa-star"></i> System</h3>
[ selectpages 
	start=devdocs/system  	
	orderby=published order=descend 	
	template=TitleListLinksWithDate excerpts=false
	paginate=false count=10 skip=0]
<h3><i class="fa fa-star"></i> Extensions</h3>
[ selectpages 
	start=devdocs/jacextensions 	
	orderby=sequence order=ascend 	
	template=TitleListLinksWithDate excerpts=false
	paginate=false count=10 skip=0]
~~~


A Section page.md file
: Uses a keyword search to list only the pages that are "daily" for the reporttype. This was a monthlong trip with about 28 daily reports.  But other posts as well.  There are other report types.  Just show the dailies on this page, show all of them without pagination.
~~~
---
Title: The 2022 Cross Country Trip
Titleslug: XC2022-ThePlan
TitleNavigation: XC2022
Published: 2022-07-06 21:00:00
Author: John
AuthorEntries: John
Layout: default
LayoutNew: item
---
# 2022 Cross Country Road Trip 
2022 July and August Hyonmi and took a cross country road trip.
Our goal was get to the pacific ocean and back in about 30 days with
a few stops along the way visiting family and friends and the country. 
We did it in 26 days.

Read about [the inital trip plan and actual outcome](XC2022-ThePlan).
Look at the [Final Trip Report](XC2022-TheFinalReport) for a link to each day, 
and see the [Google Map Trip History](XC2022-TheMaps) that google made for us.
[--more--] I've also grouped each day's [photogallery here](XC2022-TheGallery).

[ selectpages 
	start=xc2022  	
	keyword='Reporttype' keywordvalue='daily' 	
	orderby='published' order=ascend 	
	template=SummaryItems excerpts=true
	paginate=true count=28]
~~~	
	
