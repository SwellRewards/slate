## Make Redemption

```javascript
$(document).on("swell:setup", function() {
  // depending on your cart/checkout markup the selectors will need to be updated
  var fillAndSubmitCouponCodeForm = function(couponCode) {
    // set the value for the coupon code input
    $("#coupon-code-input-element").val(couponCode);

    // trigger a click on the submit button
    $("#coupon-code-submit-btn").click();
  };

  var redemptionOptionSelect = $("#redemption-option-select");

  var onSuccess = function(redemption) {
    fillAndSubmitCouponCodeForm(redemption.couponCode);
  };
  var onError = function(err) {
    $("#error").show();
  };

  swellAPI.makeRedemption(
    { redemptionOptionId: redemptionOptionSelect.val() },
    onSuccess,
    onError
  );
});
```

#### swellAPI.makeRedemption(opts, successCB, errorCB)

This method is used to redeem a customer's points for a discount. You need to pass the redemptionOptionId of the redemption option they wish to redeem. If successful the points will be deducted from the customer's account. You can optionally pass a boolean `delayPointDeduction` set to true to wait until the coupon is used in an order to remove the points.

The redemption object is passed back to the successCB which has information about the discount code (if applicable) that was generated.

#### Options

| Attribute           | Type    | Description                                                                                   |
| ------------------- | ------- | --------------------------------------------------------------------------------------------- |
| redemptionOptionId  | int     | The redemption option id that is being redeemed.                                              |
| delayPointDeduction | boolean | (optional, defauls to false) if true points are not deducted until after the order is placed. |

#### Redemption Object

| Attribute          | Type   | Description                                                                                                                                         |
| ------------------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| couponCode         | string | The coupon code for this discount                                                                                                                   |
| pointsUsed         | int    | The amount of points used for this redemption                                                                                                       |
| appliesToId        | int    | The product, variant, or collection id this discount can be used for                                                                                |
| discountType       | string | A string representing the type of discount this redemption was for. Possible values include: fixed_amount, percentage, variable, shipping, product. |
| discountRateCents  | int    | If converting points to fixed dollar discount this is the conversion rate used in cents per point.                                                  |
| redemptionOptionId | int    | The redemption option that was used to generate this redemption.                                                                                    |
