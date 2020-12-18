# Customer Model

A customer record is created for both guests and registered users. It is used to store information about the user.

You can get the current customer model in your templates with:

```
{% set customer = craft.commerce.getCustomer() %}
```

If the current customer is a guest, then the `userId` attribute will be null.

If you need to access the email address of the customer, do so from the order with `order.email`.

If you add the [Customer Info](customer-info-fields.md) field to the user profile, you can get the user’s related customer model returned for that user.

## Attributes

### user

Returns a [UserModel](https://craftcms.com/docs/2.x/templating/usermodel.html) object if the customer is logged in, or `null` if the customer is a guest.

### addresses

Returns an array of [Address models](address-model.md).

### orders

Returns an array of completed [Order models](order-model.md).

### userId

Returns the User ID associated with this customer if the customer is a registered user.

### lastUsedShippingAddress

Returns the last used shipping address. Returns `null` if there is not a last used address.

### lastUsedBillingAddress

Returns the last used billing address. Returns `null` if there is not a last used address.
