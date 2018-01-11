## Refresh Customer Details
```javascript
  $(document).on("swell:setup", function(){

    swellAPI.refreshCustomerDetails(function(){
      var customerDetails = swellAPI.getCustomerDetails();
    });

  })
```
#### swellAPI.refreshCustomerDetails(callback)
This method loads the latest customer details from the server.  You usually won't have to use this as the Swell JS automatically refreshes the customer details.  The main use case for this is if you trigger a custom action or point redemption and want to update customer related ui.
