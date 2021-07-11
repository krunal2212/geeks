
### Upgrade Magento2 Community to Cloud-Solved

Magento is one of the best Platforms for Ecommerce solutions.

To keep it short, To convert to Commerce before that please check your server or system is satisfying the requirement as described in System Check.

Once you checked everything and all is okay in the system, Below are the commands and steps that will help you to upgrade from Community to Cloud.

**NOTE:** Cloud & Enterprise versions are same, But the difference is Cloud is deployed to Adobe Magento server, and Enterprise is deployed to our customized server.

All commands are run on Ubuntu server, And Have performed upgrade using Docker and Apache server both and its working smoothly.

**TIPS :** Its better to run on docker for easy switching PHP version

**1. Download the LIVE DB + CODEBASE**
unzip DB file then run the command which will remove DEFINER (This will help the migration in CLOUD) Below is the command example to remove it.

```shell
sed -i ‘s/DEFINER=[^*]*\*/\*/g’ mydump.sql
```

**2. Copy composer.json & composer.lock file for safe side**

**3. Now setup Magento in local + Configure Elastic search**

For now remove all the PAID extension and unused extension from composer (composer.json & composer.lock) file for now.

Create a file called auth.json file in root of Magento (AUTH credential must be Magento Enterprise/Cloud username and password.

**4. Run the below command to Upgrading Magento Community 2.3.6 to 2.4.0 (Or the latest Stable version of Magento)**

Remove DIR “patches” in root DIR

```shell
composer require magento/product-community-edition=2.4.0 --no-update
```

once the above change will be done you will see composer.json Magento version will change to 2.4.0,

Then run the below commands

```shell
rm composer.lock
composer update --ignore-platform-reqs
```

Once the above command will run completed, As it is Magento2.4.0 you need to configure the Elastic search first ( For testing purpose you can use trial version of elastic search from https://www.elastic.co for 14 days) and configure in Magento backend for catalog search and related operations. Because if Elastic search will not configure you will not able to run magneto command because after Magento2.4.* it requires Elastic search because they removed MySQL search engine.

Run Magento Commands

```shell
php -dmemory_limit=6G bin/magento setup:upgrade && php -dmemory_limit=6G bin/magento setup:di:compile && php -dmemory_limit=6G bin/magento setup:static-content:deploy -f && php bin/magento indexer:reindex && php -dmemory_limit=6G bin/magento cache:flush```
```
Now your Magento is upgraded to Magento 2.4.0 → Congratulations

**5. Now let's convert to Magento2 Community to Enterprise**

IMPORTANT: When you want to upgrade to cloud/enterprise then you can first upgrade to the same version to cloud/enterprise, Example if I want to upgrade to enterprise 2.4.0 then your community version must be in the same version i.e 2.4.0

**NOTE:**  Before installing the mageno2 cloud/enterprise version if you have to change the “Default” attribute set name then, you must have the attribute set name as “Default”, Because while migrating and set up the cloud/enterprise it will refer to the Default attribute set, If you will not set up then it will show multiple errors because from community to cloud Magento will install many new attributes and they will refer “Default” attribute set.

Run command

```shell
composer require magento/product-enterprise-edition=2.4.0 --no-update
```

Once the above command will run, In your composer.json file Magento enterprise edition entry will be there in the “require” section.

Run below commands

```shell
rm composer.lock
composer update --ignore-platform-reqs
composer clear-cache
composer install
php -dmemory_limit=6G bin/magento setup:upgrade && php -dmemory_limit=6G bin/magento setup:di:compile && php -dmemory_limit=6G bin/magento setup:static-content:deploy -f && php bin/magento indexer:reindex && php -dmemory_limit=6G bin/magento cache:flush
```
Once all command will run completed, Congratulations your magento is update to Enterprise version. But if you are facing any issue related to below errors in some file. Then below is the colution.

Ex :

1. vendor/magento/magento2-functional-testing-framework/src/Magento/FunctionalTestingFramework/_bootstrap.php LINE NO 72 And replace CODE

From
```php
if (!(bool)$debugMode && extension_loaded(‘xdebug’)) {
	xdebug_disable();
}

```
To
```php
if (!(bool)$debugMode && extension_loaded(‘xdebug’)) {
	if (function_exists(‘xdebug_disable’)) {
	xdebug_disable();
	}
}
```

Now your Magento is upgraded to Magento 2.4.0 Cloud / Enterprise version → Congratulations

**6. Now Upgrading Magento2 Enterprise to latest 2.4.2 Or latest 2.4.***

Run command
 ```shell
 composer require magento/product-community-edition=2.4.2 --no-update
 ```
Once the above command will run, In your composer.json file Magento enterprise edition entry with 2.4.2 will be there in the “require” section.

Run Command
```shell
rm composer.lock
composer update --ignore-platform-reqs
php -dmemory_limit=6G bin/magento setup:upgrade && php -dmemory_limit=6G bin/magento setup:di:compile && php -dmemory_limit=6G bin/magento setup:static-content:deploy -f && php bin/magento indexer:reindex && php -dmemory_limit=6G bin/magento cache:flush
```
Congratulations → Your magento 2 cloud / enterprise version is up to date.

If you are facing any issue or any problem you can reach out any time I will happy to help.

{"mode":"full","isActive":false}