## Capture Newsletter Signup
```javascript

  // probably not the best example but simple way to accomplish capture
  $(document).on("swell:setup", function(){
    var existingEmailField = $("#my-esp-email-field");
    var existingSubmitBtn = $("#my-esp-submit-btn");

    $(existingSubmitBtn).click(function(){
      var email = existingEmailField.val();
      swellAPI.captureNewsletterSignup(email);
    });
  });

  // ideally your ESP would emit an event when capture was successful
  $(document).on("swell:setup", function(){
    $(document).on("esp:captured:email", function(email){
      swellAPI.captureNewsletterSignup(email);
    });
  })
```
#### swellAPI.captureNewsletterSignup(email, successCB, errorCB)

This method takes an email address to capture and will record it as having participated in the newsletter signup campaign.  This is useful if you want to use an existing newsletter signup form but still record the capture in Swell.
