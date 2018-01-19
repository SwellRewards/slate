## Referral Program

Use this markup if you want to include some of the referral program components directly on your website. You can setup traditional social share icons that allow your customer to share their referral link on the social media platform of their choice. You can also directly trigger the Referral Share Popup that you designed in the Swell Admin.

### Sharing
```html

  <!-- as links but can easily be any html element -->

  <!-- facebook -->
  <a class="swell-share-referral-facebook" href="javascript:void(0)">
    <i class="fa fa-facebook-square"></i> Share your referral link on Facebook
  </a>

  <!-- facebook messenger -->
  <a class="swell-share-referral-messenger" href="javascript:void(0)">
    <i class="fa fa-envelope"></i> Share your referral link via Facebook Messenger
  </a>

  <!-- twitter -->
  <a class="swell-share-referral-twitter" href="javascript:void(0)">
    <i class="fa fa-twitter-square"></i> Share your referral link on Twitter
  </a>

  <!-- email -->
  <a class="swell-share-referral-email" href="javascript:void(0)">
    <i class="fa fa-envelope"></i> Share your referral link via Email
  </a>


```

Use this markup to directly trigger sharing on one of the social media platforms. They all use a class name with a similar pattern: "swell-share-referral-[platform]"

#### Platform Class Names

Platform | Class
--------- | -----------
facebook | swell-share-referral-facebook
facebook messenger | swell-share-referral-messenger
twitter | swell-share-referral-twitter
email | swell-share-referral-email


### Popup
```html

  <!-- as a link but can easily be any html element -->
  <a class="swell-referral-link-popup" href="javascript:void(0)">Refer Your Friends</a>

```

You can use this markup to allow your customers to refer their friends without using the Rewards Popup. Use the class name "swell­-referral-link-popup" on any element you'd like to trigger the popup when clicked. Make sure you have designed and enabled this popup from the Referral Program section of your Swell Admin.
