## Campaigns
```html

<!-- as a link but can easily be any html element -->
  <a class="swell-campaign-link"
     data-campaign-id="123"
     data-display-mode="modal"
     href="javascript:void(0)">Follow Us on Twitter</a>

```

#### Participate in campaigns

You can use this markup to allow your customers to participate in your campaigns without using the Rewards Popup. Use the class name "swell­-campaign­-link" on any element you'd like to trigger one of your campaigns when clicked. You also need to specify data attributes for the "campaign­-id" and the "display­-mode".

#### Data Attributes

Attribute | Type | Description
--------- | ----------- | -----------
campaign-id | int | The id of the campaign to trigger. You can find it in the Swell Admin by clicking on the Customize link for a particular campaign.
display-mode | string | Can be "modal" or "direct". Modal will trigger a popup window that shows campaign details with a call to action. Direct will trigger the action immediately upon click (same effect as clicking the CTA in the modal).
