## Redemption Options
```html

  <!-- as a link but can easily be any html element -->
  <a class="swell-redemption-link"
     data-redemption-option-id="123"
     href="javascript:void(0)">Redeem 500 Points for $5 Off Coupon</a>

```

#### Redeem points for a discount

You can use this markup to allow your customers to redeem their points for a discount without using the Rewards Popup. Use the class name "swell­-redemption-link" on any element you'd like to trigger one of your redemption options when clicked. You also need to specify data attributes for the "redemption-option-id".

#### Data Attributes

Attribute | Type | Description
--------- | ----------- | -----------
redemption-option-id | int | The id of the redemption option to trigger. You can find it in the Swell Admin by clicking on the Customize link for a particular option.
