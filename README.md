Connect to Jira via Curl

### Usage

#### Composer
First add the follow to your `composer.json` file:
```json
"require": {
  "srag/removeplugindataconfirm": ">=0.1.0"
},
```

And run a `composer install`.

If you deliver your plugin, the plugin has it's own copy of this library and the user doesn't need to install the library.

Hint: Because of multiple autoloaders of plugins, it could be, that different versions of this library exists and suddenly your plugin use an old version of an other plugin! So you should keep up to date your plugin with `composer update`.

#### Use
First declare your plugin class like follow:
```php
//...
use srag\RemovePluginDataConfirm\PluginUninstallTrait;
//...
use PluginUninstallTrait;
//...
const PLUGIN_CLASS_NAME = self::class;
const REMOVE_PLUGIN_DATA_CONFIRM_CLASS_NAME = XRemoveDataConfirm::class;
//...
/**
 * @inheritdoc
 */
protected function deleteData() {
	// TODO: Delete your plugin data in this method
}
//...
```
`XRemoveDataConfirm` is the name of your remove data confirm class.

If your plugin is a RepositoryObject use `RepositoryObjectPluginUninstallTrait` instead:
```php
//...
use srag\RemovePluginDataConfirm\RepositoryObjectPluginUninstallTrait;
//...
use RepositoryObjectPluginUninstallTrait;
//...
```

Then create a class called `XRemoveDataConfirm` in `classes/uninstall/class.XRemoveDataConfirm.php`:
```php
<?php

require_once __DIR__ . "/../../vendor/autoload.php";

use srag\Plugins\X\Config\XConfig;
use srag\RemovePluginDataConfirm\AbstractRemovePluginDataConfirm;

/**
 * Class XRemoveDataConfirm
 *
 * @ilCtrl_isCalledBy XRemoveDataConfirm: ilUIPluginRouterGUI
 */
class XRemoveDataConfirm extends AbstractRemovePluginDataConfirm {

	const PLUGIN_CLASS_NAME = ilXPlugin::class;


	/**
	 * @inheritdoc
	 */
	public function removeUninstallRemovesData() {
		XConfig::removeUninstallRemovesData();
	}


	/**
	 * @inheritdoc
	 */
	public function getUninstallRemovesData() {
		return XConfig::getUninstallRemovesData();
	}


	/**
	 * @inheritdoc
	 */
	public function setUninstallRemovesData($uninstall_removes_data) {
		XConfig::setUninstallRemovesData($uninstall_removes_data);
	}
}

```
`ilXPlugin` is the name of your plugin class.
Replace the `X` in `XRemoveDataConfirm` with your plugin name.
If you do not use `ActiveRecordConfig` replace in the `UninstallRemovesData` methods with your own database functions

If you use `ActiveRecordConfig` add the follow to these class:
```php
///...
use XRemoveDataConfirm;
///...
/**
 * @return bool|null
 */
public static function getUninstallRemovesData() {
	return self::getXValue(XRemoveDataConfirm::KEY_UNINSTALL_REMOVES_DATA, XRemoveDataConfirm::DEFAULT_UNINSTALL_REMOVES_DATA);
}


/**
 * @param bool $uninstall_removes_data
 */
public static function setUninstallRemovesData($uninstall_removes_data) {
	self::setBooleanValue(XRemoveDataConfirm::KEY_UNINSTALL_REMOVES_DATA, $uninstall_removes_data);
}


/**
 *
 */
public static function removeUninstallRemovesData() {
	self::deleteName(XRemoveDataConfirm::KEY_UNINSTALL_REMOVES_DATA);
}
//...
```

Then you need to declare some language variables like:
English:
```
uninstall_cancel#:#Cancel
uninstall_confirm_remove_data#:#Do you want to remove the %s data as well? At most, you just want to disable the %s plugin?
uninstall_deactivate#:#Just deactivate %s plugin
uninstall_data#:#%s data
uninstall_keep_data#:#Keep %s data
uninstall_msg_kept_data#:#The %s data was kept!
uninstall_msg_removed_data#:#The %s data was also removed!
uninstall_remove_data#:#Remove %s data
```
German:
```
uninstall_cancel#:#Abbrechen
uninstall_confirm_remove_data#:#Möchten Sie die %s-Daten auch entfernen? Allenfalls möchten Sie das %s-Plugin nur deaktivieren?
uninstall_deactivate#:#%s-Plugin nur deaktivieren
uninstall_data#:#%s-Daten
uninstall_keep_data#:#%s-Daten behalten
uninstall_msg_kept_data#:#Die %s-Daten wurden behalten!
uninstall_msg_removed_data#:#Die %s-Daten wurden auch entfernt!
uninstall_remove_data#:#Entferne %s-Daten
```
If you want you can modify these. The `%s` placeholder is the name of your plugin.

### Adjustment suggestions
* Adjustment suggestions by pull requests on https://git.studer-raimann.ch/ILIAS/Plugins/RemovePluginDataConfirm/tree/develop
* Adjustment suggestions which are not yet worked out in detail by Jira tasks under https://jira.studer-raimann.ch/projects/LJIRACURL
* Bug reports under https://jira.studer-raimann.ch/projects/LJIRACURL
* For external developers please send an email to support-custom1@studer-raimann.ch

### Development
If you want development in this library you should install this library like follow:

Start at your ILIAS root directory 
```bash
mkdir -p Customizing/global/plugins/Libraries/  
cd Customizing/global/plugins/Libraries/  
git clone git@git.studer-raimann.ch:ILIAS/Plugins/RemovePluginDataConfirm.git RemovePluginDataConfirm
```
