## Customer Details

Use any of the below to display program data about the logged in customer. Make sure to include these elements only if there exists a logged in customer otherwise they will not populate.

### Point Balance
```html

  <!-- the content of the tag will be replaced by the actual point balance on page load.
       Decide what you'd like the customer to see while it loads -->


  <!-- show 0 until point balance is loaded -->
  <span class="swell-point-balance">0</span>


  <!-- show nothing until point balance is loaded -->
  <span class="swell-point-balance"></span>


  <!-- show font awesome spinner until point balance is loaded -->
  <span class="swell-point-balance"><i class="fa fa-spin fa-spinner"></i></span>

```

#### Display customer's current point balance

You can use this markup to display the customer's current point balance. Great for reminding the customer how many points they have to use or how close they are to earning a reward. Use the class name "swell-point-balance" on the element you would like to display the point balance inside of.

### Referral Link
```html

  <!-- the content of the tag will be replaced by the actual referral link on page load.
       Decide what you'd like the customer to see while it loads -->


  <!-- show nothing until referral link is loaded -->
  <span class="swell-referral-link"></span>


  <!-- show font awesome spinner until referral link is loaded -->
  <span class="swell-referral-link"><i class="fa fa-spin fa-spinner"></i></span>

```

#### Display customer's current referral link

You can use this markup to display the customer's current referral link. Use the class name "swell-referral-link" on the element you would like to display the referral link inside of.


### Earning History
```html

  <!-- as a link but can easily be any html element -->
  <a class="swell-history-link" href="javascript:void(0)">View Rewards History</a>

```

#### Display customer's rewards history

You can use this markup to display the customer's rewards history. This will include all the times a customer has earned points, earned a discount, or redeemed points for a discount. Use the class name "swell-history-link" on the element you would like to trigger the rewards history modal.
