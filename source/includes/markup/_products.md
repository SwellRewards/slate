## Products

### Display Points Earned Per Product

```html
<!-- as a paragraph but can easily be any HTML element -->
<p>You will earn <span
  class="swell-product-helper"
  data-mode="display-points"
  data-product-id="123"
  data-price-cents="1200">
    <i class="fa fa-spin fa-spinner" aria-hidden="true"></i>
  </span> points on this purchase</p>
```

Shows the number of potential points earned for the purchase of a product on a product page or elsewhere on your site. To do so, pass along the product's id to the `data-product-id` attribute and the product's price, in cents, to the `data-price-cents` attribute.

### Buy Products With Points (Shopify Plus)

```html
<!-- as a div but can easily be any HTML element -->
<div
  class="swell-buy-product-btn"
  data-variant-id="{{ product.selected_or_first_available_variant.id }}"
  data-hide-logged-out="true|false"
  data-confirm-title="Are you sure?"
  data-confirm-content="You currently have *|point_balance|* points available.  This product costs *|amount|* points to redeem."
  data-confirm-btn-text="Yes, buy it"
  data-error-type="red"
  data-error-title="Whoops!"
  data-error-content="Looks like you don't have enough points for this product.  This product costs *|amount|* points and you only have *|point_balance|* points available."
  data-error-okay-text="Ok"
  data-success-type="green"
  data-success-title="Success!"
  data-success-content="Product was successfully added to your cart"
  data-success-ok-btn="Keep Shopping"
  data-success-cart-btn="View Cart"
  data-success-cart-link="/cart"
  data-box-width="400px"
  data-mobile-box-width="90%">

  BUY FOR <span class="swell-buy-points"></span> POINTS

</div>
```

This is a helper to use our out-of-the-box buy product with points on Shopify. In addition to this markup you will need to :

1. Setup a "Free Product" redemption option in your Swell admin. The data-variant-id in the markup has to match the variant id you entered into the "Free Product" redemption option in your admin.

2. Setup a "Shopify Script" that looks for the line item properties in the cart and reduces the price accordingly.

We have written <a target="_blank" href="https://help.swellrewards.com/article/191-product-redemptions">more detailed instructions</a> to help you get this feature setup.
