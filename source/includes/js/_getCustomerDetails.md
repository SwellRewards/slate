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
created_at                  | datetime| The timestamp this customer entered our database
actionHistoryItems          | array   | The customer's earning and redemption history
email                       | string  | The customer's email address
pointRedemptions            | array   | An array containing the customer's point redemption history
pointsBalance               | int     | The customer's current point balance
pointsEarned                | int     | The total number of points ever earned for this customer
purchases                   | array   | An array containing the customer's purchase history
referralCode                | string  | The customer's referral code (eg. abcd123)
referralLink                | string  | The customer's unique referral link
referralDiscountCode        | string  | The customer's unique referral discount code
referralDiscountCodeId      | int     | An internal id for this customer's referral discount code
referralDiscountCodeOrders  | int     | The number of orders made with this customer's referral discount code
referrals                   | array   | An array containing the customer's referral history
vipTier                     | obj     | An object with details about the customer's current tier level
vipTierStats                | obj     | An object with details about the customer's current progress toward vip tiers.
payoutEmail                 | string  | The customer's paypal email used for receiving cash payouts
totalEarned                 | string  | Total amount earned as part of the affiliate program
payoutPercentage            | string  | The customer's payout percentage for the affiliate program
isAffiliate                 | bool    | Flag for whether or not this customer is an affiliate
affiliateCashPayouts        | array   | An array containing the customer's cash payouts history
affiliateProductsSold       | array   | An array containing all the products this customer has sold as an affiliate

#### Action History Item Object

Attribute | Type      | Description
--------- | --------- | -----------
action    | string    | A short description of either the action/campaign completed or the redemption option earned/redeemed
createdAt | datetime  | A timestamp when this item was created
date      | date      | The date (without time) when this item was created
points    | int       | The number of points that were either earned or redeemed
credits   | string    | If using a variable redemption option, the amount of credit earned or used.
status    | string    | For campaigns this can either be Approved, Pending, Reversed, or a percentage complete. For coupons this will be the coupon code

#### Point Redemption Object

Attribute         | Type    | Description
----------------- | ------- | -----------
amount            | int     | if redemption_option.type is "variable" then this will be the amount of points used
at_checkout       | boolean | a flag for whether or not this redemption deducted the points from the customer's balance before or after making a purchase.  If true then the points are removed after purchase otherwise they are removed upon redemption.
created_at        | date    | The timestamp of when this redemption was made
currency          | string  | The currency of the store
customer_id       | int     | The internal id of the customer who made the redemption
external_url      | string  | If the redemption is fulfilled externally and requires a link for the customer to claim then this can be used to store that url.
id                | int     |  The internal id for this redemption
is_pos            | boolean | A flag for whether or not this redemption happened on a Point of Sale system
perk_id           | int     | If this redemption happened as part of completing a campaign this will hold the id of the perk responsible for rewarding the customer for completing that campaign.
redemption_option | object  | The redemption option that was redeemed by the customer.
reward_text       | string  | The coupon code or other text presented to the customer for making the redemption.
visible           | boolean | Whether or not this should show up in the customer's history.  

#### Redemption Option Object

Attribute     | Type    | Description
------------- | ------- | -----------
amount        | int     | How many points this redemption option costs to redeem
discount_type | string  | The type of discount this redemption option represents.  Can be things like fixed_amount, variable, percentage, free_product.
name          | string  | The name given in the Swell admin for this redemption option (e.g. "$5 Off")

#### Referral Object

Attribute     | Type      | Description
------------- | --------- | -----------
completedAt   | datetime  | When the referred customer made a purchase
customerId    | int       | The internal customer id for the referral
email         | string    | The email address of the referred customer
id            | int       | An internal id for this referral object
signedUpAt    | datetime  | When the referred customer created an account
source        | string    | Where the referred customer came from
sourceHost    | string    | The hostname the referred customer came from

#### VIP Tier Object

Attribute         | Type    | Description
----------------- | ------- | -----------
id                | int     | an internal id for this tier
name              | string  | The name of the tier
description       | string  | A short description of the tier requirements
pointsMultiplier  | decimal | The points multiplier for the current tier


#### VIP Tier Stats Object

Attribute         | Type  | Description
----------------- | ----- | -----------
pointsEarned      | int   | How many points the customer has earned within the tier window
amountSpentCents  | int   | How many cents the customer has spent within the tier window
purchasesMade     | int   | How many purchases the customer has made within the tier window
referralsCompleted| int   | How many referrals the customer has made within the tier window


#### Cash Payout Object

Attribute         | Type    | Description
----------------- | ------- | -----------
id                | int     | an internal id for this tier
customer_id       | int     | The internal customer id for the referral
payout_date       | datetime| The timestamp when the payout is scheduled to happen
amount_earned     | float   | The amount the customer earned for this cash payout
currency          | string  | The currency code for the cash payout (e.g "USD")
status            | string  | The status of this cash payout
email             | string  | The paypal email this cash payout was or will be sent to
purchase_amount   | string  | The amount of the purchase made that triggered this cash payout
date              | datetime| The timestamp of when this cash payout was created


#### Affiliate Product Sold Object

Attribute         | Type    | Description
----------------- | ------- | -----------
title             | string  | The title of the product sold by an affiliate
revenue_generated | int     | The amount of revenue generated in cents by the total sales of this product by this affiliate
number_of_items   | int     | How many of this product has been sold by this affiliate
