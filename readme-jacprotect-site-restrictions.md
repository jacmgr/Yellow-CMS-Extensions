---
Title: Readme - jacProtect Site Restrictions
Project: Yellow CMS - jac Extensions
Purpose: Draft a README FILE.
sequence: readme-20
lastupdate: 2024-05-06 12:41:30
tag: readme, system
version: 0.0.9
---
# Restrict User Access to Pages

Trying to figure out the best way to restrict access to a site to just myself.  I only have 1 user through the [yellow edit extension](https://github.com/annaesvensson/yellow-edit).  I looked at the [restrict extension](url) and the [draft](https://github.com/annaesvensson/yellow-draft) and the [private extension(https://github.com/schulle4u/yellow-private). Each has some pros and cons, but they don't do what I intended.  For now I settled on a couple solutions.

> UNFINISHED README..... You can see it in action at Author's example Site Make sure you read to the bottom of the page to see how to login and try it out.

[--more--]

## Description

I am the only user and editor of my sites. I have several personal sites. They are mostly out there on the internet, but really, I'm the only one visiting them.   There are 1 or 2 sites that I want to restrict all access to the contents.  Don't want to hide them,  just not let folks see the content. Nothing top secret, and if I fail, I don't care if someone actually does see them.   The other solutions mentioned involve creating a type of user account and making some page settings and creating an extension.  I figured I already have a user account through [yellow edit extension](https://github.com/annaesvensson/yellow-edit). Why not just use that; so I did.

Basically in the default layouts and any alternate layout I check if I am logged in and if I am I show the main content.  The site and menus and footers all still show up.  Most of my sites only have 2 layouts, so it is fairly easy.

## Getting Started

* Utilize this snippet in your Layout

```php
<?php
//if not logged in don't show content.
if ($this->yellow->user->isUser("name")): ?> 

Whatever content should be restricted goes here..............

<?php endif; ?>
```

* For example, Edit default.html (or whatever relevant template) to utilize this snippet

```php
<?php $this->yellow->layout("header") ?>
<div class="content">
<div class="main" role="main">

<?php if ($this->yellow->user->isUser("name")): ?> 

<h1><?php echo $this->yellow->page->getHtml("titleContent") ?></h1>
<?php echo $this->yellow->page->getContentHtml() ?>

<?php endif; ?>

</div>
</div>
<?php $this->yellow->layout("footer") ?>

```


### Dependencies

* Yellow edit extension.

## Help

Some Help from author on github discussions. 

## Authors

[https://github.com/jacmgr]


## Acknowledgments

This idea is discussed in several different threads on the [Datenstrom Yellow Discussion Board](https://github.com/datenstrom/yellow/discussions)

Thanks to [Datenstrom Yellow CMS team](https://datenstrom.se/yellow/).  It really is FUN!
