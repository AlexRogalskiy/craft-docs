# Available Variables

The following twig template variables are available:

## craft.commerce.settings

To get all Craft Commerce general settings:

```twig
{% set settings = craft.commerce.settings %}
```

## craft.commerce.products

See [craft.commerce.products](craft-commerce-products.md)

## craft.commerce.orders

See [craft.commerce.orders](craft-commerce-orders.md)

## craft.commerce.cart

See [craft.commerce.cart](craft-commerce-cart.md)

## craft.commerce.countriesList

Returns a list usable for a dropdown select box.

Data returned as `[32:'Australia', 72:'USA']`

```twig
<select>
{% for id, countryName in craft.commerce.countriesList %}
    <option value="{{ id }}">{{ countryName }}</option>
{% endfor %}
</select>
```

## craft.commerce.countries

Returns an array of [Country models](country-model.md).

```twig
<select>
{% for country in craft.commerce.countries %}
    <option value="{{ country.id }}">{{ country.name }}</option>
{% endfor %}
</select>
```

## craft.commerce.statesArray

Returns an array indexed by country IDs, usable for a dropdown select box.

Data returned as `[72:[3:'California', 4:'Washington'],32:[7:'New South Wales']]`

```twig
<select>
{% for countryId, states in craft.commerce.statesArray %}
    <optgroup label="{{ craft.commerce.countriesList[countryId] }}">
    {% for stateId, stateName in craft.commerce.statesArray[countryId] %}
        <option value="{{ stateId }}">{{ stateName }}</option>
    {% endfor %}
  </optgroup>
{% endfor %}
</select>
```

## craft.commerce.states

Returns an array of [State models](state-model.md).

```twig
<select>
{% for states in craft.commerce.countries %}
    <option value="{{ state.id }}">{{ state.name }}</option>
{% endfor %}
</select>
```

## craft.commerce.availableShippingMethods

Returns the shipping methods available to applied to the current cart. Will not include some shipping methods if none of the shipping method’s rules can match the cart.

```twig
{% for handle, method in craft.commerce.availableShippingMethods %}
    <label>
        <input type="radio" name="shippingMethod" value="{{ handle }}"
               {% if handle == cart.shippingMethodHandle %}checked{% endif %} />
        <strong>{{ method.name }}</strong> {{ method.amount|currency(cart.currency) }}
    </label>
{% endfor %}
```

## craft.commerce.paymentMethods

Returns all payment methods available to the customer.

```twig
{% if not craft.commerce.paymentMethods|length %}
    <p>No payment methods available.</p>
{% endif %}

{% if craft.commerce.paymentMethods|length %}
<form method="POST" id="paymentMethod" class="form-inline">

    <input type="hidden" name="action" value="commerce/cart/updateCart">
    <input type="hidden" name="redirect" value="commerce/checkout/payment">
    {{ getCsrfInput() }}

    <label for="">Payment Method</label>
    <select id="paymentMethodId" name="paymentMethodId" class="form-control" >
        {% for id,name in craft.commerce.paymentMethods %}
            <option value="{{ id }}" {% if id == cart.paymentMethod.id %}selected{% endif %}>{{ name }}</option>
        {% endfor %}
    </select>

</form>
{% endif %}
```

## craft.commerce.taxCategories

Returns an array of all tax categories set up in the system.

```twig
{% for taxCategory in craft.commerce.taxCategories %}
{{ taxCategory.id }} - {{ taxCategory.name }}
{% endfor %}
```

See [Order Status Model](order-status-model.md)

## craft.commerce.productTypes

Returns an array of all product types set up in the system.

```twig
{% for type in craft.commerce.productTypes %}
{{ type.handle }} - {{ type.name }}
{% endfor %}
```

## craft.commerce.orderStatuses

Returns an array of all the [Order Status models](order-status-model.md) set up in the system.

```twig
{% for status in craft.commerce.orderStatuses %}
{{ status.handle }} - {{ status.name }}
{% endfor %}
```

## craft.commerce.discounts

Returns an array of all discounts set up in the system.

```twig
{% for discount in craft.commerce.discounts %}
{{ discount.name }} - {{ discount.description }}
{% endfor %}
```

## craft.commerce.getDiscountByCode(code)

Returns a discount that matches the code supplied.

```twig
{% set discount = craft.commerce.getDiscountByCode('HALFOFF')
 %}
{% if discount %}
{{ discount.name }} - {{ discount.description }}
{% endif %}
```

## craft.commerce.sales

Returns an array of all sales set up in the system.

```twig
{% for sale in craft.commerce.sales %}
{{ sale.name }} - {{ sale.description }}
{% endfor %}
```
