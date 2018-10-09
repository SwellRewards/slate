## Get Customer Details

#### swellAPI.getCustomerDetails()

This method returns an object containing all the program details about the logged in customer.  You can use this to create custom ui's that display items like their current point balance, referral link, and earning/redemption history.

```javascript
  $(document).on("swell:setup", function(){
    var customerDetails = swellAPI.getCustomerDetails();

    $("#custom-point-balance-ui").html(customerDetails.pointsBalance);
    $("#custom-referral-link-ui").html(customerDetails.referralLink);

    customerDetails.actionHistoryItems.forEach(function(item) {
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

Attribute           | Type    | Description
------------------- | ------- | -----------
actionHistoryItems  | array   | The customer's earning and redemption history
email               | string  | The customer's email address
pointRedemptions    | array   | An array containing the customer's point redemption history
pointsBalance       | int     | The customer's current point balance
purchases           | array   | An array containing the customer's purchase history
referralCode        | string  | The customer's referral code (eg. abcd123)
referralLink        | string  | The customer's unique referral link
referrals           | array   | An array containing the customer's referral history
vipTier             | obj     | An object with details about the customer's current tier level
vipTierStats        | obj     | An object with details about the customer's current progress toward vip tiers.


#### Action History Item Object

Attribute | Type      | Description
--------- | --------- | -----------
action    | string    | A short description of either the action/campaign completed or the redemption option earned/redeemed
createdAt | datetime  | A timestamp when this item was created
date      | date      | The date (without time) when this item was created
points    | int       | The number of points that were either earned or redeemed
status    | string    | For campaigns this can either be Approved, Pending, Reversed, or a percentage complete. For coupons this will be the coupon code

#### Point Redemption Object

Attribute         | Type    | Description
----------------- | ------- | -----------
amount            | int     |
approved          | boolean |
approved_at       | date    |
at_checkout       | boolean |
created_at        | date    |
currency          | string  |
customer_id       | int     |
external_url      | string  |
id                | int     |
is_pos            | boolean |
perk_id           | int     |
redemption_option | object  |
referral_id       | int     |
reward_text       | string  |
token             | string  |
user_id           | int     |
value_cents       | int     |
visible           | int     |

#### Redemption Option Object

Attribute     | Type    | Description
------------- | ------- | -----------
amount        | int     |
discount_type | string  |
name          | string  |
type          | string  |

#### Referral Object

Attribute     | Type      | Description
------------- | --------- | -----------
completedAt   | datetime  |
customerId    | int       |
email         | string    |
id            | int       |
merchantId    | int       |
signedUpAt    | datetime  |
source        | string    |
sourceHost    | string    |

#### VIP Tier Object

Attribute         | Type    | Description
----------------- | ------- | -----------
name              | string  | The name of the tier
description       | string  | A short description of the tier requirements
pointsMultiplier  | decimal | The points multiplier for the current tier


#### VIP Tier Stats Object

Attribute         | Type  | Description
----------------- | ----- | -----------
pointsEarned      | int   | How many points the customer has earned within the tier window
amountSpentCents  | int   | How many cents the customer has spent within the tier window
purchasesMade     | int   | How many purchases the customer has made within the tier window
