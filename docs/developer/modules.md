# Developing and Installing Modules

Modules are what make **SyncEngine** truly extensible.  
They allow developers to add new **tasks**, **webservices**, **triggers**, or any custom functionality, all neatly packaged for installation through SyncEngine’s interface.

This guide explains how to create, structure, and distribute your own modules, and how SyncEngine automatically installs and manages them.

---

## What Is a Module?

A **module** in SyncEngine is a self-contained package that extends the system with new capabilities.  
Modules can add new tasks, modify behavior, or even integrate external APIs.  
Once installed, SyncEngine automatically detects and loads them into your workspace.

Modules are installed directly from the **Modules** page inside SyncEngine’s interface.  
When you upload a module ZIP file, SyncEngine performs the full installation automatically, including any setup or cleanup logic your module defines.

---

## Folder Structure

Every module ZIP must follow this exact structure:

```
DEVELOPERNAME/
 └── MODULENAME/
      ├── composer.json
      ├── vendor/
      └── src/
          ├── Task/
          │    └── YourTask.php
          ├── Webservice/
          ├── Event/
          └── Module.php
```

The **outer folder** should contain your developer name, and the **inner folder** your module name.  
This is required so SyncEngine can properly identify and map the module.

Example structure:

```
JohnDoe/
 └── DataTools/
      ├── composer.json
      ├── vendor/
      └── src/
          └── Task/
               └── UppercaseTask.php
```

---

## Installing Modules

Once your module is packaged correctly:

1. Open **SyncEngine → Modules** in the interface.
2. Click **Upload Module**.
3. Select your ZIP file (e.g. `JohnDoe_DataTools.zip`).
4. SyncEngine will automatically unpack, install, and autoload your module.

During installation, SyncEngine will look for an optional `install()` function inside your module’s main class (usually `Module.php`).  
Likewise, when you uninstall the module, SyncEngine will trigger the `uninstall()` function if present, perfect for cleanup operations or database adjustments.

> **Note:** Always include a pre-built `vendor/` folder in your ZIP so that users do **not** need Composer installed on their servers.

---

## Example Module: “Uppercase Task”

Below is an example of a simple custom module that adds a new task: `UppercaseTask`.  
This task converts all text fields in your dataset to uppercase.

### Folder Layout

```
JohnDoe/
 └── DataTools/
      ├── composer.json
      ├── vendor/
      └── src/
          ├── Task/
          │    └── UppercaseTask.php
          └── Datatools.php
```

### composer.json

```json
{
  "name": "johndoe/datatools",
  "description": "Adds custom text processing tasks to SyncEngine",
  "type": "syncengine-module",
  "autoload": {
    "psr-4": {
      "JohnDoe\\DataTools\\": "src/"
    }
  }
}
```

### src/Datatools.php

```php
<?php

namespace SyncEngine\Module\JohnDoe\DataTools;

use SyncEngine\Model\ModuleModel;

class Datatools extends ModuleModel
{
    const VERSION = '1.0';
    
    public function __construct()
    {
        parent::__construct();
        $this->name        = 'Uppercase Task';
        $this->description = 'Converts all string values in the dataset to uppercase.';
    }
	
    public function install(): void
    {
        // Optional: Actions to perform during installation
        // e.g. create default storage, register resources, etc.
    }

    public function uninstall(): void
    {
        // Optional: Cleanup actions during uninstall
    }
}
```


### src/Task/UppercaseTask.php

```php
<?php

namespace SyncEngine\Module\JohnDoe\DataTools\Task;

use SyncEngine\Model\TaskModel;
use SyncEngine\Structure\Data\ConfigData;
use SyncEngine\Service\ExecuteContext;
use SyncEngine\Service\ExecuteData;
use SyncEngine\Exception\ExecuteException;

class UppercaseTask implements TaskModel
{    
    public function execute(ConfigData $config, ExecuteContext $context, ExecuteData $data): ExecuteData
    {
        $rows = $data->all();
        $targetKey = $config['target_key'] ?? null;

        try {
            foreach ($rows as &$row) {
                foreach ($row as $key => &$value) {
                    if (is_string($value)) {
                        $value = mb_strtoupper($value);
                    }
                }
            }

            $data->set($rows, $targetKey);
        } catch ( ExecuteException $error ) {
            $context->addError( $error );
        } catch ( \Throwable $error ) {
			if ( ! empty( $error::$SYNCENGINE_EXITPREVIEW ) ) {
				throw $error;
			}
			$context->addError( $error );
		}
        return $data;
    }
}
```

Once installed, this task will automatically appear in the task list under the name **Uppercase**, ready to use in any automation.

---

## Best Practices for Module Development

- **Always include the `composer.json` file.**  
  This ensures SyncEngine can correctly map your classes using PSR-4 autoloading.

- **Bundle the `vendor` folder** before zipping the module.  
  This allows installation on systems that do not have Composer installed.

- **Use namespaces** that match your folder structure (e.g., `JohnDoe\DataTools`).

- **Keep modules self-contained.**  
  Avoid hard dependencies on other modules unless absolutely necessary.

- **Optionally implement `install()` and `uninstall()` hooks** for setup and cleanup logic.

- **Test locally** before distribution, once zipped and uploaded, SyncEngine takes care of the rest.

---

## Summary

| Step | Description                                               |
|------|-----------------------------------------------------------|
| **1. Develop your module** | Create tasks, webservices, or triggers under `/src/`.     |
| **2. Add composer.json** | Define autoloading and metadata.                          |
| **3. Include vendor folder** | Prebuild dependencies via Composer.                       |
| **4. Zip your module** | Name and structure it as `DeveloperName/ModuleName`.      |
| **5. Upload to SyncEngine** | Go to *Modules → Upload Module* to install automatically. |

> **Tip:** Distribute your modules as ZIPs, users can install or remove them easily without using Composer or command-line tools.

---

By following this structure, your custom modules will integrate seamlessly with SyncEngine, instantly expanding its capabilities with your own unique tools and logic.
