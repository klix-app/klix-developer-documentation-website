# Plugins for eCommerce plartforms

Klix provides plugins for most popular eCommerce platforms so Klix can be integrated as simple as other plugins you might be already using in your shop. Integration using eCommerce platform plugin consists of a plugin installation and configuration. Plugin installation instructions differs between eCommerce platforms but configuration usually consists of two steps:

1. Enabling Klix as a payment method.
2. Specifying Brand ID and secret API key in plugin settings.

## eCommerce plartforms

### Magento

Compatible versions: 2.0+

Installation instructions:

1. Connect to Magento server terminal.
2. Create and navigate to a directory `app/code/` in your Magento root directory if does not exist already.
3. Download Klix Magento plugin: `curl https://portal.klix.app/ecommerce_modules/magento-v2.0+.zip --output magento-v2.0+.zip`
4. Extract Klix Magento plugin archive contents to `app/code/` directory so that there's `app/code/SpellPayment/Magento2Module` directory structure after that: `unzip -q magento-v2.0+.zip && rm magento-v2.0+.zip`
5. Navigate to Magento root directory and run `php bin/magento module:enable SpellPayment_Magento2Module --clear-static-content`
6. Run `php bin/magento setup:upgrade`
7. Run `php bin/magento setup:static-content:deploy` (note that message "Manual static content deployment is not required in "default" and "developer" modes" can be ignored).
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/magento/1_run_commands.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="Terminal screen" title="Run Klix plugin installation commands in terminal" />
</div>
<!-- markdownlint-disable MD033 -->
8. Log in to your Magento store admin panel.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/magento/2_login.png" style="display:block;margin-left:auto;margin-right:auto;width:50%;margin-top:5px;margin-bottom:10px;" alt="Magento admin panel login screen" title="Log in to your Magento admin panel" />
</div>
<!-- markdownlint-disable MD033 -->

### OpenCart

Compatible versions: 3.0+

Installation instructions:

1. Click on [this link](https://portal.klix.app/ecommerce_modules/opencart-v3.0+.zip) to download Klix OpenCart extension.
2. Log in to your OpenCart store admin panel by specifying authentication credentials.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/1_login.png" style="display:block;margin-left:auto;margin-right:auto;width:75%;margin-top:5px;margin-bottom:10px;" alt="OpenCart admin panel login screen" title="Log in to your OpenCart admin panel" />
</div>
<!-- markdownlint-disable MD033 -->
3. Open "`Extensions`" -> "`Installer`" section from the main menu.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/2_open_extension_upload_section.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="OpenCart extension upload screen" title="Open extension upload section" />
</div>
<!-- markdownlint-disable MD033 -->
4. Press "`Upload`" button and select Klix OpenCart extension file downloaded in the first step. Progress bar will indicate extension upload progress.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/3_upload_extension.png" style="display:block;margin-left:auto;margin-right:auto;width:85%;margin-top:5px;margin-bottom:10px;" alt="OpenCart extension install screen" title="Upload extension" />
</div>
<!-- markdownlint-disable MD033 -->
5. Open "`Extensions`" -> "`Extensions`" section from the main menu and filter payment extensions.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/4_filter_extensions.png" style="display:block;margin-left:auto;margin-right:auto;width:85%;margin-top:5px;margin-bottom:10px;" alt="Extensions filter screen" title="Filter payment extensions" />
</div>
<!-- markdownlint-disable MD033 -->
6. Find Klix extension in extension list and press "`+`" (Install).
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/5_install_extension.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="Klix extension install screen" title="Install Klix extension" />
</div>
<!-- markdownlint-disable MD033 -->
7. Press "`Edit`" Klix extension.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/6_edit_extension.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="Klix extension edit button screen" title="Edit Klix extension" />
</div>
<!-- markdownlint-disable MD033 -->
8. Enter Brand ID and Secret key provided by Klix contact person, mark "`Enable API`" to enable Klix payment method and select order payment success and error statuses.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/7_edit_extension_settings.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="Klix extension settings edit screen" title="Edit Klix extension settings" />
</div>
<!-- markdownlint-disable MD033 -->
9. Go to your OpenCart store checkout page and check that Klix payment method is available. It's advisable to test Klix integration using a real card.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/8_preview_klix_payment_method.png" style="display:block;margin-left:auto;margin-right:auto;width:90%;margin-top:5px;margin-bottom:10px;" alt="Checkout payment method selection screen" title="Preview Klix payment method" />
</div>
<!-- markdownlint-disable MD033 -->

### Prestashop

### Shopify

Installation instructions:

1. Click on [this link](https://www.shopify.com/login?redirect=%2Fadmin%2Fauthorize_gateway%2F1055469) in order to open Shopify login screen.
2. Log in to your Shopify store by specifying store address, email and password.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/shopify/1_login.png" style="display:block;margin-left:auto;margin-right:auto;width:50%;margin-top:5px;margin-bottom:10px;" alt="Shopify login screen" title="Log in to your Shopify store" />
</div>
<!-- markdownlint-disable MD033 -->
3. Confirm Klix payment provider installation.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/shopify/2_install.png" style="display:block;margin-left:auto;margin-right:auto;width:70%;margin-top:5px;margin-bottom:10px;" alt="Klix payment provider installation confirmation screen" title="Confirm Klix payment provider installation" />
</div>
<!-- markdownlint-disable MD033 -->
4. Open `Settings` -> `Payments` section.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/shopify/3_payments_settings.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="Payments section on settings screen" title="Open payments settings" />
</div>
<!-- markdownlint-disable MD033 -->
5. Press `Choose alternative payment` button in `Alternative payment methods` section.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/shopify/4_alternative_payment_methods.png" style="display:block;margin-left:auto;margin-right:auto;width:70%;margin-top:5px;margin-bottom:10px;" alt="Alternative payment methods screen" title="Choose alternative payment method" />
</div>
<!-- markdownlint-disable MD033 -->
6. Find and open Klix payment provider settings.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/shopify/5_find_klix_payment_provider.png" style="display:block;margin-left:auto;margin-right:auto;width:90%;margin-top:5px;margin-bottom:10px;" alt="Payment provider search screen" title="Find and open Klix payment provider settings" />
</div>
<!-- markdownlint-disable MD033 -->
7. Enter Brand ID and Secret key provided by Klix contact person and press `Activate Klix` button.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/shopify/6_activate_klix.png" style="display:block;margin-left:auto;margin-right:auto;width:70%;" alt="Klix payment method configuration screen" title="Provide Klix configuration" />
</div>
<!-- markdownlint-disable MD033 -->
8. Go to your Shopify store checkout page and check that Klix payment method is available. It's advisable to test Klix integration using a real card.
<div>
    <img src="../images/ecommerce-platforms/shopify/7_preview_klix_payment_method.png" style="display:block;margin-left:auto;margin-right:auto;width:70%;" alt="Checkout payment method selection screen" title="Preview Klix payment method" />
</div>
<!-- markdownlint-disable MD033 -->

### WooCommerce
