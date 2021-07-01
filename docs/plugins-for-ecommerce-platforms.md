# Plugins for eCommerce plartforms

Klix provides plugins for most popular eCommerce platforms so that Klix can be integrated as simple as other plugins you might be already using in your shop. Integration using eCommerce platform plugin consists of a plugin installation and configuration. Plugin installation instructions differs between eCommerce platforms but configuration usually consists of two steps:

1. Enabling Klix as a payment method.
2. Specifying Brand ID and Secret key in plugin settings.

If you have already signed Klix merchant agreement and have received production environment API keys then those should be used in these instructions. Otherwise there's an option to use [test-environment](/../test-environment) credentials to preview Klix payment gateway solution functionality.

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

### Mozello

Installation instructions:

1. Log in to your Mozello store administration portal.
2. Open `Catalog` -> `Catalog settings` -> `Payment` page and locate `Credit cards / payment platforms` section.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/mozello/1_provide_klix_settings.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="Klix payment method configuration screen in Mozello" title="Configure Klix payment method in Mozello" />
</div>
<!-- markdownlint-disable MD033 -->
3. Select "Klix by Citadele" in `Connect online payment platform` drop down and enter Brand ID and Secret key provided by Klix contact person. Change payment method title in checkout form if needed.
4. Go to your Mozello store checkout page and check that Klix payment method is available. It's advisable to test Klix integration using a real card.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/mozello/2_preview_klix_payment_method.png" style="display:block;margin-left:auto;margin-right:auto;width:80%;margin-top:5px;margin-bottom:10px;" alt="Checkout payment method selection screen in Mozello store" title="Preview Klix payment method in Mozello store" />
</div>
<!-- markdownlint-disable MD033 -->

### OpenCart

Compatible versions: 3.0+

Installation instructions:

1. Click on [this link](https://drive.google.com/uc?export=download&id=1OEann_0BeyVUpiWBjQNAQC9GaJioktTr) to download Klix OpenCart extension.
2. Log in to your OpenCart store admin panel by specifying authentication credentials.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/1_login.png" style="display:block;margin-left:auto;margin-right:auto;width:75%;margin-top:5px;margin-bottom:10px;" alt="OpenCart admin panel login screen" title="Log in to your OpenCart admin panel" />
</div>
<!-- markdownlint-disable MD033 -->
3. Open "Extensions" -> "Installer" section from the main menu.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/2_open_extension_upload_section.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="OpenCart extension upload screen" title="Open extension upload section" />
</div>
<!-- markdownlint-disable MD033 -->
4. Press "Upload" button and select Klix OpenCart extension file downloaded in the first step. Progress bar will indicate extension upload progress.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/3_upload_extension.png" style="display:block;margin-left:auto;margin-right:auto;width:85%;margin-top:5px;margin-bottom:10px;" alt="OpenCart extension install screen" title="Upload extension" />
</div>
<!-- markdownlint-disable MD033 -->
5. Open "Extensions" -> "Extensions" section from the main menu and filter payment extensions.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/4_filter_extensions.png" style="display:block;margin-left:auto;margin-right:auto;width:85%;margin-top:5px;margin-bottom:10px;" alt="Extensions filter screen" title="Filter payment extensions" />
</div>
<!-- markdownlint-disable MD033 -->
6. Find Klix extension in extension list and press "+" (Install).
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/5_install_extension.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="Klix extension install screen" title="Install Klix extension" />
</div>
<!-- markdownlint-disable MD033 -->
7. Press "Edit" Klix extension.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/6_edit_extension.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="Klix extension edit button screen" title="Edit Klix extension" />
</div>
<!-- markdownlint-disable MD033 -->
8. Enter Brand ID and Secret key provided by Klix contact person, mark "Enable API" to enable Klix payment method and select order payment success and error statuses.
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
10. Localize Klix checkout page (optional). By default all Klix payment method section text elements are in English. In order to add translations for Klix section to your store checkout page go to OpenCart store admin panel, open "Design" -> "Language Editor". Press on "+" (add new) button and for each language create translations for required keys: "klix_payment_method_selection" (Klix payment section title), "klix_place_order" (Klix pay button title). In given example translation for default "Select payment method" label is defined for Russian. In a similar way translation for pay button can be changed, the only difference is in translation key which should be "klix_place_order". Note that route should always be "checkout/checkout".
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/9_localize_klix_payment_section.png" style="display:block;margin-left:auto;margin-right:auto;width:60%;margin-top:5px;margin-bottom:10px;" alt="Translation definition screen" title="Add translation" />
</div>
<!-- markdownlint-disable MD033 -->
11. Go to your OpenCart store checkout page and preview translations by switching to desired language.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/opencart/10_preview_translated_payment_section.png" style="display:block;margin-left:auto;margin-right:auto;width:95%;margin-top:5px;margin-bottom:10px;" alt="Translated checkout payment method selection screen" title="Preview translations" />
</div>
<!-- markdownlint-disable MD033 -->

### PrestaShop

Compatible versions: 1.7+

Installation instructions:

1. Click on [this link](https://drive.google.com/uc?export=download&id=1mE1_h1z_lqFjIDZTevv75IqBYOzRiMZN) to download Klix PrestaShop plugin in case your store Prestshop version is 1.7.6. or newer. If you use Prestashop version that's older than 1.7.6. then click on [this link](https://portal.klix.app/ecommerce_modules/prestashop-v1.7+.zip) (note that translations functionality will work only with Prestashop 1.7.6. and newer version).
2. Log in to your PrestaShop store admin panel by specifying authentication credentials.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/prestashop/1_login.png" style="display:block;margin-left:auto;margin-right:auto;width:60%;margin-top:5px;margin-bottom:10px;" alt="PrestaShop admin panel login screen" title="Log in to your PrestaShop admin panel" />
</div>
<!-- markdownlint-disable MD033 -->
3. Open "Modules" -> "Module Manager" section.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/prestashop/2_open_module_manager.png" style="display:block;margin-left:auto;margin-right:auto;width:35%;margin-top:5px;margin-bottom:10px;" alt="Module mangager menu screen" title="Open module manager" />
</div>
<!-- markdownlint-disable MD033 -->
4. Press "Upload a module" button.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/prestashop/3_upload_module_button.png" style="display:block;margin-left:auto;margin-right:auto;width:90%;margin-top:5px;margin-bottom:10px;" alt="Open upload module dialog screen" title="Open module upload dialog" />
</div>
<!-- markdownlint-disable MD033 -->
5. Select a module file downloaded in the first step. Selected module file will be installed.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/prestashop/4_select_module_file.png" style="display:block;margin-left:auto;margin-right:auto;width:70%;margin-top:5px;margin-bottom:10px;" alt="Upload module screen" title="Upload Klix module" />
</div>
<!-- markdownlint-disable MD033 -->
6. After module is installed press "Configure" button.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/prestashop/5_press_configure_module.png" style="display:block;margin-left:auto;margin-right:auto;width:70%;margin-top:5px;margin-bottom:10px;" alt="Installed module screen" title="Open Klix module configuration" />
</div>
<!-- markdownlint-disable MD033 -->
7. Enter Brand ID and Secret key provided by Klix contact person, mark "Enable API" to enable Klix payment method.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/prestashop/6_configure_module.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="Klix module configuration screen" title="Configure Klix module" />
</div>
<!-- markdownlint-disable MD033 -->
8. Localize Klix checkout page (optional). By default all Klix payment method section text elements are in English. In order to add translations for Klix section to your store checkout page press "Translate" button.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/prestashop/7_translate_module.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="Klix module configuration screen" title="Translate Klix module" />
</div>
<!-- markdownlint-disable MD033 -->
9. Then choose your store language for which you would like to enter translations.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/prestashop/8_choose_language.png" style="display:block;margin-left:auto;margin-right:auto;width:40%;margin-top:5px;margin-bottom:10px;" alt="Klix module translation language selection screen" title="Choose translation language" />
</div>
<!-- markdownlint-disable MD033 -->
10.  And provide translations for all provided texts.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/prestashop/9_provide_translations.png" style="display:block;margin-left:auto;margin-right:auto;width:90%;margin-top:5px;margin-bottom:10px;" alt="Klix translations screen" title="Translate Klix module texts" />
</div>
11. Go to your PrestShop store checkout page and check that Klix payment method is available. It's advisable to test Klix integration using a real card.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/prestashop/10_preview_klix_payment_method.png" style="display:block;margin-left:auto;margin-right:auto;width:90%;margin-top:5px;margin-bottom:10px;" alt="Checkout payment method selection screen" title="Preview Klix payment method" />
</div>
<!-- markdownlint-disable MD033 -->

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

Compatible versions: 3.5+

Installation instructions:

1. Click on [this link](https://drive.google.com/uc?export=download&id=1RBGG79wJ6xPeT_T_JYXfpvx8lDjAlHQ6) to download Klix WooCommerce plugin.
2. Log in to your WooCommerce store admin panel by specifying authentication credentials.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/woocommerce/1_login.png" style="display:block;margin-left:auto;margin-right:auto;width:45%;margin-top:5px;margin-bottom:10px;" alt="WooCommerce admin panel login screen" title="Log in to your WooCommerce admin panel" />
</div>
<!-- markdownlint-disable MD033 -->
3. Open "Plugins" -> "Add new" section.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/woocommerce/2_select_plugin_menu.png" style="display:block;margin-left:auto;margin-right:auto;width:20%;margin-top:5px;margin-bottom:10px;" alt="Plugin menu screen" title="Open new plugin section" />
</div>
<!-- markdownlint-disable MD033 -->
4. Press on "Upload Plugin" and select Klix WooCommerce plugin file downloaded in the first step, press "Install Now". In rare cases e.g. if your server is not configured to allow automatic installations you might need to install Klix WooCommerce plugin by manually transfering Klix plugin files onto the server. Follow [these instructions](https://wordpress.org/support/article/managing-plugins/#manual-plugin-installation) in such case.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/woocommerce/3_upload_plugin.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="Upload plugin screen" title="Select and install plugin from file" />
</div>
<!-- markdownlint-disable MD033 -->
5. Press "Activate" button to activate installed Klix WooCommerce plugin.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/woocommerce/4_activate_plugin.png" style="display:block;margin-left:auto;margin-right:auto;width:80%;margin-top:5px;margin-bottom:10px;" alt="Finished plugin installation screen" title="Activate plugin" />
</div>
<!-- markdownlint-disable MD033 -->
6. Find Klix plugin and press on "Settings" link.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/woocommerce/5_find_klix_plugin.png" style="display:block;margin-left:auto;margin-right:auto;width:90%;margin-top:5px;margin-bottom:10px;" alt="Plugins screen" title="Find Klix plugin and open plugin settings" />
</div>
<!-- markdownlint-disable MD033 -->
7. Enter Brand ID and Secret key provided by Klix contact person and check `Enable API` checkbox.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/woocommerce/6_specify_klix_plugin_settings.png" style="display:block;margin-left:auto;margin-right:auto;width:100%;margin-top:5px;margin-bottom:10px;" alt="Klix plugin settings screen" title="Specify Klix plugin settings" />
</div>
<!-- markdownlint-disable MD033 -->
8. Go to your WooCommerce store checkout page and check that Klix payment method is available. It's advisable to test Klix integration using a real card.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/woocommerce/7_test_klix_payment_option.png" style="display:block;margin-left:auto;margin-right:auto;width:65%;margin-top:5px;margin-bottom:10px;" alt="Payment option selection screen" title="Test Klix payment option" />
</div>
<!-- markdownlint-disable MD033 -->
9. In order to use Klix-Checkout in plugin settings unselect "Enable payment method selection" and select "Enable directed payment".  
Then from shipping options fetch the shipping method titles and send them to support@klix.app.  
In case of pickup in store, please send the list of shop addresses.
<!-- markdownlint-disable MD033 -->
<div>
    <img src="../images/ecommerce-platforms/woocommerce/8_shipping_options.png" style="display:block;margin-left:auto;margin-right:auto;width:65%;margin-top:5px;margin-bottom:10px;" alt="Shipping options" title="Shipping options" />
</div>
<!-- markdownlint-disable MD033 -->
