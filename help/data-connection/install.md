---
title: Install [!DNL Data Connection]
description: Learn how to install, update, and uninstall the [!DNL Data Connection] extension from Adobe Commerce.
exl-id: e78e8ab0-8757-4ab6-8ee1-d2e137fe6ced
role: Admin, Developer
feature: Install
---
# Install [!DNL Data Connection]

Before you install the extension, [review the prerequisites](overview.md#prereqs).

## Install the extension

The [!DNL Data Connection] extension is available from the [Adobe Marketplace](https://commercemarketplace.adobe.com/magento-experience-platform-connector.html). When you install this extension from the command line of the server, it connects to your Adobe Commerce installation as a [service](../landing/saas.md). When the process is complete, **[!DNL Data Connection]** and **Commerce Services Connector** appear on the **System** menu under **Services** in the Commerce _Admin_.

>[!IMPORTANT]
>
>While the name of the extension has changed from Experience Platform connector to [!DNL Data Connection], the package name remains `experience-platform-connector` to support backward compatibility.

1. To download the `experience-platform-connector` package, run the following from the command line:

   ```bash
   composer require magento/experience-platform-connector
   ```

   This metapackage contains the following modules and extensions:

   * `module-experience-connector-admin` - Updates the Admin UI so you can select the Datastream ID for a specific Adobe Commerce instance.
   * `module-experience-connector` - Sets the `Organization ID` and `datastreamId` in the Storefront Events SDK.
   * `data-services` - Provides attribute context for storefront events. For example, when a checkout event occurs, information about how many items were in the cart and product attribute data for those items are included.
   * `services-id` - Connects your Adobe Commerce instance to [Adobe Commerce SaaS](../landing/saas.md) using sandbox and production API keys and to the Adobe Experience Platform to retrieve the IMS Organization ID.
   * `orders-connector` - Connects the order status service to your Adobe Commerce instance.

1. (Optional) To include [!DNL Live Search] data, which comprises [search events](events.md#search-events), install the [[!DNL Live Search]](../live-search/install.md) extension.

1. (Optional) To include B2B data, which comprises [requisition events](events.md#b2b-events), install the [B2B extension](#install-the-b2b-extension).

### Configure the orders connector

After you install the `experience-platform-connector` extension, you must finalize installation of the `orders-connector` module based on the deployment type: on-premises or Adobe Commerce on Cloud infrastructure.

#### On-premises

In on-premises environments, you must manually enable code generation and Adobe Commerce Events:

   ```bash
   bin/magento events:generate:module
   bin/magento module:enable Magento_AdobeCommerceEvents
   bin/magento setup:upgrade
   bin/magento setup:di:compile
   bin/magento config:set adobe_io_events/eventing/enabled 1
   ```

#### On Cloud infrastructure

In Adobe Commerce on Cloud infrastructure, enable the `ENABLE_EVENTING` global variable in `.magento.env.yaml`. [Learn more](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-global.html#enable_eventing).

   ```bash
   stage:
      global:
         ENABLE_EVENTING: true
   ```

Commit and push updated files to the Cloud environment. When deployment is finished, enable sending events with the following command:

   ```bash
   bin/magento config:set adobe_io_events/eventing/enabled 1
   ```

### Install the B2B extension

For B2B merchants, install the following extension to include [requisition list](events.md#b2b-events) event data.

Download the `magento/experience-platform-connector-b2b` extension by running the following from the command line:

   ```bash
   composer require magento/experience-platform-connector-b2b
   ```

## Update the [!DNL Data Connection] extension {#update}

To update the [!DNL Data Connection] extension, run the following from the command line:

```bash
composer update magento/experience-platform-connector --with-dependencies
```

Or, for B2B merchants:

```bash
composer update magento/experience-platform-connector-b2b --with-dependencies
```

To update to a major version such as from 2.0.0 to 3.0.0, edit the project's root [!DNL Composer] `.json` file as follows:

1. Open the root `composer.json` file and search for `magento/experience-platform-connector`.

1. In the `require` section, update the version number as follows:

   ```json
   "require": {
      ...
      "magento/experience-platform-connector": "^3.0",
      ...
    }
   ```

1. **Save** `composer.json`. Then, run the following from the command line:

   ```bash
   composer update magento/experience-platform-connector –-with-dependencies
   ```

   Or, for B2B merchants:

   ```bash
   composer update magento/experience-platform-connector-b2b --with-dependencies
   ```

## Uninstall the [!DNL Data Connection] extension {#uninstall}

To uninstall the [!DNL Data Connection] extension, refer to [uninstall modules](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/uninstall-modules.html).
