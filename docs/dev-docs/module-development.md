# Building Modules
Creating a module for the open-source system **SyncEngine** involves structuring your code to align with **SyncEngine's** conventions. This guide outlines the core components of your module and their respective functionalities.
___

## Module Structure Overview

The directory structure of your module should follow this structure:
```
AuthorName/ModuleName/
├── register.php
├── src/
│ ├── ModuleName.php
│ ├── Controllers/
│ ├── Tasks/
│ ├── Webservice/
│ ├── Blueprints/
│ └── ...
├── templates/
├── vendor/
```

### **register.php**

This file is responsible for autoloading vendor libraries. Typically, it contains the following line:

```php
include __DIR__ . '/vendor/autoload.php';
```
This ensures that any external dependencies used by your module are properly loaded.
> Tip: Generate your autoloader using [Composer](https://getcomposer.org)

### **ModuleName.php (Main Module File)**
The ModuleName.php file is the entry point of your module. It defines critical information about the module and contains functions that interact with SyncEngine's lifecycle and actions.

#### Core Properties
```php
public string $name = 'ModuleName';
public string $description = 'Describe your module in one sentence';
const AUTHOR  = 'AuthorName';
const VERSION = '1.0';
```

- $name: The module's name that is displayed. *
- $description: A brief description of your module for the module overview page. *
- AUTHOR: The creator's/company name of the developer.
- VERSION: The version number of your module using [Semantic Versioning 2.0.0](https://semver.org/).

####  Lifecycle and Action Functions
Define the following functions to handle module-specific tasks during its lifecycle:

- install: Called when the module is installed. Use this to set up necessary configurations or database schemas.
- uninstall: Called when the module is uninstalled. Use this to clean up resources.
- renderRequest: Used to handle and render specific config or other required info.

### **src/ Directory**
This folder organizes your module's backend logic. It typically includes subdirectories for various functionalities:

- Controllers/: Houses controller classes that mediate between views and models.
- Tasks/: Contains your custom build Tasks.
- Webservice/: Contains your webservice(s) that let you connect your system with specific config.
- Blueprints/: Includes blueprints files to standardize configurations or other structured data.

### **templates/ Directory**
In this folder you can keep all your specific templates for any config pages or other non **SyncEngine** views.
Currently supported templating engine is [Twig](https://twig.symfony.com/doc/3.x/templates.html)


# Creating Custom Tasks

SyncEngine provides developers with a robust platform for extending functionality by creating custom tasks. This is made simple through the use of the `TaskModel` class. By extending this base class, developers can define their own task logic and integrate seamlessly into the SyncEngine system.

---

## Getting Started with Custom Tasks

To create a custom task in SyncEngine, you need to create a new class that extends the `TaskModel`. This ensures your task inherits essential functionality while allowing you to define task-specific behavior.

### The Basic Structure

Here’s a template for creating a custom task:

```php
<?php

namespace SyncEngine\Module\AUTHOR\MODULENAME\Task;

use SyncEngine\Model\TaskModel;
use SyncEngine\Service\ExecuteContext;
use SyncEngine\Service\ExecuteData;
use SyncEngine\Task\Type\ModifierTaskType;

class CalculateTask extends TaskModel {
    public function __construct() {
        parent::__construct();

        $this->type = ModifierTaskType::TYPE;
        $this->name = 'Calculate';
        $this->description = 'Add and subtract';
    }

    function execute( array $config, ExecuteContext $context, ExecuteData $data ): ExecuteData
	{
		return $data;
	}
}
```