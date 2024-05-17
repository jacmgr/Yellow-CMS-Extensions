---
Title: Readme - jacMathjax 
Project: Yellow CMS - jacMathjax
Purpose: Drafting of a Project README File 
sequence: readme-100
lastupdate: 2024-04-03
tag: readme, functionality
version: 0.0.9
mathjax: true
---
[Yellow CMS]: https://datenstrom.se/yellow/
[MathJax]: https://mathjax.org/

# jacMathjax 0.0.9

[Yellow CMS] Extension for display of [MathJax] produced mathematical/scientific notion in LaTeX, MathML, or AsciiMath in your markdown page.

[--more--]
![jacMathJax a Yellow CMS Extension](images/screenshot-jacmathjax.jpg)

## Description
Mathjax library is JavaScript and works mostly over the internet. It takes your prepared markup (LaTeX/MathML/AsciiMath) and transforms it to an HTML output that you capture and display n your webpage.  This extensions simply provides access to that capability from within [Yellow].

### Dependencies

There are no additional dependencies that need to be downloaded or installed.

- mathjax: is used from its CDN : [https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML]
- yellow cms: You already got that or you wouldn't be here!

### Download and Install

1. Download the [Zip] File and extract into your yellow extensions folder.
2. The structure of the files should be

```
   +---workers
   |   |   jacmathjax.php
   |   |   |   
   |   +---jac
   |   |   |       
   |   |   +---jacmathjax
   |   |   |   |   jacmathjax-cfg.php
   |   |   |   |   jacmathjax-ext.php
   |   |   |   |   jacmathjax.php
   |   |   |   +---lib
   |   |   |          MathJax.js
   |   |   |           
```

Note the file **workers/jacmathjax.php** is simply a method that allows me to choose where to put my extensions.  I like to keep my stuff grouped together.

```php
<?php
require_once 'jac/jacmathjax/jacmathjax-ext.php';
```

You will need to copy that one file up to the main yellow extension folder, so that yellow can find the extension.

### Configurations

You should not need to change or adjust the seperate configuration file.  However, if you are an expert MathJax person you could add your additionl MathJax Plugins here.

```php
$this->config = array(
	'using_script' => 'LOCAL',
	'usingExtraHTML' => true,
    'CDN' => 'https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML',
    'LOCAL' => 'MathJax.js?config=TeX-MML-AM_CHTML',
	'template' => '<script type="text/javascript" src="%mathjax_script%"></script>',
);
```

### Using in your Site

On pages where you use LaTeX, MathML, or AsciiMath, add the Meta Field `mathjax: true`.  That's it!

> Example Page Source Markdown
~~~
---
Title: My sample page
mathjax: true
---

```
<p>
When ``$a \ne 0$`` , there are two solutions to ``\(ax^2 + bx + c = 0\)`` and they are
``$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$``
</p>
```

<p>
When `$a \ne 0$`, there are two solutions to `\(ax^2 + bx + c = 0\)` and they are
`$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$`
</p>
~~~

> Produces:

```
<p>
When ``$a \ne 0$`` , there are two solutions to ``\(ax^2 + bx + c = 0\)`` and they are
``$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$``
</p>
```

<p>
When `$a \ne 0$`, there are two solutions to `\(ax^2 + bx + c = 0\)` and they are
`$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$`
</p>


## Help

Some Help from author on github discussionsr. 
Lots of great help at [MathJax site][MathJax]. 
You can see more examples [on authors site](https://urichip.com/jh/posts/using-mathjax?source=true).

## Authors

[https://github.com/jacmgr]


## Version History

* 0.0.9 2024-04-13  Yellow CMS update 0.9.3

## License

I have no idea and need no ackknowledgement for my work.  You should however, observer the license from [MathJax](https://docs.mathjax.org/en/latest/misc/faq.html#faq-license)


This project is licensed under the [NAME HERE] License - see the LICENSE.md file for details

## Acknowledgments

Thanks to [Datenstrom Yellow CMS team](https://datenstrom.se/yellow/).  It really is FUN!
