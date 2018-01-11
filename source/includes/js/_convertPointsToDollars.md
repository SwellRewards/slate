## Convert Points to Dollars
#### swellAPI.convertPointsToDiscount(opts, successCB, errorCB)
```javascript

  // let customer enter how many to redeem
  $(document).on("swell:setup", function(){
    var amountToRedeem = $("#redemption-amount-input").val();

    var onSuccess = function(redemption){ fillAndSubmitCouponCodeForm(redemption.couponCode); }
    var onError = function(err) { $("#error").show(); }

    swellAPI.convertPointsToDiscount({amount: amountToRedeem}, onSuccess, onError);
  });

  // redeem all of their points
  // in reality you'd likely want to make sure you aren't redeeming a larger discount
  // than the current cart total as to not waste points
  $(document).on("swell:setup", function(){
    var details = swellAPI.getCustomerDetails();

    var onSuccess = function(redemption){ fillAndSubmitCouponCodeForm(redemption.couponCode); }
    var onError = function(err) { $("#error").show(); }

    swellAPI.convertPointsToDiscount({amount: details.pointsBalance}, onSuccess, onError);
  });
```

This method is used to redeem a customers points for a fixed discount at some predetermined conversion rate set in your Swell admin.  For example if you setup a redemption rate of 5 cents per point then you could redeem 20 points for a $1 discount code.  If successful the points will be deducted from the customer's account.  You can optionally pass a boolean `delayPointDeduction` set to true to wait until the coupon is used in an order to remove the points.

#### Options

Attribute | Type | Description
--------- | ----------- | -----------
amount | int | The number of points to redeem
delayPointDeduction | boolean | (optional, defauls to false) if true points are not deducted until after the order is placed.

#### Redemption Object

Attribute | Type | Description
--------- | ----------- | -----------
couponCode | string | The coupon code for this discount
pointsUsed | int | The amount of points used for this redemption
appliesToId | int | The product, variant, or collection id this discount can be used for
discountType | string |  A string representing the type of discount this redemption was for.  Possible values include: fixed_amount, percentage, variable, shipping, product.
discountRateCents | int | If converting points to fixed dollar discount this is the conversion rate used in cents per point.
redemptionOptionId | int | The redemption option that was used to generate this redemption.
