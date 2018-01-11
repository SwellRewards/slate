## Get Customer Details

#### swellAPI.getCustomerDetails()

This method returns an object containing all the program details about the logged in customer.  You can use this to create custom ui's that display items like their current point balance, referral link, and earning/redemption history.

```javascript
  $(document).on("swell:setup", function(){
    var customerDetails = swellAPI.getCustomerDetails();

    $("#custom-point-balance-ui").html(customerDetails.pointsBalance);
    $("#custom-referral-link-ui").html(customerDetails.referralLink);

    customerDetails.actionHistoryItems.forEach(function(item){
      $("#custom-history-container").append(
        "<li>" +
          item.date   + " -- " +
          item.action + " -- " +
          item.points +
        "</li>");
    });
  })
```

#### Details Object

Attribute | Type | Description
--------- | ----------- | -----------
email | string | The customer's email address
referralLink | string | The customer's unique referral link
referralCode | string | The customer's referral code (eg. abcd123)
pointsBalance | int | The customer's current point balance
actionHistoryItems | array | The customer's earning and redemption history
vipTier | obj | An object with details about the customer's current tier level
vipTierStats | obj | An object with details about the customer's current progress toward vip tiers.


#### Action History Item Object

Attribute | Type | Description
--------- | ----------- | -----------
createdAt | datetime | A timestamp when this item was created
date | date | The date (without time) when this item was created
action | string | A short description of either the action/campaign completed or the redemption option earned/redeemed
points | int | The number of points that were either earned or redeemed
status | string | For campaigns this can either be Approved, Pending, Reversed, or a percentage complete.  For coupons this will be the coupon code

#### VIP Tier Object

Attribute | Type | Description
--------- | ----------- | -----------
name | string | The name of the tier
description | string | A short description of the tier requirements
pointsMultiplier | decimal | The points multiplier for the current tier


#### VIP Tier Stats Object

Attribute | Type | Description
--------- | ----------- | -----------
pointsEarned | int | How many points the customer has earned within the tier window
amountSpentCents | int | How many cents the customer has spent within the tier window
purchasesMade | int | How many purchases the customer has made within the tier window
