# Plugins for eCommerce platforms

Klix provides plugins for most popular eCommerce platforms so that Klix can be integrated as simple as other plugins you might be already using in your shop. Integration using eCommerce platform plugin consists of a plugin installation and configuration. Plugin installation instructions differs between eCommerce platforms but configuration usually consists of two steps:

1. Enabling Klix as a payment method.
2. Specifying Brand ID and Secret key in plugin settings.

If you have already signed Klix merchant agreement and have received production environment API keys then those should be used in these instructions. Otherwise there's an option to use [test-environment](/../test-environment) credentials to preview Klix payment gateway solution functionality.

## eCommerce platforms

### Magento

Compatible versions: 2.0+

Installation instructions:

1. Connect to Magento server terminal.
2. Create and navigate to a directory `app/code/` in your Magento root directory if does not exist already.
3. Download Klix Magento plugin: `curl https://portal.klix.app/ecommerce_modules/magento-v2.0+.zip --output magento-v2.0+.zip`
4. Extract Klix Magento plugin archive contents to `app/code/` directory so that there's `app/code/SpellPayment/Magento2Module` directory structure after that: `unzip -q magento-v2.0+.zip && rm magento-v2.0+.zip`
5. Navigate to Magento root directory and run `php bin/magento module:enable SpellPayment_ExpressCheckout --clear-static-content`
6. Run `php bin/magento setup:upgrade`
7. Run `php bin/magento setup:static-content:deploy` (note that message "Manual static content deployment is not required in "default" and "developer" modes" can be ignored).
![Run php bin/magento setup:static-content:deploy](../images/ecommerce-platforms/magento/1_run_commands.png#instruction-image)

8. Log in to your Magento store admin panel.
![Log in to your Magento store admin panel.](../images/ecommerce-platforms/magento/2_login.png#instruction-image)

### Mozello

Installation instructions:

1. Log in to your Mozello store administration portal.
2. Open `Catalog` -> `Catalog settings` -> `Payment` page and locate `Credit cards / payment platforms` section.
![Catalog settings](../images/ecommerce-platforms/mozello/1_provide_klix_settings.png#instruction-image)

3. Select "Klix by Citadele" in `Connect online payment platform` drop down and enter Brand ID and Secret key provided by Klix contact person. Change payment method title in checkout form if needed.
4. Go to your Mozello store checkout page and check that Klix payment method is available. It's advisable to test Klix integration using a real card.
![Mozello checkout](../images/ecommerce-platforms/mozello/2_preview_klix_payment_method.png#instruction-image)


### OpenCart

Compatible versions: 3.0-4+

Installation instructions:

1. Download plugin by Opencart version:
    * [3.0+](https://klixecom.blob.core.windows.net/modules/opencart-v3.0+.ocmod.zip)
    * [4.0+](https://klixecom.blob.core.windows.net/modules/klix.ocmod.zip) 
2. Log in to your OpenCart store admin panel by specifying authentication credentials.
![Opencart login page](../images/ecommerce-platforms/opencart/1_login.png#instruction-image)


3. Open "Extensions" -> "Installer" section from the main menu.
![Opencart extension installer page](../images/ecommerce-platforms/opencart/2_open_extension_upload_section.png#instruction-image)

4. Press "Upload" button and select Klix OpenCart extension file downloaded in the first step. Progress bar will indicate extension upload progress.
![Upload extension instruction](../images/ecommerce-platforms/opencart/3_upload_extension.png#instruction-image)

5. Open "Extensions" -> "Extensions" section from the main menu and filter payment extensions.
![Finding Klix plugin in extension page](../images/ecommerce-platforms/opencart/4_filter_extensions.png#instruction-image)

6. Find Klix extension in extension list and press "+" (Install).
![Klix extension install screen](../images/ecommerce-platforms/opencart/5_install_extension.png#instruction-image)

7. Press "Edit" Klix extension.
![Klix extension edit button screen](../images/ecommerce-platforms/opencart/6_edit_extension.png#instruction-image)

8. Enter Brand ID and Secret key provided by Klix contact person, mark "Enable API" to enable Klix payment method and select order payment success and error statuses.
![Klix extension settings edit screen](../images/ecommerce-platforms/opencart/7_edit_extension_settings.png#instruction-image)

9. Go to your OpenCart store checkout page and check that Klix payment method is available. It's advisable to test Klix integration using a real card.
![Checkout payment method selection screen](../images/ecommerce-platforms/opencart/8_preview_klix_payment_method.png#instruction-image)

10. Localize Klix checkout page (optional). By default all Klix payment method section text elements are in English. In order to add translations for Klix section to your store checkout page go to OpenCart store admin panel, open "Design" -> "Language Editor". Press on "+" (add new) button and for each language create translations for required keys: "klix_payment_method_selection" (Klix payment section title), "klix_place_order" (Klix pay button title). Note that empty translation key should be selected in translation key drop-down and one of previously mentioned Klix translation keys should be copied to text input field below translation key drop-down. In a given example translation for default "Select payment method" label is defined for Russian. In a similar way translation for pay button can be changed, the only difference is in translation key which should be "klix_place_order". Note that route should always be "checkout/checkout".
![Translation definition screen](../images/ecommerce-platforms/opencart/9_localize_klix_payment_section.png#instruction-image)

11. Go to your OpenCart store checkout page and preview translations by switching to desired language.
![Translated checkout payment method selection screen](../images/ecommerce-platforms/opencart/10_preview_translated_payment_section.png#instruction-image)

12. <b>Pay Later widget</b> will be enabled after modification refresh. Open "Extensions" -> "Modifications" section. Click on <b> refresh</b> button.
![Refresh modifications](../images/ecommerce-platforms/opencart/11_modifications_refresh.png#instruction-image)


### PrestaShop

Compatible versions: 1.7+

Installation instructions:

1. Download plugin by Prestashop version:
    * [1.6 - 1.6.1.11](https://klixecom.blob.core.windows.net/modules/klix-prestashop1.6.zip)
    * [1.7 - 1.7.5](https://portal.klix.app/ecommerce_modules/prestashop-v1.7+.zip) 
    * [1.7.6 - 1.7.8.9](https://klixecom.blob.core.windows.net/modules/klix-prestashop.zip) 
    * [8.0 or newer](https://klixecom.blob.core.windows.net/modules/klix-prestashop8.zip) 

2. Log in to your PrestaShop store admin panel by specifying authentication credentials.
![PrestaShop admin panel login screen](../images/ecommerce-platforms/prestashop/1_login.png#instruction-image)

3. Open "Modules" -> "Module Manager" section.
![Module mangager menu screen](../images/ecommerce-platforms/prestashop/2_open_module_manager.png#instruction-image)

4. Press "Upload a module" button.
![Open upload module dialog screen](../images/ecommerce-platforms/prestashop/3_upload_module_button.png#instruction-image)

5. Select a module file downloaded in the first step. Selected module file will be installed.
![Upload module screen](../images/ecommerce-platforms/prestashop/4_select_module_file.png#instruction-image)

6. After module is installed press "Configure" button.
![Installed module screen](../images/ecommerce-platforms/prestashop/5_press_configure_module.png#instruction-image)

7. Enter Brand ID and Secret key provided by Klix contact person, mark "Enable API" to enable Klix payment method.
![Klix module configuration screen](../images/ecommerce-platforms/prestashop/6_configure_module.png#instruction-image)

8. Localize Klix checkout page (optional). By default all Klix payment method section text elements are in English. In order to add translations for Klix section to your store checkout page press "Translate" button.
![Klix module configuration screen](../images/ecommerce-platforms/prestashop/7_translate_module.png#instruction-image)

9. Then choose your store language for which you would like to enter translations.
![Klix module translation language selection screen](../images/ecommerce-platforms/prestashop/8_choose_language.png#instruction-image)

10.  And provide translations for all provided texts.
![Klix translations screen](../images/ecommerce-platforms/prestashop/9_provide_translations.png#instruction-image)

11. Go to your PrestShop store checkout page and check that Klix payment method is available. It's advisable to test Klix integration using a real card.
![Checkout payment method selection screen](../images/ecommerce-platforms/prestashop/10_preview_klix_payment_method.png#instruction-image)


### Shopify

Installation instructions:

1. Login to [Business Portal](https://portal.klix.app/)
![After an install Shopify will redirect to our Business Portal page.](../images/ecommerce-platforms/shopify/business-portal.png#instruction-image)
2. After successful login open [Shopify app store](https://apps.shopify.com/klix-payments) in **new tab** and install the plugin.
3. During instalation Shopify will redirect to our Business Portal page to confirm Shopify store integration.
    ![After successful login click on Create in Business Portal.](../images/ecommerce-platforms/shopify/create-merchant-business-portal.png#instruction-image)
4. Business Portal will redirect back to Shopify settings to settings view. In settings click on **Activate Klix payments**.
    ![Business Portal will redirect back to Shopify settings to settings view.](../images/ecommerce-platforms/shopify/activate-klix-payments.png#instruction-image)
5. Switch **Payment capture method** to automatic.
![Switch Payment capture method to automatic](../images/ecommerce-platforms/shopify/payment-capture-method-1.png#instruction-image)
![Switch Payment capture method to automatic](../images/ecommerce-platforms/shopify/automatic-capture.png#instruction-image)
6. Go to your Shopify store checkout page and check that Klix payment method is available. For test purposes please enable **Enable test mode** checkbox.

### WooCommerce

Compatible versions: 3.5+

Installation instructions:

1. Click on [this link](https://klixecom.blob.core.windows.net/modules/klix-woocommerce.zip) to download Klix WooCommerce plugin.
2. Log in to your WooCommerce store admin panel by specifying authentication credentials.
![WooCommerce admin panel](../images/ecommerce-platforms/woocommerce/1_login.png#instruction-image)

3. Open "Plugins" -> "Add new" section.
![Plugin menu screen](../images/ecommerce-platforms/woocommerce/2_select_plugin_menu.png#instruction-image)

4. Press on "Upload Plugin" and select Klix WooCommerce plugin file downloaded in the first step, press "Install Now". In rare cases e.g. if your server is not configured to allow automatic installations you might need to install Klix WooCommerce plugin by manually transfering Klix plugin files onto the server. Follow [these instructions](https://wordpress.org/support/article/managing-plugins/#manual-plugin-installation) in such case.
![Upload plugin screen](../images/ecommerce-platforms/woocommerce/3_upload_plugin.png#instruction-image)

5. Press "Activate" button to activate installed Klix WooCommerce plugin.
![Finished plugin installation screen](../images/ecommerce-platforms/woocommerce/4_activate_plugin.png#instruction-image)

6. Find Klix plugin and press on "Settings" link.
![Plugins screen](../images/ecommerce-platforms/woocommerce/5_find_klix_plugin.png#instruction-image)

7. Enter Brand ID and Secret key provided by Klix contact person and check `Enable API` checkbox.
![Klix plugin settings screen](../images/ecommerce-platforms/woocommerce/6_specify_klix_plugin_settings.png#instruction-image)

8. Go to your WooCommerce store checkout page and check that Klix payment method is available. It's advisable to test Klix integration using a real card.
![Payment option selection screen](../images/ecommerce-platforms/woocommerce/7_test_klix_payment_option.png#instruction-image)
