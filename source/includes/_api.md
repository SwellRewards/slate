# API

## Introduction

Swell provides a simple and powerful REST API to integrate rewards into your business or application.

This API reference provides information on available endpoints and how to interact with them. If you have questions or need additional help feel free to reach out to our engineering team at <a href="mailto:eng@swellrewards.com">eng@swellrewards.com</a>.

## Authentication

Swell uses a combination of a <code>merchant_id</code> and <code>api_key</code> to authenticate requests.  You can find this information in the Settings page of your Swell account.  Swell expects for both <code>merchant_id</code> and <code>api_key</code> to be included in all API requests and can be passed as request parameters or as HTTP headers.  



## Actions

### Record a customer action

```ruby
require 'faraday'

action = Hash.new
action[:type] = "CustomAction"
action[:customer_email] = "john@example.com"
action[:action_name] = "completed_survey"

# add authentication params (could also use headers)
action[:merchant_id] = 12345
action[:api_key] = "MY_API_KEY"

conn = Faraday.new(:url => "https://app.swellrewards.com")

response = conn.post do |req|
  req.url "/api/v2/actions"
  req.headers['Content-Type'] = 'application/json'
  req.body = action.to_json
end

```

This endpoint records an action performed by a customer.  It will apply the action to all matching active campaigns and award the necessary points and/or discounts.

#### HTTP Request

`POST https://app.swellrewards.com/api/v2/actions`

#### Query Parameters

Parameters | Type | Description
--------- | ----------- | -----------
type | string | The type of action.  Almost always set this to <code>"CustomAction"</code>
customer_email | string | The email address used to identify the customer who performed the action.
action_name | string | The name of the action that was performed.  This must match the name set for one of your Custom Action Campaigns.
ip_address | string (optional) | The ip address of the user when the action was performed.  This is required if this action completes a referral program requirement.
user_agent | string (optional) | The browser user agent of the user when the action was performed.  This is strongly recommended if this action completes a referral program requirements.
created_at | datetime (optional) | The time the action was performed.  This will default to the current time.  Use this if you need to back date an action.
reward_points | int (optional) | Use this to override the normal amount of points awarded for performing the action.  Will default to the rules of the campaign otherwise.
history_title | string (optional) | Use this to override the normal description shown to the user in their history.  Will default to the campaign settings otherwise.

## Customers


### Create and update customer records

```ruby
require 'faraday'

customer = Hash.new
customer[:id] = "CUS_1231H9UQOW9H23"
customer[:email] = "john@example.com"
customer[:first_name] = "John"
customer[:last_name] = "Smith"

# add authentication params (could also use headers)
customer[:merchant_id] = 12345
customer[:api_key] = "MY_API_KEY"

conn = Faraday.new(:url => "https://app.swellrewards.com")

response = conn.post do |req|
  req.url "/api/v2/customers"
  req.headers['Content-Type'] = 'application/json'
  req.body = customer.to_json
end

```

This endpoint both creates and updates a customer's record in our system.  Use this primarily to notify us when you add a customer manually in your admin or a customer changes their email address.


#### HTTP Request

`POST https://app.swellrewards.com/api/v2/customers`

#### Query Parameters

Parameters | Type | Description
--------- | ----------- | -----------
id | string | The identifier used to uniquely identify the customer in your system.
email | string |
first_name | string |
last_name | string |



### Fetch customer details

```ruby
require 'faraday'

customer_data = Hash.new

customer_data[:customer_email] = "john@example.com"

# add authentication params (could also use headers)
customer_data[:merchant_id] = 12345
customer_data[:api_key] = "MY_API_KEY"

connection = Faraday.new "https://app.swellrewards.com" do |conn|
  conn.response :json, :content_type => 'application/json'
end

customer = connection.get "/api/v2/customers", customer_data

puts customer.body["points_balance"]
puts customer.body["points_earned"]
puts customer.body["referral_code"]["code"]

```

This endpoint returns a Swell customer record.  Most commonly used to fetch a customer's point balance and unique referral link.


#### HTTP Request

`GET https://app.swellrewards.com/api/v2/customers?customer_email=john@example.com`

#### Query Parameters

Parameters | Type | Description
--------- | ----------- | -----------
customer_id | string | The identifier used to uniquely identify the customer in your system. Required if no customer_email is passed.
customer_email | string | The customer's email address.  Required if no customer_id is passed.
with_referral_code | boolean | Defaults to false.  If you'd like information about the customers referral code you can set this to true.
with_history | boolean | Defaults to false.  If you'd like information about the customers point earning and redemption history you can set this to true.

> The above returns JSON structured like this:

```json
{
  "thirty_party_id": "CUS_1231H9UQOW9H23",
  "first_name": "John",
  "last_name": "Smith",
  "email": "john@example.com",
  "points_balance": 455,
  "points_earned": 850,
  "total_spend_cents": 251200,
  "total_purchases": 8,
  "perks_redeemed": 3,
  "last_purchase_at": "2016-09-10T13:44:15.000Z",
  "points_expire_at": "2019-12-15T18:41:13.000Z",
  "last_seen_at": "2016-09-10T18:41:13.000Z",

  "referral_code": {
    "code": "ABC123",
    "total_clicks": 6123,
    "shares": 1231,
    "facebook_shares": 200,
    "twitter_shares": 150,
    "email_shares": 180,
    "emails_sent": 400,
    "unique_clicks": 4124,
    "links_clicked_from_email": 123,
    "links_clicked_from_twitter": 1231,
    "links_clicked_from_facebook": 332,
    "orders": 21,
    "amount_cents": 32423,
    "average_amount_cents": 4350,
    "expires_at": null,
    "expired": false,
    "completed_referral_customers": [
      {
        "email": "sally@example.com",
        "first_name": "Sally",
        "last_name": "Jane"
      }
    ]
  },

  "history_items": [
    {
      "created_at": "2016-06-28T18:41:13.000Z",
      "date": "2016-06-28",
      "action": "Create an Account",
      "points": 200,
      "status": "Approved"
    },
    {
      "created_at": "2016-08-28T18:41:13.000Z",
      "date": "2016-06-28",
      "action": "Complete our Survey",
      "points": 150,
      "status": "Approved"
    },
    {
      "created_at": "2016-08-28T18:41:13.000Z",
      "date": "2016-06-28",
      "action": "$5 Coupon",
      "points": -250,
      "status": "FIVEOFF-XYZ"
    }
  ]
}

```

## Point Redemptions

### Create a redemption

```ruby
require 'faraday'

redemption = Hash.new
redemption[:customer_external_id] = "CUS_1231H9UQOW9H23"
redemption[:customer_email] = "john@example.com"
redemption[:redemption_option_id] = 12345

# add authentication params (could also use headers)
redemption[:merchant_id] = 12345
redemption[:api_key] = "MY_API_KEY"

conn = Faraday.new(:url => "https://app.swellrewards.com")

response = conn.post do |req|
  req.url "/api/v2/redemptions"
  req.headers['Content-Type'] = 'application/json'
  req.body = redemption.to_json
end

```

This endpoint will redeem a customer's points for a particular redemption option.  It will check to ensure the customer is eligible and has enough points for the selected redemption option and then it will deduct the points from their balance, generate the coupon code, and return it in the response.


#### HTTP Request

`POST https://app.swellrewards.com/api/v2/redemptions`

#### Query Parameters

Parameters | Type | Description
--------- | ----------- | -----------
customer_external_id | string | The identifier used to uniquely identify the customer in your system.
customer_email | string | The email address of the customer redeeming points.
redemption_option_id | int | The unique identifier of the redemption option being redeemed.

 > The above returns JSON structured like this:

```json
{
  "id": 12345,
  "reward_text": "FIVEOFF-ABC123",
  "approved": true,
  "approved_at": "2016-08-14T18:41:13.000Z",
  "created_at": "2016-08-14T18:41:13.000Z",
  "updated_at": "2015-08-14T18:41:13.000Z"
}

```


## Campaigns

### Get active campaigns

```ruby
require 'faraday'

params = Hash.new

# add authentication params (could also use headers)
params[:merchant_id] = 12345
params[:api_key] = "MY_API_KEY"

connection = Faraday.new "https://app.swellrewards.com" do |conn|
  conn.response :json, :content_type => 'application/json'
end

campaigns = connection.get "/api/v2/campaigns", params

```

This endpoint returns a list of campaigns available for customers to participate in.  If you provide a particular customer we can return their current status and eligibility on each of the campaigns.

#### HTTP Request

`GET https://app.swellrewards.com/api/v2/campaigns`

#### Query Parameters

Parameters | Type | Description
--------- | ----------- | -----------
with_status | boolean (optional) | Defaults to false.  Enable to get a particular customer's status for each campaign. Either customer_id or customer_email are required for this to work.
customer_id | string (optional) | The id used to identify the customer.
customer_email | string (optional) | The email address used to identify the customer.


> The above returns JSON structured like this:

```json
[{
  "id": 12345,
  "title": "Tweet About Us",
  "details": "Tweet about us and earn 100 points.",
  "type": "TwitterTweetCampaign",
  "cta_text": "Send Tweet",
  "share_text": "SwellRewards is the coolest company, we love them.",
  "url": "https://app.swellrewards.com",
  "username": null,
  "entity_id": null,
  "icon": "fa-twitter",
  "reward_text": "100 Points",
  "expires_at": null,
  "created_at": "2016-08-14T18:41:13.000Z",
  "updated_at": "2015-08-14T18:41:13.000Z",
  "max_completions_per_user": 1,
  "min_actions_required": 1,
  "status": {
    "global_completions_left": 10000,
    "customer_actions_completed": 0,
    "customer_times_completed": 0,
    "customer_minutes_until_next_action": 0,
    "customer_minutes_until_next_completion": 0
  }
},
{
  "id": 12346,
  "title": "Follow Us On Instagram",
  "details": "Follow us on instagram and earn 250 points.",
  "type": "InstagramFollowCampaign",
  "cta_text": "Follow @SwellRewards",
  "share_text": null,
  "url": null,
  "username": "SwellRewards",
  "entity_id": null,
  "icon": "fa-instagram",
  "reward_text": "250 Points",
  "expires_at": null,
  "created_at": "2016-08-14T18:41:13.000Z",
  "updated_at": "2015-08-14T18:41:13.000Z",
  "max_completions_per_user": 1,
  "min_actions_required": 1,
  "status": {
    "global_completions_left": 10000,
    "customer_actions_completed": 1,
    "customer_times_completed": 1,
    "customer_minutes_until_next_action": 0,
    "customer_minutes_until_next_completion": 0
  }
}]

```

#### Response Campaign Parameters

Parameters | Type | Description
--------- | ----------- | -----------
id | int | unique campaign id.
title | string | campaign headline, 1-liner.
details | string | information on how to complete the campaign.
type | string | One of the various campaign types, such as SpendBasedCampaign, TransactionBasedCampaign, ReferralCampaign, TwitterRetweetCampaign, InstagramFollowCampaign, TwitterTweetCampaign, TwitterFollowCampaign, etc.
cta_text | string | the text found on the button to participate in the campaign (e.g. "Tweet", "Follow", etc).
share_text | string | message to be shared on social media, used in the TwitterTweetCampaign.
url | string | campaign dependent. sometimes a destination to link to from cta button, for TwitterTweet it's a url to be included in the tweet.
username | string | campaign dependent. For TwitterFollowCampaign it's the twitter handle to follow.
entity_id | string | campaign dependent. For TwitterRetweetCampaign it's the tweet id to retweet.
icon | string| image used for campaign. if it's a url then it's an image otherwise it's a font awesome class name.
reward_text | string | describes what the customer gets for completing (e.g. "500 Points").
expires_at | datetime | for limited time promotions, this is when the campaign ends.
created_at | datetime |
updated_at | datetime |
max_completions_per_user | int | The maximum number of times a single customer can complete this campaign.
min_actions_required | int | The number of actions (for punch card style campaigns) that are required for the campaign to be completed.


#### Response Status Parameters

Parameters | Type | Description
--------- | ----------- | -----------
global_completions_left | int | Number of times this campaign can be completed globally (for all customers in total).
customer_actions_completed | int | Number of actions the customer has performed towards this campaign.
customer_times_completed | int | Number of times the customer has already completed this campaign.
customer_minutes_until_next_action | int | Number of minutes the customer must wait until they can complete take another action towards completing this campaign.
customer_minutes_until_next_completion | int | Number of minutes the customer must wait until they are eligible to complete this campaign again.



## Redemption Options

### Get active redemption options

```ruby
require 'faraday'

params = Hash.new

# add authentication params (could also use headers)
params[:merchant_id] = 12345
params[:api_key] = "MY_API_KEY"

connection = Faraday.new "https://app.swellrewards.com" do |conn|
  conn.response :json, :content_type => 'application/json'
end

redemption_options = connection.get "/api/v2/redemption_options", params

```

This endpoint returns a list of redemption options available for customers to redeem.

#### HTTP Request

`GET https://app.swellrewards.com/api/v2/redemption_options`

#### Query Parameters

Parameters | Type | Description
--------- | ----------- | -----------
none required


> The above returns JSON structured like this:

```json
[{
  "id": 12345,
  "name": "$5.00 Off",
  "description": "Get $5 off every item in our store for 500 points.",
  "icon": "fa-dollar",
  "cost_text": "500 Points"
},
{
  "id": 12346,
  "name": "$10.00 Off",
  "description": "Get $10 off every item in our store for 1000 points.",
  "icon": "fa-dollar",
  "cost_text": "1000 Points"
}]

```

#### Response Redemption Option Parameters

Parameters | Type | Description
--------- | ----------- | -----------
id | int | unique identifier
name | string | option title (e.g. $5.00 Off)
description | string | short description (e.g. Get $5 Off every item in our store for 500 Points)
icon | string | image if url, otherwise font awesome class name
cost_text | string | how many points this option costs (e.g. 500 Points)


## Orders

### Create an order

```ruby
require 'faraday'

order = Hash.new
order[:status] = "paid"
order[:customer_email] = "john@example.com"
order[:total_amount_cents] = 150188
order[:total_refunded_cents] = 0
order[:currency_code] = "USD"
order[:order_id] = "ORD_43H9ADHW1308123"
order[:customer_id] = "CUS_1231H9UQOW9H23"
order[:coupon_code] = "SAVE5"
order[:ip_address] = "127.0.0.1"
order[:user_agent] = "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36"
order[:shipping_amount_cents] = 538
order[:discount_amount_cents] = 500
order[:tax_amount_cents] = 350

order[:billing_address] = Hash.new
order[:billing_address][:first_name] = "John"
order[:billing_address][:last_name] = "Smith"
order[:billing_address][:country_code] = "US"
order[:billing_address][:address1] = "129 Newbury St"
order[:billing_address][:address2] = "Swell Rewards"
order[:billing_address][:city] = "Boston"
order[:billing_address][:state] = "MA"
order[:billing_address][:zip] = "02215"
order[:billing_address][:phone] = "18442924097"

order[:shipping_address] = Hash.new
order[:shipping_address][:first_name] = "John"
order[:shipping_address][:last_name] = "Smith"
order[:shipping_address][:country_code] = "US"
order[:shipping_address][:address1] = "129 Newbury St"
order[:shipping_address][:address2] = "Swell Rewards"
order[:shipping_address][:city] = "Boston"
order[:shipping_address][:state] = "MA"
order[:shipping_address][:zip] = "02215"
order[:shipping_address][:phone] = "18442924097"

item = Hash.new
item[:id] = "PRO_FHR93H9H912GG"
item[:name] = "iPhone 7 128GB"
item[:quantity] = 2
item[:price_cents] = 74900

order[:items] = [item]


# add authentication params (could also use headers)
order[:merchant_id] = 12345
order[:api_key] = "MY_API_KEY"

conn = Faraday.new(:url => "https://app.swellrewards.com")

response = conn.post do |req|
  req.url "/api/v2/orders"
  req.headers['Content-Type'] = 'application/json'
  req.body = order.to_json
end

```

This endpoint records an order made by a customer.  It will apply the order to all matching active campaigns and award the necessary points and/or discounts.  


#### HTTP Request

`POST https://app.swellrewards.com/api/v2/orders`

#### Query Parameters

Parameters | Type | Description
--------- | ----------- | -----------
customer_email | string | The email address of the customer who placed the order.
total_amount_cents | int | The total amount the customer spent in cents.
currency_code | string | The currency used (e.g. 'USD').
order_id | string | An identifier that uniquely identifies this order in your system.
customer_id | string | An identifier that uniquely identifies the customer who made this order in your system.
status | string (optional) | The current order status.  Will be used to determine when to apply to campaigns.
created_at | datetime (optional) | A timestamp describing when this order was made.
customer_tags | string (optional) | A comma separated list of tags assigned to the customer.
coupon_code | string (optional) | The coupon code used on this order.
ip_address | string (optional) | The ip address of the customer when the order was placed.  This is required for tracking referrals.
user_agent | string (optional) | The browser user agent of the customer when the order was placed.  This is strongly recommended for tracking referrals.
total_refunded_cents | int (optional) | The total amount refunded in cents.
shipping_amount_cents | int (optional) | The total amount the customer spent on shipping in cents.
discount_amount_cents | int (optional) | The total amount the customer saved with discounts in cents.
tax_amount_cents | int (optional) | The total amount the customer spent on taxes.
items | array | An array of items purchased.  Please see below for the required structure for each item.
billing_address | object (optional) | An address object representing the billing address used for the order.
shipping_address | object (optional) | An address object representing the shipping address used for the order.


#### Item Parameters

Parameters | Type | Description
--------- | ----------- | -----------
id | string | An identifier that uniquely identifies this product in your system.
name | string | A customer facing name for the product.
quantity | int | The quantity of this product purchased by the customer in this order.
price_cents | int | The total price of this item (per quantity) in cents.
collections | string (optional) | An comma separated list of all the collections (or tags) that this product belongs to.
type | string (optional) | A category or product type, used in filtering eligibility.
vendor | string (optional) | The vendor of the product.

#### Address Parameters

Parameters | Type | Description
--------- | ----------- | -----------
country_code | string | The two character country code (e.g. "US").
first_name | string |
last_name | string |
address1 | string |
address2 | string |
city | string |
state | string |
zip | string |
phone | string |




## Refunds


### Create a refund

```ruby
require 'faraday'

refund = Hash.new
refund[:order_id] = "ORD_43H9ADHW1308123"
refund[:total_amount_cents] = 250

# add authentication params (could also use headers)
refund[:merchant_id] = 12345
refund[:api_key] = "MY_API_KEY"

conn = Faraday.new(:url => "https://app.swellrewards.com")

response = conn.post do |req|
  req.url "/api/v2/refunds"
  req.headers['Content-Type'] = 'application/json'
  req.body = refund.to_json
end

```

This endpoint records an refund made by you for an existing order.  It will apply the refund to order and then adjust any reward given by the original order.


#### HTTP Request

`POST https://app.swellrewards.com/api/v2/refunds`

#### Query Parameters

Parameters | Type | Description
--------- | ----------- | -----------
order_id | string | The identifier used to uniquely identify the order in your system.  It must match the order_id used when the order was created with the /orders endpoint.
total_amount_cents | int | The total amount the customer was refunded in cents.



## Events

### Introduction

Swell can publish events to a webhook url or directly to one of our partner integrations.  These events can be used to trigger transactional emails, supplement customer data in your crm, or pretty much anything else you could imagine.



### Points Earned

```ruby

Example Event JSON Structure:

{{
    "topic": "swell/points/earned",
    "name": "Swell Points Earned",
    "email": "john.smith@gmail.com",
    "customer": {
        "total_spend_cents": 0,
        "total_purchases": 0,
        "perks_redeemed": 0,
        "last_purchase_at": null,
        "email": "john.smith@gmail.com",
        "points_earned": 200,
        "points_balance": 200,
        "points_expire_at": null,
        "first_name": "John",
        "last_name": "Smith",
        "last_seen_at": "2016-10-15T15:29:27.000Z",
        "thirty_party_id": "4302977345",
        "referral_code": {
            "code": "117yu4g",
            "shares": 0,
            "facebook_shares": 0,
            "twitter_shares": 0,
            "email_shares": 0,
            "emails_sent": 0,
            "emails_viewed": 0,
            "links_clicked_from_email": 0,
            "links_clicked_from_twitter": 0,
            "links_clicked_from_facebook": 0,
            "orders": 0,
            "amount_cents": 0,
            "average_amount_cents": 0,
            "expires_at": null,
            "expired": false,
            "completed_referral_customers": [],
            "email": "john.smith@gmail.com",
            "unique_clicks": 0,
            "total_clicks": 0
        }
    },
    "perk": {
        "id": 1,
        "campaign_id": 1,
        "merchant_id": 1,
        "customer_id": 139,
        "reward_points": 200,
        "completed": true,
        "completed_at": "2016-10-15T15:29:27.911Z",
        "awarded": true,
        "awarded_at": "2016-10-15T15:29:27.911Z",
        "pending": false,
        "reversed": false,
        "reversed_at": null,
        "expired": false,
        "expired_at": null,
        "expires_at": null,
        "redemption_option_id": null,
        "history_title": "Create an account",
        "created_at": "2016-10-15T15:29:27.828Z"
    }
}

```

This event is triggered any time a customer earns points.  This event is useful for keeping a customers point balance up to date in your CRM or email marketing platform.  


Important Attributes | Description
--------- | -----------
email | The email address of the customer who earned points.
customer.points_balance | The customers updated point balance.
perk.reward_points | The amount of points they just earned.
perk.history_title | A short description of the action that lead them to earn these points.


### Coupon Earned (Redeemed)

```ruby

Example Event JSON Structure:

{
    "topic": "swell/redemption/created",
    "name": "Swell Redemption Created",
    "redemption": {
        "id": 1,
        "created_at": "2016-10-15T15:56:01.692Z",
        "updated_at": "2016-10-15T15:56:01.692Z",
        "reward_text": "FIVEOFF",
        "approved": true,
        "approved_at": "2016-10-15T15:56:01.734Z",
        "is_admin": false,
        "is_pos": false,
        "at_checkout": false
    },
    "redemption_option": {
        "id": 149,
        "name": "$5.00 Off",
        "description": "Get $5.00 off your next purchase for 500 points",
        "icon": "fa-dollar",
        "cost_text": "500 Points",
        "amount": 500
    },
    "email": "john.smith@gmail.com",
    "customer": {
        "total_spend_cents": 0,
        "total_purchases": 0,
        "perks_redeemed": 2,
        "last_purchase_at": null,
        "email": "john.smith@gmail.com",
        "points_earned": 5200,
        "points_balance": 4700,
        "points_expire_at": null,
        "first_name": "John",
        "last_name": "Smith",
        "last_seen_at": "2016-10-15T15:54:43.000Z",
        "thirty_party_id": "4302977345",
        "referral_code": {
            "code": "117yu4g",
            "shares": 0,
            "facebook_shares": 0,
            "twitter_shares": 0,
            "email_shares": 0,
            "emails_sent": 0,
            "emails_viewed": 0,
            "links_clicked_from_email": 0,
            "links_clicked_from_twitter": 0,
            "links_clicked_from_facebook": 0,
            "orders": 0,
            "amount_cents": 0,
            "average_amount_cents": 0,
            "expires_at": null,
            "expired": false,
            "completed_referral_customers": [],
            "email": "john.smith@gmail.com",
            "unique_clicks": 0,
            "total_clicks": 0
        }
    }
}

```

This event is triggered any time a customer redeems points for a discount.  This event is useful for sending an email with the coupon code to the customer.


Important Attributes | Description
--------- | -----------
email | The email address of the customer who earned points.
redemption.reward_text | The coupon code earned or redeemed.
redemption_option.amount | The amount of points redeemed.
redemption_option.name | Short description of what the coupon is for.



### Coupon Earned (Awarded)

```ruby

Example Event JSON Structure:

{
    "topic": "swell/redemption/created",
    "name": "Swell Redemption Created",
    "redemption": {
        "id": 2,
        "created_at": "2016-10-15T16:00:17.786Z",
        "updated_at": "2016-10-15T16:00:17.786Z",
        "reward_text": "TENPERCENT",
        "approved": true,
        "approved_at": "2016-10-15T16:00:17.915Z",
        "is_admin": false,
        "is_pos": false,
        "at_checkout": false
    },
    "redemption_option": {
        "id": 152,
        "name": "10% Off",
        "description": "Get 10% off your next purchase for 0 points",
        "icon": "fa-percent",
        "cost_text": "0 Points",
        "amount": 0
    },
    "email": "john.smith@gmail.com",
    "customer": {
        "total_spend_cents": 0,
        "total_purchases": 0,
        "perks_redeemed": 2,
        "last_purchase_at": null,
        "email": "john.smith@gmail.com",
        "points_earned": 5200,
        "points_balance": 4700,
        "points_expire_at": null,
        "first_name": "John",
        "last_name": "Smith",
        "last_seen_at": "2016-10-15T16:00:17.000Z",
        "thirty_party_id": "4302977345",
        "referral_code": {
            "code": "117yu4g",
            "shares": 0,
            "facebook_shares": 0,
            "twitter_shares": 0,
            "email_shares": 0,
            "emails_sent": 0,
            "emails_viewed": 0,
            "links_clicked_from_email": 0,
            "links_clicked_from_twitter": 0,
            "links_clicked_from_facebook": 0,
            "orders": 0,
            "amount_cents": 0,
            "average_amount_cents": 0,
            "expires_at": null,
            "expired": false,
            "completed_referral_customers": [],
            "email": "john.smith@gmail.com",
            "unique_clicks": 0,
            "total_clicks": 0
        }
    },
    "perk": {
        "id": 3,
        "campaign_id": 4,
        "merchant_id": 1,
        "customer_id": 139,
        "reward_points": 0,
        "completed": false,
        "completed_at": null,
        "awarded": false,
        "awarded_at": null,
        "pending": false,
        "reversed": false,
        "reversed_at": null,
        "expired": false,
        "expired_at": null,
        "expires_at": null,
        "redemption_option_id": 152,
        "history_title": "Join our newsletter",
        "created_at": "2016-10-15T16:00:17.000Z",
        "redemption_option": {
            "id": 152,
            "name": "10% Off",
            "description": "Get 10% off your next purchase for 0 points",
            "icon": "fa-percent",
            "cost_text": "0 Points",
            "amount": 0
        }
    }
}

```

This event is triggered any time a customer earns a discount from completing a campaign (e.g. Newsletter Signup).  This event is useful for sending an email with the coupon code to the customer.


Important Attributes | Description
--------- | -----------
email | The email address of the customer who earned points.
redemption.reward_text | The coupon code earned or redeemed.
redemption_option.name | Short description of what the coupon is for.
perk.history_title | A short description of the action that lead them to earn this discount.


### Referral Completed

```ruby

Example Event JSON Structure:

{
    "topic": "swell/referral/completed",
    "name": "Swell Referral Completed",
    "perk": {
        "id": 9,
        "campaign_id": 7,
        "merchant_id": 1,
        "customer_id": 139,
        "reward_points": 500,
        "completed": true,
        "completed_at": "2016-10-15T16:02:56.142Z",
        "awarded": true,
        "awarded_at": "2016-10-15T16:02:56.142Z",
        "pending": false,
        "reversed": false,
        "reversed_at": null,
        "expired": false,
        "expired_at": null,
        "expires_at": null,
        "redemption_option_id": null,
        "history_title": "Refer a friend",
        "created_at": "2016-10-15T16:02:56.125Z"
    },
    "email": "john.smith@gmail.com",
    "referrer": {
        "total_spend_cents": 0,
        "total_purchases": 0,
        "perks_redeemed": 3,
        "last_purchase_at": null,
        "email": "john.smith@gmail.com",
        "points_earned": 5700,
        "points_balance": 5200,
        "points_expire_at": null,
        "first_name": "John",
        "last_name": "Smith",
        "last_seen_at": "2016-10-15T16:01:40.000Z",
        "thirty_party_id": "4302977345",
        "referral_code": {
            "code": "117yu4g",
            "shares": 0,
            "facebook_shares": 0,
            "twitter_shares": 0,
            "email_shares": 0,
            "emails_sent": 0,
            "emails_viewed": 0,
            "links_clicked_from_email": 0,
            "links_clicked_from_twitter": 0,
            "links_clicked_from_facebook": 0,
            "orders": 0,
            "amount_cents": 0,
            "average_amount_cents": 0,
            "expires_at": null,
            "expired": false,
            "completed_referral_customers": [{
                "email": "sally.jane@gmail.com",
                "first_name": "Sally",
                "last_name": "Jane",
                "referral_points_contributed": 0,
                "last_seen_at": "2016-10-15T16:02:51.000Z"
            }],
            "email": "john.smith@gmail.com",
            "unique_clicks": 1,
            "total_clicks": 1
        }
    },
    "referral_code": {
        "id": 139,
        "code": "117yu4g",
        "campaign_id": 7,
        "clicks": 1,
        "invalid_clicks": 0,
        "customer_id": 139,
        "shares": 0,
        "facebook_shares": 0,
        "twitter_shares": 0,
        "email_shares": 0,
        "emails_sent": 0,
        "emails_viewed": 0,
        "links_clicked": 1,
        "links_clicked_from_email": 0,
        "links_clicked_from_twitter": 0,
        "links_clicked_from_facebook": 0,
        "orders": 0,
        "amount_cents": 0,
        "average_amount_cents": 0,
        "expires_at": null,
        "expired": false,
        "target_referrals": null,
        "customer": {
            "id": 139,
            "user_id": null,
            "merchant_id": 1,
            "total_purchases": 0,
            "perks_redeemed": 3,
            "last_purchase_at": null,
            "is_member": false,
            "email": "john.smith@gmail.com",
            "name": null,
            "total_spend_cents": 0,
            "subscribed": true,
            "email_sent_count": 0,
            "email_opened_count": 0,
            "email_clicked_count": 0,
            "email_bounced_count": 0,
            "email_soft_bounced_count": 0,
            "email_rejected_count": 0,
            "email_unsubscribed_count": 0,
            "email_marked_as_spam_count": 0,
            "email_delayed_count": 0,
            "points_earned": 5700,
            "points_balance": 5200,
            "source": "store_account",
            "points_expiration_job_id": null,
            "points_expire_at": null,
            "first_name": "John",
            "last_name": "Smith",
            "prettyTotalSpend": "$0",
            "last_seen_at": "2016-10-15T16:01:40.000Z"
        },
        "completed_referral_customers": [{
            "email": "sally.jane@gmail.com",
            "first_name": "Sally",
            "last_name": "Jane",
            "referral_points_contributed": 0,
            "last_seen_at": "2016-10-15T16:02:51.000Z"
        }],
        "email": "john.smith@gmail.com"
    },
    "referred_customer": {
        "total_spend_cents": 2500,
        "total_purchases": 1,
        "perks_redeemed": 5,
        "last_purchase_at": "2016-10-15T16:02:51.000Z",
        "email": "sally.jane@gmail.com",
        "points_earned": 450,
        "points_balance": 450,
        "points_expire_at": null,
        "first_name": "Sally",
        "last_name": "Jane",
        "last_seen_at": "2016-10-15T16:02:51.000Z",
        "thirty_party_id": "4303221377",
        "referral_code": {
            "code": "77jspfy",
            "shares": 0,
            "facebook_shares": 0,
            "twitter_shares": 0,
            "email_shares": 0,
            "emails_sent": 0,
            "emails_viewed": 0,
            "links_clicked_from_email": 0,
            "links_clicked_from_twitter": 0,
            "links_clicked_from_facebook": 0,
            "orders": 0,
            "amount_cents": 0,
            "average_amount_cents": 0,
            "expires_at": null,
            "expired": false,
            "completed_referral_customers": [],
            "email": "sally.jane@gmail.com",
            "unique_clicks": 0,
            "total_clicks": 0
        }
    }
}

```

This event is triggered any time a customer refers a friend that satisfies the referral program requirements.  This event is useful for sending an email thanking the customer for the referral.


Important Attributes | Description
--------- | -----------
email | The email address of the customer who made the referral.
referral_code | Object has information about their overall referral stats.  Useful for including an overview in an email.
referral_code.completed_referral_customers | An array of customers they have referred so far.
referred_customer.email | The email address of the customer they referred.
referred_customer.first_name | The first name of the customer they referred.
perk.reward_points | If referrer earned points, this is how many.
redemption.reward_text | If referrer earned a coupon, this is the coupon code.
redemption_option.name | If referrer earned a coupon, this is the discount name.


### Points Reminder

```ruby

Example Event JSON Structure:
{
    "topic": "swell/points/reminder",
    "name": "Swell Points Reminder",
    "email": "john.smith@gmail.com",
    "customer": {
        "total_spend_cents": 0,
        "total_purchases": 0,
        "perks_redeemed": 2,
        "last_purchase_at": null,
        "email": "john.smith@gmail.com",
        "points_earned": 375,
        "points_balance": 375,
        "points_expire_at": null,
        "first_name": null,
        "last_name": null,
        "last_seen_at": "2016-10-15T18:14:56.000Z",
        "thirty_party_id": "4302977345",
        "referral_code": {
            "code": "x09u052",
            "shares": 0,
            "facebook_shares": 0,
            "twitter_shares": 0,
            "email_shares": 0,
            "emails_sent": 0,
            "emails_viewed": 0,
            "links_clicked_from_email": 0,
            "links_clicked_from_twitter": 0,
            "links_clicked_from_facebook": 0,
            "orders": 1,
            "amount_cents": 3000,
            "average_amount_cents": 3000,
            "expires_at": null,
            "expired": false,
            "completed_referral_customers": [{
                "email": "hillary.clinton@gmail.com",
                "first_name": "Hillary",
                "last_name": "Clinton",
                "referral_points_contributed": 0,
                "last_seen_at": "2016-10-15T16:38:20.000Z"
            }],
            "email": "john.smith@gmail.com",
            "unique_clicks": 1,
            "total_clicks": 1
        }
    },
    "points_needed": 125,
    "redemption_option": {
        "id": 149,
        "name": "$5.00 Off",
        "description": "Get $5.00 off your next purchase for 500 points",
        "icon": "fa-dollar",
        "cost_text": "500 Points",
        "amount": 500
    }
}

```

This event is triggered after a certain amount of days of inactivity.  This is great for sending an email to remind the customer how close to earning a reward they are.


Important Attributes | Description
--------- | -----------
email | The email address of the customer who made the referral.
points_needed | The number of points needed to earn a reward.
redemption_option | Object describing the reward option they are close to earning.


### Redemption Reminder

```ruby
Example Event JSON Structure:
{
    "topic": "swell/redemption/reminder",
    "name": "Swell Redemption Reminder",
    "email": "john.smith@gmail.com",
    "customer": {
        "total_spend_cents": 0,
        "total_purchases": 0,
        "perks_redeemed": 3,
        "last_purchase_at": null,
        "email": "john.smith@gmail.com",
        "points_earned": 575,
        "points_balance": 575,
        "points_expire_at": null,
        "first_name": null,
        "last_name": null,
        "last_seen_at": "2016-10-15T18:20:22.000Z",
        "thirty_party_id": "4302977345",
        "referral_code": {
            "code": "x09u052",
            "shares": 0,
            "facebook_shares": 0,
            "twitter_shares": 0,
            "email_shares": 0,
            "emails_sent": 0,
            "emails_viewed": 0,
            "links_clicked_from_email": 0,
            "links_clicked_from_twitter": 0,
            "links_clicked_from_facebook": 0,
            "orders": 1,
            "amount_cents": 3000,
            "average_amount_cents": 3000,
            "expires_at": null,
            "expired": false,
            "completed_referral_customers": [{
                "email": "hillary.clinton@gmail.com",
                "first_name": "Hillary",
                "last_name": "Clinton",
                "referral_points_contributed": 0,
                "last_seen_at": "2016-10-15T16:38:20.000Z"
            }],
            "email": "john.smith@gmail.com",
            "unique_clicks": 1,
            "total_clicks": 1
        }
    },
    "redemption_option": {
        "id": 149,
        "name": "$5.00 Off",
        "description": "Get $5.00 off your next purchase for 500 points",
        "icon": "fa-dollar",
        "cost_text": "500 Points",
        "amount": 500
    }
}
```

This event is triggered after a certain amount of days of inactivity and the customer has enough points for a reward.  This is great for sending an email to remind the customer that they have a reward waiting for them.


Important Attributes | Description
--------- | -----------
email | The email address of the customer who made the referral.
redemption_option | Object describing the reward option they are able to earn.



### Customer Birthday

```ruby
Example Event JSON Structure:
{
    "topic": "swell/customer/birthday",
    "name": "Swell Customer Birthday",
    "email": "john.smith@gmail.com",
    "customer": {
        "total_spend_cents": 2500,
        "total_purchases": 1,
        "perks_redeemed": 5,
        "last_purchase_at": "2016-09-23T00:55:35.000Z",
        "email": "asudhquiwehqwiueh123@mailinator.com",
        "points_earned": 450,
        "points_balance": 450,
        "points_expire_at": null,
        "first_name": "John",
        "last_name": "Smith",
        "last_seen_at": "2016-09-23T00:55:35.000Z",
        "thirty_party_id": "4055305025"
    },
    "perk": {
      "id": 2,
      "campaign_id": 4,
      "merchant_id": 1,
      "customer_id": 128,
      "reward_points": 0,
      "completed": false,
      "completed_at": null,
      "awarded": false,
      "awarded_at": null,
      "pending": false,
      "reversed": false,
      "reversed_at": null,
      "expired": false,
      "expired_at": null,
      "expires_at": null,
      "redemption_option_id": 152,
      "history_title": "Happy Birthday",
      "created_at": "2016-09-23T00:47:40.000Z"
    },
    "redemption": {
      "id": 2,
      "created_at": "2016-09-23T00:47:40.758Z",
      "updated_at": "2016-09-23T00:47:40.758Z",
      "reward_text": "abc123",
      "approved": true,
      "approved_at": "2016-09-23T00:47:40.903Z"
    },
    "redemption_option": {
      "id": 149,
      "amount": 500,
      "name": "$5.00 Off",
      "description": "Get $5.00 off your next purchase for 500 points",
      "icon": "fa-dollar",
      "cost_text": "500 Points"
    }
}
```

This event is triggered after a certain amount of days of inactivity and the customer has enough points for a reward.  This is great for sending an email to remind the customer that they have a reward waiting for them.


Important Attributes | Description
--------- | -----------
email | The email address of the customer who's birthday it is.
perk.reward_points | If customer was gifted points, this is how many.
redemption.reward_text | If customer was gifted a coupon, this is the coupon code.
redemption_option.name | If customer was gifted a coupon, this is the discount name.






### Referral Link Share

```ruby
Example Event JSON Structure:

{
    "topic": "swell/referral/share",
    "name": "Swell Referral Share",
    "email": "sally.jane@gmail.com",
    "from": "john.smith@gmail.com",
    "customer": {
        "total_spend_cents": 0,
        "total_purchases": 0,
        "perks_redeemed": 4,
        "last_purchase_at": null,
        "email": "john.smith@gmail.com",
        "points_earned": 675,
        "points_balance": 675,
        "points_expire_at": null,
        "first_name": null,
        "last_name": null,
        "last_seen_at": "2016-10-15T18:43:24.000Z",
        "thirty_party_id": "4302977345",
        "referral_code": {
            "code": "x09u052",
            "shares": 0,
            "facebook_shares": 0,
            "twitter_shares": 0,
            "email_shares": 0,
            "emails_sent": 0,
            "emails_viewed": 0,
            "links_clicked_from_email": 0,
            "links_clicked_from_twitter": 0,
            "links_clicked_from_facebook": 0,
            "orders": 1,
            "amount_cents": 3000,
            "average_amount_cents": 3000,
            "expires_at": null,
            "expired": false,
            "completed_referral_customers": [{
                "email": "hillary.clinton@gmail.com",
                "first_name": "Hillary",
                "last_name": "Clinton",
                "referral_points_contributed": 0,
                "last_seen_at": "2016-10-15T16:38:20.000Z"
            }],
            "email": "john.smith@gmail.com",
            "unique_clicks": 1,
            "total_clicks": 1
        }
    },
    "referral_link": "http://rwrd.io/x09u052"
}
```

This event is triggered when a customer shares their referral link to their friends.  This is great for telling new customers that someone they know like's your store.


Important Attributes | Description
--------- | -----------
email | The email address of the recipient of the referral link (the friend).
customer.first_name | The name of the customer who shared their referral link.
referral_link | The url of the customer's referral link being shared
from | The email address of the customer who shared their referral link



### VIP Tier Earned

```ruby

Example Event JSON Structure:
{
    "topic": "swell/tier/earned",
    "name": "Swell Tier Earned",
    "email": "john.smith@gmail.com",
    "customer": {
        "total_spend_cents": 0,
        "total_purchases": 0,
        "perks_redeemed": 2,
        "last_purchase_at": null,
        "email": "john.smith@gmail.com",
        "points_earned": 375,
        "points_balance": 375,
        "points_expire_at": null,
        "first_name": null,
        "last_name": null,
        "last_seen_at": "2016-10-15T18:14:56.000Z",
        "thirty_party_id": "4302977345",
        "vip_tier_name": "SILVER",
        "vip_tier_ends_at": "2017-10-31",
        "referral_code": {
            "code": "x09u052",
            "shares": 0,
            "facebook_shares": 0,
            "twitter_shares": 0,
            "email_shares": 0,
            "emails_sent": 0,
            "emails_viewed": 0,
            "links_clicked_from_email": 0,
            "links_clicked_from_twitter": 0,
            "links_clicked_from_facebook": 0,
            "orders": 1,
            "amount_cents": 3000,
            "average_amount_cents": 3000,
            "expires_at": null,
            "expired": false,
            "completed_referral_customers": [{
                "email": "hillary.clinton@gmail.com",
                "first_name": "Hillary",
                "last_name": "Clinton",
                "referral_points_contributed": 0,
                "last_seen_at": "2016-10-15T16:38:20.000Z"
            }],
            "email": "john.smith@gmail.com",
            "unique_clicks": 1,
            "total_clicks": 1
        }
    },
    "new_tier": {
        "id": 2,
        "name": "GOLD",
        "description": "Earn Gold status after spending $100",
        "points_earned": 0,
        "amount_spent_cents": 10000,
        "purchases_made": 0,
        "points_multiplier": 1.50,
        "rank": 2
    },
    "old_tier": {
        "id": 1,
        "name": "SILVER",
        "description": "Earn Silver status after spending $50",
        "points_earned": 0,
        "amount_spent_cents": 5000,
        "purchases_made": 0,
        "points_multiplier": 1.20,
        "rank": 1
    }
}

```

This event is triggered when a customer meets the requirement for a new tier. This is great for letting a customer know they reached a certain status level.


Important Attributes | Description
--------- | -----------
email | The email address of the customer who earned the new tier.
customer | The customer object who earned the new tier.
new_tier | Object describing the new tier they just earned
old_tier | Object describing the tier they came from.  Attribute won't be present if they had no previous tier.


### VIP Tier Lost

```ruby

Example Event JSON Structure:
{
    "topic": "swell/tier/lost",
    "name": "Swell Tier Lost",
    "email": "john.smith@gmail.com",
    "customer": {
        "total_spend_cents": 0,
        "total_purchases": 0,
        "perks_redeemed": 2,
        "last_purchase_at": null,
        "email": "john.smith@gmail.com",
        "points_earned": 375,
        "points_balance": 375,
        "points_expire_at": null,
        "first_name": null,
        "last_name": null,
        "last_seen_at": "2016-10-15T18:14:56.000Z",
        "thirty_party_id": "4302977345",
        "vip_tier_name": "SILVER",
        "vip_tier_ends_at": "2017-10-31",
        "referral_code": {
            "code": "x09u052",
            "shares": 0,
            "facebook_shares": 0,
            "twitter_shares": 0,
            "email_shares": 0,
            "emails_sent": 0,
            "emails_viewed": 0,
            "links_clicked_from_email": 0,
            "links_clicked_from_twitter": 0,
            "links_clicked_from_facebook": 0,
            "orders": 1,
            "amount_cents": 3000,
            "average_amount_cents": 3000,
            "expires_at": null,
            "expired": false,
            "completed_referral_customers": [{
                "email": "hillary.clinton@gmail.com",
                "first_name": "Hillary",
                "last_name": "Clinton",
                "referral_points_contributed": 0,
                "last_seen_at": "2016-10-15T16:38:20.000Z"
            }],
            "email": "john.smith@gmail.com",
            "unique_clicks": 1,
            "total_clicks": 1
        }
    },
    "old_tier": {
        "id": 2,
        "name": "GOLD",
        "description": "Earn Gold status after spending $100",
        "points_earned": 0,
        "amount_spent_cents": 10000,
        "purchases_made": 0,
        "points_multiplier": 1.50,
        "rank": 2
    },
    "new_tier": {
        "id": 1,
        "name": "SILVER",
        "description": "Earn Silver status after spending $50",
        "points_earned": 0,
        "amount_spent_cents": 5000,
        "purchases_made": 0,
        "points_multiplier": 1.20,
        "rank": 1
    }
}

```

This event is triggered when a customer fails to meet the requirement for a tier after the specified period of time has passed.


Important Attributes | Description
--------- | -----------
email | The email address of the customer who earned the new tier.
customer | The customer object who earned the new tier.
new_tier | Object describing the new tier they dropped to. Attribute won't be present if they now have no tier.
old_tier | Object describing the tier they just lost.
