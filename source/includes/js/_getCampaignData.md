## Get Campaign Data
#### swellAPI.getActiveCampaigns()

This method returns an array containing all active campaigns you are running as well as the logged in customer's status toward each one.  You can use this method to build custom ui around your campaigns.

```javascript
  $(document).on("swell:setup", function(){
    var activeCampaigns = swellAPI.getActiveCampaigns();

    activeCampaigns.forEach(function(campaign){
      // render each campaign
    });
  })
```

#### Campaign Object

Attribute | Type | Description
--------- | ----------- | -----------
id | int | The campaign id, useful for setting up click handlers or triggering custom actions
ctaText | string | ­the call to action button text set in swell admin
details | string | the description of the campaign set in the swell admin
title  ­ | string | the one line description of the campaign set in swell admin
expiresAt | datetime |  campaign expiration if campaign is running for limited time
icon | string |  the font awesome icon class (or image url) set for this campaign
rewardText | string | description of the reward the customer will receive for completion
customerActionsCompleted | int | number of actions completed (if applicable)
customerCanParticipate  | boolean | based on the rules, can this customer participate right now
customerCompleted | boolean | flag whether or not they have completed the campaign
customerCompletions | int | number of times they've completed the campaign
customerMinutesUntilNextParticipation | int | number of minutes until customer can participate in the campaign again
customerHoursUntilNextParticipation | int | number of hours until customer can participate in the campaign again
customerDaysUntilNextParticipation | int | number of days until customer can participate in the campaign again
