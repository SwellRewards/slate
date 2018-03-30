## Events

```javascript
$(document).on("swell:event:name:here", eventHandler);
```

All Swell related events are triggered on the document object. Please see example on the right for an example of how to listen for these events using jQuery.

### swell:initialized

```javascript
$(document).on("swell:initialized", function() {
  // do something using my campaigns or redemption options
});
```

This event is triggered after page load when the Swell javascript is finished loading. This is useful if you want to know when you can safely read campaign and redemption option data from the swellAPI object. Note: This is triggered before loading the logged in customers data from Swell. Please use the Setup event if you are interested in the customers data.

### swell:setup

```javascript
$(document).on("swell:setup", function() {
  // do something using logged in customer (campaigns and redemption options available too)
});
```

This event is triggered after Swell has completely finished setup. The main difference between setup and initialized is that setup waits until the logged in customer's details have been loaded from Swell servers before firing.

### swell:redemption:created

```javascript
$(document).on("swell:redemption:created", function(redemption) {
  if (redemption.discountType === "product") {
    // add the free product to cart
    cartJS.add(redemption.appliesToId);
  } else {
    // control the display of the coupon code to your customers
    myCoolUI.showCouponCode(redemption.couponCode);

    // save the code for later (to display it on future page loads?)
    sessionStorage.setItem("most-recent-coupon", redemption.couponCode);
  }
});
```

This event is triggered when a redemption is made by a customer (or programatically via SDK). This is often helpful if you are implementing a custom redemption UX. The event is passed a single object representing the redemption made. It will include the following attributes:

### swell:referred:customer:discount

```javascript
function handleReferredCustomerCouponCode(couponCode) {
  // do whatever you want with the coupon code
  // show in popup, tab, add to checkout, etc
}

// handle the generation of the coupon code
$(document).on("swell:referred:customer:discount", function(couponCode) {
  // one common strategy is to save it in localStorage so that it's available on every page load if you need to display or use it.
  localStorage.setItem("swell-referred-customer-coupon-code", couponCode);
  handleReferredCustomerCouponCode(couponCode);
});

// on page load, use previously saved coupon code if one exists
$(document).ready(function() {
  var swellReferredCustomerCouponCode = localStorage.getItem(
    "swell-referred-customer-coupon-code"
  );
  if (swellReferredCustomerCouponCode) {
    handleReferredCustomerCouponCode(swellReferredCustomerCouponCode);
  }
});
```

This event is triggered when an eligible referred customer clicks a referral link. This is often helpful if you are implementing a custom referred customer user experience. The event is passed the coupon code the customer earned.

#### Redemption Attributes

| Attribute          | Type   | Description                                                                                                                                         |
| ------------------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| couponCode         | string | The coupon code for this discount                                                                                                                   |
| pointsUsed         | int    | The amount of points used for this redemption                                                                                                       |
| appliesToId        | int    | The product, variant, or collection id this discount can be used for                                                                                |
| discountType       | string | A string representing the type of discount this redemption was for. Possible values include: fixed_amount, percentage, variable, shipping, product. |
| discountRateCents  | int    | If converting points to fixed dollar discount this is the conversion rate used in cents per point.                                                  |
| redemptionOptionId | int    | The redemption option that was used to generate this redemption.                                                                                    |
