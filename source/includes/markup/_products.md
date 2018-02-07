## Products

### Display Points Earned Per Product

```html

<!-- as a paragraph but can easily be any HTML element -->
<p>You will earn <span
  class="swell-product-helper"
  data-mode="display-points"
  data-product-id="{{ currentProduct.selected_or_first_available_variant.id }}"
  data-price-cents="{{ currentProduct.selected_or_first_available_variant.price }}">X</span> points on this purchase</p>

```

Shows the number of potential points earned for the purchase of a product on a product page or elsewhere on your site.
