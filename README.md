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

Usage examples:
---------------
You can get DBALManager as service in two ways:
```
$container->get('jarjak.dbal_manager');
$container->get(DBALManager::class);
```
For usage examples please refer to [DBALManager documentation](https://github.com/JarJak/DBALManager/blob/master/README.md).

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
