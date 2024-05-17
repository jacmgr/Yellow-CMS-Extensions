---
Title: Readme - jacMultiSite on One Installation
Project: Yellow CMS - HACK
Purpose: Draft a README FILE.
sequence: readme-30
lastupdate: 2024-04-21 12:41:30
tag: readme, system
version: 0.0.9
---

# How I did Multi Sites on One Installation

This is a CORE HACK. It is NOT AN EXTENSION.  However, it is rather simple.

[--more--]

## Description

An in-depth paragraph about your project and overview of use.

## Getting Started


## System Configuration File

File: jac-yellow-system-cfg.php   

Located adjacent to your yellow.php file.  Specifies the location of the yellow system folder. 

```

	$JACyellowsystemLocation = 'yellow/';
        $this->system->set("coreSystemDirectory", $JACyellowsystemLocation."system/");
        $this->system->set("coreCacheDirectory", $JACyellowsystemLocation."system/cache/");
        $this->system->set("coreExtensionDirectory", $JACyellowsystemLocation."system/extensions/");
        $this->system->set("coreLayoutDirectory", $JACyellowsystemLocation."system/layouts/");
        $this->system->set("coreThemeDirectory", $JACyellowsystemLocation."system/themes/");
        $this->system->set("coreTrashDirectory", $JACyellowsystemLocation."system/trash/");
        $this->system->set("coreWorkerDirectory", $JACyellowsystemLocation."system/workers/");
        list($pathInstall, $pathRoot, $pathHome) = $this->lookup->findFileSystemInformation();
        $this->system->set("coreServerInstallDirectory", $pathInstall);
        $this->system->set("coreServerRootDirectory", $pathRoot);
        $this->system->set("coreServerHomeDirectory", $pathHome);

```

### Core.php Hack

approx line 100 find these lines........

```
       if ($this->system->get("coreDebugMode")>=1) {
            ini_set("display_errors", 1);
            error_reporting(E_ALL);
        }
        date_default_timezone_set($this->system->get("coreTimezone"));
```

And simply insert the lines below before the datew_default_timezone line.....

```
       if ($this->system->get("coreDebugMode")>=1) {
            ini_set("display_errors", 1);
            error_reporting(E_ALL);
        }
// ---------------------------------------  jacmgr
// insert these lines below
// jacmgr jachack April2024
	    if (file_exists('jac-yellow-system-cfg.php')){
	    	include('jac-yellow-system-cfg.php');
		} 
// ---------------------------------------  jacmgr
// continue with original core.php
        date_default_timezone_set($this->system->get("coreTimezone"));

```

### Final Step: Edit yellow.php

```
// Datenstrom Yellow, https://github.com/datenstrom/yellow
// ---------------------------------------------------------------------
    //jacmgr: jachack to use one system folder for multiple sites
if (file_exists('jac-yellow-system-cfg.php')){
    // must agree with what is in 'jac-yellow-system-cfg.php'
$JACyellowsystemLocation = 'yellow/';
//can't chage this unless you also add to $JACyellowsysteLocation  
$JACyellowSystemFolderName = 'system';  
require($JACyellowsystemLocation."$JACyellowSystemFolderName/workers/core.php");
} else {
	// standard yellow system install with no jachac
	require("system/workers/core.php");	 	
}
// ---------------------------------------------------------------------
// continues below normally....
if (PHP_SAPI!="cli") {
    $yellow = new YellowCore();
    $yellow->load();
    $yellow->request();
} else {
    $yellow = new YellowCore();
    $yellow->load();
    exit($yellow->command());
}
```

### Overall Multi Site Structure.

```
+--topfolder/root (i.e. www.example.com)
  | yellow.php
  | jac-yellow-system-cfg.php [ specifies system is at $JACyellowsystemLocation = 'yellow/'; ]
  | jacsite-cfg.php  [ specifies content is at /sites/example/content/ ]
  | .htaccess
  | robot.txt  
  | 
  |-+-media  [ media for main top level site ]
  |   |-+--downloads
  |   |-+--images
  |   |-+--thumbnails
  |
  |-+-yellow
  |   |-+--| system   (current version in use)
  |   |    |-+--extensions 
  |   |    |-+--layouts
  |   |    |-+--themes
  |   |    |    | example.css
  |   |    |    | example.png     
  |   |    |    | site1.css
  |   |    |    | site1.png
  |   |    |    | site2.css
  |   |    |    | site2.png
  |   |    |    | site3.css
  |   |    |    | site3.png     
  |   |    |    |-+--jaccopenhagen
  |   |    |    |    |jaccopenhagen-original.css  
  |   |	   |    |    |jaccopenhagen-orange.css   
  |   |    |    |    |jaccopenhagen-blue.css  
  |   |    |    |-+--fontawesome
  |   |    |
  |   |    |-+--workers 
  |   |       | each yellow extension.php
  |   |       | each jac extension.php that include from subfolder jac  
  |   |       |-+--jac
  |   |           |-+--jacextension1     
  |   |           |-+--jacextension2 
  |   |           |-+--jacextension3 
  |   |           |-+--jacextension4
  |   |
  |   |-+--| system-08   (prior version accessible by modifying site system config)
  |   |    |-+--extensions 
  |   |    |-+--layouts
  |   |    |-+--themes
  |   |    |-+--workers 
  |   |  
  |   |-+--| system-myhack   (my hacked version accessible by modifying site system config)      
  |        |-+--extensions 
  |        |-+--layouts
  |        |-+--themes
  |        |-+--workers 
  |     
  |-+-sites
       |  
       |-+-example  [This is only CONTENT folder for TOP Level site www.example.com ]
       |    | .htaccess
       |    | robot.txt  
       |    |-+--content 
       |-+-site2    
       |    | yellow.php
       |    | jac-yellow-system-cfg.php [ specifies system is at $JACyellowsystemLocation = '../../yellow/'; ]
       |    | jacsite-cfg.php
       |    | .htaccess
       |    | robot.txt  
       |    |-+--content
       |    |-+--media
       |-+-site3  
       |    | yellow.php
       |    | jac-yellow-system-cfg.php [ specifies system is at $JACyellowsystemLocation = '../../yellow/'; ]
       |    | jacsite-cfg.php
       |    | .htaccess
       |    | robot.txt  
       |    |-+--content
       |    |-+--media
       |-+-site4  
       |    | yellow.php
       |    | jac-yellow-system-cfg.php [ specifies system is at $JACyellowsystemLocation = '../../yellow/'; ]
       |    | jacsite-cfg.php
       |    | .htaccess
       |    | robot.txt  
       |    |-+--content
       |    |-+--media
```

