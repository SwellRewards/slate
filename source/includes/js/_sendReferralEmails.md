## Send Referral Emails

### swellAPI.identifyReferrer()
```javascript

$(document).on("swell:setup", function() {
  var customersInput = $("#customers-input");
  var sendEmailBtn = $("#customers-send-btn");

  $(sendEmailBtn).click(function() {
    onSuccess = function() {
      $("#success").show();
    }

    onError = function() {
      $("#error").show();
    }

    swellAPI.identifyReferrer(email, onSuccess, onError);
  });
})

```

This method takes an email address as a string to identify the email to attribute referred emails to. This is useful if you are building a custom referral program experience in which logged out customers can still participate.

#### Parameters

Parameter | Type | Description
----------|------|------------
email | string | The email address of the customer who will be referring
onSuccess | function | The function to be called if the email is sent successfully
onError | function | The function to be called if the email is sent unsuccessfully


### swellAPI.sendReferralEmails()
```javascript

  $(document).on("swell:setup", function() {
    var referredCustomersInput = $("#referred-customers-input");
    var sendEmailsBtn = $("#referred-customers-send-btn");

    $(sendEmailsBtn).click(function() {
      // assuming you allowed comma separated email input
      var emails = referredCustomersInput.val().split(",");

      var onSuccess = function() {
        $("#success").show();
      }

      var onError = function(err) {
        $("#error").show();
      }

      swellAPI.sendReferralEmails(emails,onSuccess, onError);

    });
  });

```

This method takes an array of email addresses to send the referral email share email to.  This is useful if you are building a custom referral program experience and want your customer to be able to share his referral link with his or her friends via email.

#### Parameters

Parameter | Type | Description
----------|------|------------
emails | array | An array containing the email addresses of those referred
onSuccess | function | The function to be called if the email is sent successfully
onError | function | The function to be called if the email is sent unsuccessfully
