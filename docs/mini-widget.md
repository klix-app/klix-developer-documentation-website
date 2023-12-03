# Mini widget

## Mini widget custom view

To enable mini-widget custom view you should have on page 2 global variables:

- `window.showCustomView = true;`
- ```
  window.customMiniWidget = {};

  window.customMiniWidget.populateData = function (data) {
    //function to handle submitted data
  }
  window.customMiniWidget.returnWidgetView = function () { 
    return `html to show`
  }
  ```

`customMiniWidget.populateData(data);` will be called to populate your custom view with data
`data` will be object with to fields `data.preferredOffer` and `data.offers` where fields will be
`FinancingProduct` and `FinancingProduct[]` accordingly

`data.preferredOffer` is preferred offer for this amount

`data.offers` is a list of all available product for provided amount and preferred product type for this amount

`customMiniWidget.returnWidgetView()` call should return string for render your custom design

Here id description for `FinancingProduct` class

| Parameter                   | Description                                                                                   | Type   |
|-----------------------------|-----------------------------------------------------------------------------------------------|--------|
| `id`                        | product id                                                                                    | string |
| `principalAmount`           | loan amount to borrowed                                                                       | number |
| `downPaymentAmount`         | down payment (will be more then 0 if requered by legal)                                       | number |
| `requestedAmount`           | total amount of the loan                                                                      | number |
| `apr`                       | annual percentage rate                                                                        | number |
| `commissionRate`            | commission rate                                                                               | number |
| `fixedCommission`           | commission amount if product have fixed commission amount                                     | number |
| `interestRate`              | interest rate                                                                                 | number |
| `gracePeriodRepaymentCount` | grace period (without any fees) if applicable                                                 | number |
| `totalRepaymentCount`       | total loan period                                                                             | number |
| `monthlySplitPaymentAmount` | loan monthly payment if loan going to be pai within grace period                              | number |
| `monthlyPaymentAmount`      | loan monthly payment with all fees applied                                                    | number |
| `monthlyPaymentGraceAmount` | loan monthly payment within grace period if extended term is chosen                           | number |
| `firstMonthPaymentAmount`   | first month payment for loan (monthly payment + commission fee in case of COMMISSION product) | number |
| `name`                      | name of the product                                                                           | string |
| `productType`               | product type. Possible values: `GRACE_PERIOD`, `INSTALLMENT_CREDIT`, `COMMISSION`             | string |
| `sequence`                  | sequence number in product priority list                                                      | number |
