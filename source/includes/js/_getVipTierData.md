## Get VIP Tier Data
#### swellAPI.getVipTiers()

This method returns an array containing all vip tiers you setup.  You can use this method to build custom ui around your vip tier program.

```javascript
  $(document).on("swell:setup", function(){
    var vipTiers = swellAPI.getVipTiers();

    vipTiers.forEach(function(tier){
      // do something with each tier
    });
  })
```

#### VIP Tier Object

Attribute | Type | Description
--------- | ----------- | -----------
id | int | The vip tier id
name | string | name of this tier as set in the admin
description | string | the description of this tier
requiredAmountSpentCents | int | The amount of cents a customer must spend within window to earn this tier
requiredAmountSpent | string | A version of requiredAmountSpentCents useful for display (e.g. "$100")
requiredPointsEarned | int | The amount of points a customer must earn within window to earn this tier
requiredPurchasesMade | int | The amount of purchases a customer must make within window to earn this tier
pointsMultiplier | decimal | The points multiplier customers earn for reaching this tier
rank | int | This tiers rank compared to all other tiers.  Useful for determining order.
