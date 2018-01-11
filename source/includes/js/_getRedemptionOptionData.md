## Get Redemption Option Data
#### swellAPI.getActiveRedemptionOptions()

This method returns an array containing all active redemption options you setup.  You can use this method to build custom ui around your redemption options.

```javascript
  $(document).on("swell:setup", function(){
    var redemptionOptions = swellAPI.getActiveRedemptionOptions();

    redemptionOptions.forEach(function(option){
      // render each option
    });
  })
```

#### Redemption Option Object

Attribute | Type | Description
--------- | ----------- | -----------
id | int | The redemption option id, useful for setting up click handlers or triggering custom redemptions
name | string | name of this option as set in the admin
icon | string | the font awesome icon class or image url set for this option
description | string | the description of this option
costText | string | the number of points required string set in ad min "200 Points"
costInPoints | int | the number of points required to redeem this option
