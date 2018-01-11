## Show Popups
#### swellAPI.showPopupByType(type)

This method will show any of the designed popups in Swell by type. Use this if you are using the designed popups and want to show them programmatically.  The following types are possible:

```javascript
  $(document).on("swell:setup", function(){
    swellAPI.showPopupByType("RewardsPopup");
  });
```
#### RewardsPopup  ­
The standard rewards popup that customers use to earn points, redeem points, manage referral program, view history, and read faqs. You can configure it in the Rewards Popup section of the Swell Admin.

#### EmailCapturePopup
This popup is used to collect email addresses for your newsletter. You can configure and design it in the Newsletter Signup section of the Swell Admin.

#### ReferralLinkPopup
This popup is used by customers to view and share their referral link via social media and email. You can configure and design it in the Referral Program section of the Swell Admin.

#### HistoryPopup  ­
This popup is used to view all point earning and redemption activity. It will also display any coupon codes earned by the customer.
