---
title: Connect Vertex
description: Find out how you can confiure Sprykers third party Vertex in to your Spryker Based project.
last_updated: Nov 3 2023
template: howto-guide-template
---

This document describes how to connect a Spryker project to Vertex.

## Prerequisites

- [Install Vertex](/docs/pbc/all/tax-management/202311.0/base-shop/third-party-integrations/vertex/install-vertex/install-vertex.html)
- Create an account with [Vertex](https://www.vertexinc.com/). If you need support getting a Vertex account, [contact support](https://support.spryker.com/) or your Customer Success Manager.

## Connect Vertex

1. In the Back Office, go to **Apps**.
2. On the **App Composition Platform Catalog** page, click **Vertex**.
  This opens the Vertex app details page.
3. In the top right corner of the Vertex app details page, click **Connect app**.
  The notification saying that the application connection is pending is displayed.
4. In the top right corner of the Vertex app details page, click **Configure**.
5. To activate the app, select **Active**.
6. In **Security URI**, enter the Security URI of your Vertex platform. For details on the Security URI, see [Vertex documentation](https://tax-calc-api.vertexcloud.com/resources/index.html).
7. In **Transaction calls URI**, enter the Transaction Calls URI of your Vertex platform. For details on the Transaction Calls URI, see [Vertex documentation](https://tax-calc-api.vertexcloud.com/resources/index.html).
7. In **Client ID/Client secret**, enter your Vertex client secret. See [Vertex documentation](https://tax-calc-api.vertexcloud.com/resources/index.html) for details about how to obtain it.
8. For **DEFAULT TAXPAYER COMPANY CODE**, enter the company code you set in your Vertex account.
9. Optional: To enable invoice saving in Vertex, select **Enable invoice save in Vertex**.
10. Click **Save**.

![vertex-configuration](https://spryker.s3.eu-central-1.amazonaws.com/docs/pbc/all/tax-management/vertex/configure-vertex/vertex-configuration.png)

## Verify Vertex connection

{% info_block warningBox "Test the configuration" %}

To ensure accuracy and compliance with tax laws, we highly recommend thoroughly testing the Vertex integration.

{% endinfo_block %}

Once you've configured Vertex, the taxes are calculated in real time in the checkout. A message about this is displayed on the checkout page.

![vertex_checkout_page](https://spryker.s3.eu-central-1.amazonaws.com/docs/pbc/all/tax-management/vertex/configure-vertex/vertex_checkout_page.png)

On the Storefront, the tax amount is displayed on the checkout summary page.

In the Back Office, the taxes are displayed on the order details page.

If you configured invoices to be saved in Vertex, you can view the taxes processed by Vertex as follows:

1. In the Vertex dashboard, go to **Reporting** > **Standard Reports**.
2. Click **Report Output**.
3. Next to the report you want to view the taxes for, click **Action**>**View report**.
![vertex-report-output](https://spryker.s3.eu-central-1.amazonaws.com/docs/pbc/all/tax-management/vertex/configure-vertex/vertex-report-output.png)
4. On the invoice page, you can verify the invoice number that corresponds to the Spryker order number and the applicable country tax calculated by Vertex.
![invoice-in-vertex](https://spryker.s3.eu-central-1.amazonaws.com/docs/pbc/all/tax-management/vertex/configure-vertex/invoice-in-vertex.png)

## Retain Vertex configuration after a destructive deployment

{% info_block errorBox "" %}
[Destructive deployment](https://spryker.com/docs/dg/dev/acp/retaining-acp-apps-when-running-destructive-deployments.html) permanently deletes the configuration of Vertex.

To run a destructive deployment, follow the steps:
1. Disconnect Vertex.
2. Run a destructive deployment.
3. Reconnect Vertex.

{% endinfo_block %}

## Next steps

* [Vertex FAQ](/docs/pbc/all/tax-management/{{page.version}}/base-shop/third-party-integrations/vertex/vertex-faq.html)
* [Troubleshooting Vertex](/docs/pbc/all/tax-management/{{page.version}}/base-shop/third-party-integrations/vertex/troubleshooting-vertex.html)
