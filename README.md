DBALManagerBundle
==================

[![SensioLabsInsight](https://insight.sensiolabs.com/projects/26cdcbf9-dd47-452a-a933-f954ecd90d03/big.png)](https://insight.sensiolabs.com/projects/26cdcbf9-dd47-452a-a933-f954ecd90d03)

This bundle provides [DBALManager](https://github.com/JarJak/DBALManager) as a Symfony service.

Installation:
------------

1. Run:
```composer require jarjak/dbal-manager-bundle```

2. [Symfony <4 only] Add bundle to your AppKernel.php:

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

Simple example:
---

You want to insert data or update them if row already exists.

```php
$sqlArray = [
	'id' => 1,
	'username' => 'JohnKennedy',
	'email' => 'john@kennedy.gov'
];

$dbalManager = $this->get('jarjak.dbal_manager');
$dbalManager->insertOrUpdate('user', $sqlArray);
```
Or you want to just skip this row if it exists:
```php
$dbalManager->insertIgnore('user', $sqlArray);
```

Advanced example:
---

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

$dbalManager = $this->get(DBALManager::class);
$dbalManager->insertOrUpdate('user', $sqlArray, 2, ['active']);
```

Multiple database connections
-----------------------------

If you have more than one DB connection, then you can create multiple managers, one for each connection.
All you need is to pass DBAL Connection service (`@secondary_connection`) to setConnection() or constructor.

```yaml
secondary_dbal_manager:
    class: JarJak\DBALManager
    arguments:
        - "@secondary_connection"
```
or:

```php
$secondaryDBALManager = new JarJak\DBALManager($this->get('secondary_connection'));
```

Dumping Queries
---------------

DBALManager can use VarDumper dump SQL Query from QueryBuilder ready to be copypasted into database server (with parameters already included).

```php
/* @var QueryBuilder $queryBuilder */
JarJak\SqlDumper::dumpQuery($queryBuilder);
```

If you don't use QueryBuilder you can still dump parametrized SQL with:

```php
JarJak\SqlDumper::dumpSql($sql, $params);
```
