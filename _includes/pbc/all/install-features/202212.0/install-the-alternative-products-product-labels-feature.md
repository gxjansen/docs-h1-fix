

This document describes how to integrate the Alternative Products + Product Labels feature into a Spryker project.

## Install feature core

Follow the steps below to install the Alternative Products + Product Labels feature core.

### Prerequisites

To start feature integration, integrate the required features:

| NAME | VERSION | INTEGRATION GUIDE|
|---|---|---|
|Alternative Products| {{page.version}} | [Alternative Products feature integration](/docs/scos/dev/feature-integration-guides/{{page.version}}/alternative-products-feature-integration.html)|
|Product Labels| {{page.version}} | [Product Labels feature integration](/docs/scos/dev/feature-integration-guides/{{page.version}}/product-labels-feature-integration.html)|

### 1) Install the required modules using Composer

Install the required modules:

```yaml
composer require spryker/product-alternative-product-label-connector:"^1.0.0" --update-with-dependencies
```
{% info_block warningBox "Verification" %}

Make sure that the following modules have been installed:

| MODULE | EXPECTED DIRECTORY |
| --- | --- |
| ProductAlternativeProductLabelConnector | vendor/spryker/product-alternative-product-label-connector |

{% endinfo_block %}

### 2) Add infrastructural data

1. Insall the plugins:

| PLUGIN | SPECIFICATION | PREREQUISITES | NAMESPACE |
|---|---|---|---|
|ProductAlternativeProductLabelConnectorInstallerPlugin|Installs the configured infrastructural alternative product labels.|None|Spryker\Zed\ProductAlternativeProductLabelConnector\Communication\Plugin\Installer|

2. Add the following to your project:

**src/Pyz/Zed/Installer/InstallerDependencyProvider.php**

```php   
<?php

namespace Pyz\Zed\Installer;

use Spryker\Zed\Installer\InstallerDependencyProvider as SprykerInstallerDependencyProvider;
use Spryker\Zed\ProductAlternativeProductLabelConnector\Communication\Plugin\Installer\ProductAlternativeProductLabelConnectorInstallerPlugin;

class InstallerDependencyProvider extends SprykerInstallerDependencyProvider
{
	/**
	 * @return \Spryker\Zed\Installer\Dependency\Plugin\InstallerPluginInterface[]
	 */
	public function getInstallerPlugins()
	{
		return [
			new ProductAlternativeProductLabelConnectorInstallerPlugin(),
		];
	}
}
```

3. Execute registered installer plugins and install the infrastructural data:

```bash
console setup:init-db
```

{% info_block warningBox "Verification" %}

Make sure that the configured infrastructural alternative product label has been added to the `spy_product_label` table in the database.

{% endinfo_block %}

### 3) Setup alternative products labels workflow

Enable the following behavior types by registering the plugins:

| PLUGIN | SPECIFICATION | PREREQUISITES | NAMESPACE |
|---|---|---|---|
|PostProductAlternativeCreatePlugin|After the product alternative is created, adds product alternatives availability label to the abstract product.|None|Spryker\Zed\ProductAlternativeProductLabelConnector\Communication\Plugin|
|PostProductAlternativeDeletePlugin|After the product alternative is deleted, removes product alternatives availability label from the abstract product.|None|Spryker\Zed\ProductAlternativeProductLabelConnector\Communication\Plugin|
|ProductAlternativeLabelUpdaterPlugin|Used to persist alternative product label relation changes into the database. <br>The plugin is called when the `ProductLabelRelationUpdaterConsole` command is executed.|None|Spryker\Zed\ProductAlternativeProductLabelConnector\Communication\Plugin|

**src/Pyz/Zed/ProductAlternative/ProductAlternativeDependencyProvider.php**

```php
<?php

namespace Pyz\Zed\ProductAlternative;

use Spryker\Zed\ProductAlternativeProductLabelConnector\Communication\Plugin\PostProductAlternativeCreatePlugin;
use Spryker\Zed\ProductAlternativeProductLabelConnector\Communication\Plugin\PostProductAlternativeDeletePlugin;
use Spryker\Zed\ProductAlternative\ProductAlternativeDependencyProvider as SprykerProductAlternativeDependencyProvider;

class ProductAlternativeDependencyProvider extends SprykerProductAlternativeDependencyProvider
{
    /**
     * @return \Spryker\Zed\ProductAlternativeExtension\Dependency\Plugin\PostProductAlternativeCreatePluginInterface[]
     */
    protected function getPostProductAlternativeCreatePlugins(): array
    {
        return [
            new PostProductAlternativeCreatePlugin(),
        ];
    }

    /**
     * @return \Spryker\Zed\ProductAlternativeExtension\Dependency\Plugin\PostProductAlternativeDeletePluginInterface[]
     */
    protected function getPostProductAlternativeDeletePlugins(): array
    {
        return [
            new PostProductAlternativeDeletePlugin(),
        ];
    }
}
```

**src/Pyz/Zed/ProductLabel/ProductLabelDependencyProvider.php**

```php
<?php

namespace Pyz\Zed\ProductLabel;

use Spryker\Zed\ProductAlternativeProductLabelConnector\Communication\Plugin\ProductAlternativeLabelUpdaterPlugin;
use Spryker\Zed\ProductLabel\ProductLabelDependencyProvider as SprykerProductLabelDependencyProvider;

class ProductLabelDependencyProvider extends SprykerProductLabelDependencyProvider
{
    /**
     * @return \Spryker\Zed\ProductLabel\Dependency\Plugin\ProductLabelRelationUpdaterPluginInterface[]
     */
    protected function getProductLabelRelationUpdaterPlugins()
    {
        return [
            new ProductAlternativeLabelUpdaterPlugin(),
        ];
    }
}
```

{% info_block warningBox "Verification" %}

Make sure the following happens:
- When you add product alternatives, it adds the corresponding label to the product.
- When you remove product alternatives, it removes the corresponding label from the product.

{% endinfo_block %}