DBALManagerBundle
==================

This bundle provides DBALManager as a Symfony service.

Installation:
------------

1. In your composer.json file, add:

```json
"require": {
	"jarjak/dbal-manager-bundle": "dev-master"
},
"repositories": [
	{
		"type": "git",
		"url": "https://github.com/JarJak/DBALManagerBundle"
	}
],
```

2. Run: ```composer install```

3. Add bundle to your AppKernel.php:

```php
class AppKernel extends Kernel
{
    public function registerBundles()
    {
        $bundles = array(
            //...
            new JarJak\DBALManagerBundle\JarJakDBALManagerBundle(),
        );

        //...

        return $bundles;
    }

    //...
}
```

Example usage:
--------------

Lets say we have user table with: 
- unique usernames and emails
- column active can contain only 0 or 1 (not nullable)
- column address can be null

```php
$sqlArray = [
	'username' => 'JohnKennedy',
	'email' => 'john@kennedy.gov'
	'password' => $password,
	'address' => '',
	'active' => 0,
];

$DBALManager = $this->get('jarjak.dbal_manager');
$DBALManager->insertOrUpdateByArray('user', $sqlArray, 2, ['active']);
```

Multiple database connections
-----------------------------

If you have more than one DB connection, then you can create multiple managers, one for each connection.
All you need is to pass DBAL Connection service to setConnection().

```yaml
    secondary_dbal_manager:
        class: JarJak\DBALManager
        calls:
            - [setConnection, ["@secondary_connection"]]
```

or:

```php
$secondaryDBALManager = new JarJak\DBALManager();
$secondaryDBALManager->setConnection($this->get('secondary_connection'));
```

Dumping Queries
---------------

DBALManager can use VarDumper dump SQL Query from QueryBuilder ready to be copypasted into database server (with parameters already included).

```php
/* @var QueryBuilder $queryBuilder */
\JarJak\DBALManager::dumpQuery($queryBuilder);
```
