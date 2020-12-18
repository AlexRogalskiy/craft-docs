# Order Queries

You can fetch orders in your templates or PHP code using **order queries**.

::: code
```twig
{# Create a new order query #}
{% set myOrderQuery = craft.orders() %}
```
```php
// Create a new order query
$myOrderQuery = \craft\commerce\elements\Order::find();
```
:::

Once you’ve created an order query, you can set [parameters](#parameters) on it to narrow down the results, and then [execute it](https://craftcms.com/docs/3.x/element-queries.html#executing-element-queries) by calling `.all()`. An array of [Order](commerce3:craft\commerce\elements\Order) objects will be returned.

::: tip
See [Element Queries](https://craftcms.com/docs/3.x/element-queries.html) in the Craft docs to learn about how element queries work.
:::

## Example

We can display an order with a given order number by doing the following:

1. Create an order query with `craft.orders()`.
2. Set the [number](#number) parameter on it.
3. Fetch the order with `.one()`.
4. Output information about the order as HTML.

```twig
{# Get the requested order number from the query string #}
{% set orderNumber = craft.app.request.getQueryParam('number') %}

{# Create an order query with the 'number' parameter #}
{% set myOrderQuery = craft.orders()
    .number(orderNumber) %}

{# Fetch the order #}
{% set order = myOrderQuery.one() %}

{# Make sure it exists #}
{% if not order %}
    {% exit 404 %}
{% endif %}

{# Display the order #}
<h1>Order {{ order.getShortNumber() }}</h1>
<!-- ... ->
```

## Parameters

Order queries support the following parameters:

<!-- BEGIN PARAMS -->

| Param                                     | Description
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| [anyStatus](#anystatus)                   | Clears out the [status()](https://docs.craftcms.com/api/v3/craft-elements-db-elementquery.html#method-status) and [enabledForSite()](https://docs.craftcms.com/api/v3/craft-elements-db-elementquery.html#method-enabledforsite) parameters.
| [asArray](#asarray)                       | Causes the query to return matching orders as arrays of data, rather than [Order](commerce3:craft\commerce\elements\Order) objects.
| [clearCachedResult](#clearcachedresult)   | Clears the cached result.
| [customer](#customer)                     | Narrows the query results based on the customer.
| [customerId](#customerid)                 | Narrows the query results based on the customer, per their ID.
| [dateAuthorized](#dateauthorized)         | Narrows the query results based on the orders’ authorized dates.
| [dateCreated](#datecreated)               | Narrows the query results based on the orders’ creation dates.
| [dateOrdered](#dateordered)               | Narrows the query results based on the orders’ completion dates.
| [datePaid](#datepaid)                     | Narrows the query results based on the orders’ paid dates.
| [dateUpdated](#dateupdated)               | Narrows the query results based on the orders’ last-updated dates.
| [email](#email)                           | Narrows the query results based on the customers’ email addresses.
| [expiryDate](#expirydate)                 | Narrows the query results based on the orders’ expiry dates.
| [fixedOrder](#fixedorder)                 | Causes the query results to be returned in the order specified by [id](#id).
| [gateway](#gateway)                       | Narrows the query results based on the gateway.
| [gatewayId](#gatewayid)                   | Narrows the query results based on the gateway, per its ID.
| [hasLineItems](#haslineitems)             | Narrows the query results to only orders that have line items.
| [hasPurchasables](#haspurchasables)       | Narrows the query results to only orders that have certain purchasables.
| [hasTransactions](#hastransactions)       | Narrows the query results to only carts that have at least one transaction.
| [id](#id)                                 | Narrows the query results based on the orders’ IDs.
| [ignorePlaceholders](#ignoreplaceholders) | Causes the query to return matching orders as they are stored in the database, ignoring matching placeholder elements that were set by [craft\services\Elements::setPlaceholderElement()](https://docs.craftcms.com/api/v3/craft-services-elements.html#method-setplaceholderelement).
| [inReverse](#inreverse)                   | Causes the query results to be returned in reverse order.
| [isCompleted](#iscompleted)               | Narrows the query results to only orders that are completed.
| [isPaid](#ispaid)                         | Narrows the query results to only orders that are paid.
| [isUnpaid](#isunpaid)                     | Narrows the query results to only orders that are not paid.
| [limit](#limit)                           | Determines the number of orders that should be returned.
| [number](#number)                         | Narrows the query results based on the order number.
| [offset](#offset)                         | Determines how many orders should be skipped in the results.
| [orderBy](#orderby)                       | Determines the order that the orders should be returned in. (If empty, defaults to `id ASC`.)
| [orderStatus](#orderstatus)               | Narrows the query results based on the order statuses.
| [orderStatusId](#orderstatusid)           | Narrows the query results based on the order statuses, per their IDs.
| [origin](#origin)                         | Narrows the query results based on the origin.
| [preferSites](#prefersites)               | If [unique()](https://docs.craftcms.com/api/v3/craft-elements-db-elementquery.html#method-unique) is set, this determines which site should be selected when querying multi-site elements.
| [reference](#reference)                   | Narrows the query results based on the order reference.
| [relatedTo](#relatedto)                   | Narrows the query results to only orders that are related to certain other elements.
| [search](#search)                         | Narrows the query results to only orders that match a search query.
| [shortNumber](#shortnumber)               | Narrows the query results based on the order short number.
| [trashed](#trashed)                       | Narrows the query results to only orders that have been soft-deleted.
| [uid](#uid)                               | Narrows the query results based on the orders’ UIDs.
| [user](#user)                             | Narrows the query results based on the customer’s user account.
| [with](#with)                             | Causes the query to return matching orders eager-loaded with related elements.

### `anyStatus`

Clears out the [status()](https://docs.craftcms.com/api/v3/craft-elements-db-elementquery.html#method-status) and [enabledForSite()](https://docs.craftcms.com/api/v3/craft-elements-db-elementquery.html#method-enabledforsite) parameters.

::: code
```twig
{# Fetch all orders, regardless of status #}
{% set orders = craft.orders()
    .anyStatus()
    .all() %}
```

```php
// Fetch all orders, regardless of status
$orders = \craft\commerce\elements\Order::find()
    ->anyStatus()
    ->all();
```
:::

### `asArray`

Causes the query to return matching orders as arrays of data, rather than [Order](commerce3:craft\commerce\elements\Order) objects.

::: code
```twig
{# Fetch orders as arrays #}
{% set orders = craft.orders()
    .asArray()
    .all() %}
```

```php
// Fetch orders as arrays
$orders = \craft\commerce\elements\Order::find()
    ->asArray()
    ->all();
```
:::

### `clearCachedResult`

Clears the cached result.

### `customer`

Narrows the query results based on the customer.

Possible values include:

| Value | Fetches orders…
| - | -
| a [Customer](commerce3:craft\commerce\models\Customer) object | with a customer represented by the object.

::: code
```twig
{# Fetch the current customer’s orders #}
{% set currentCustomer = craft.commerce.customers.customer %}
{% set orders = craft.orders()
    .customer(currentCustomer)
    .all() %}
```

```php
// Fetch the current customer’s orders
$currentCustomer = \craft\commerce\Plugin::getInstance()
    ->getCustomers()
    ->customer;
$orders = \craft\commerce\elements\Order::find()
    ->customer($currentCustomer)
    ->all();
```
:::

### `customerId`

Narrows the query results based on the customer, per their ID.

Possible values include:

| Value | Fetches orders…
| - | -
| `1` | with a customer with an ID of 1.
| `'not 1'` | not with a customer with an ID of 1.
| `[1, 2]` | with a customer with an ID of 1 or 2.
| `['not', 1, 2]` | not with a customer with an ID of 1 or 2.

::: code
```twig
{# Fetch the current customer’s orders #}
{% set currentCustomerId = craft.commerce.customers.customer.id %}
{% set orders = craft.orders()
    .customerId(currentCustomerId)
    .all() %}
```

```php
// Fetch the current customer’s orders
$currentCustomerId = \craft\commerce\Plugin::getInstance()
    ->getCustomers()
    ->customerId;
$orders = \craft\commerce\elements\Order::find()
    ->customerId($currentCustomerId)
    ->all();
```
:::

### `dateAuthorized`

Narrows the query results based on the orders’ authorized dates.

Possible values include:

| Value | Fetches orders…
| - | -
| `'>= 2018-04-01'` | that were authorized on or after 2018-04-01.
| `'< 2018-05-01'` | that were authorized before 2018-05-01
| `['and', '>= 2018-04-04', '< 2018-05-01']` | that were completed between 2018-04-01 and 2018-05-01.

::: code
```twig
{# Fetch orders that were authorized recently #}
{% set aWeekAgo = date('7 days ago')|atom %}

{% set orders = craft.orders()
    .dateAuthorized(">= #{aWeekAgo}")
    .all() %}
```

```php
// Fetch orders that were authorized recently
$aWeekAgo = new \DateTime('7 days ago')->format(\DateTime::ATOM);

$orders = \craft\commerce\elements\Order::find()
    ->dateAuthorized(">= {$aWeekAgo}")
    ->all();
```
:::

### `dateCreated`

Narrows the query results based on the orders’ creation dates.

Possible values include:

| Value | Fetches orders…
| - | -
| `'>= 2018-04-01'` | that were created on or after 2018-04-01.
| `'< 2018-05-01'` | that were created before 2018-05-01
| `['and', '>= 2018-04-04', '< 2018-05-01']` | that were created between 2018-04-01 and 2018-05-01.

::: code
```twig
{# Fetch orders created last month #}
{% set start = date('first day of last month')|atom %}
{% set end = date('first day of this month')|atom %}

{% set orders = craft.orders()
    .dateCreated(['and', ">= #{start}", "< #{end}"])
    .all() %}
```

```php
// Fetch orders created last month
$start = (new \DateTime('first day of last month'))->format(\DateTime::ATOM);
$end = (new \DateTime('first day of this month'))->format(\DateTime::ATOM);

$orders = \craft\commerce\elements\Order::find()
    ->dateCreated(['and', ">= {$start}", "< {$end}"])
    ->all();
```
:::

### `dateOrdered`

Narrows the query results based on the orders’ completion dates.

Possible values include:

| Value | Fetches orders…
| - | -
| `'>= 2018-04-01'` | that were completed on or after 2018-04-01.
| `'< 2018-05-01'` | that were completed before 2018-05-01
| `['and', '>= 2018-04-04', '< 2018-05-01']` | that were completed between 2018-04-01 and 2018-05-01.

::: code
```twig
{# Fetch orders that were completed recently #}
{% set aWeekAgo = date('7 days ago')|atom %}

{% set orders = craft.orders()
    .dateOrdered(">= #{aWeekAgo}")
    .all() %}
```

```php
// Fetch orders that were completed recently
$aWeekAgo = new \DateTime('7 days ago')->format(\DateTime::ATOM);

$orders = \craft\commerce\elements\Order::find()
    ->dateOrdered(">= {$aWeekAgo}")
    ->all();
```
:::

### `datePaid`

Narrows the query results based on the orders’ paid dates.

Possible values include:

| Value | Fetches orders…
| - | -
| `'>= 2018-04-01'` | that were paid on or after 2018-04-01.
| `'< 2018-05-01'` | that were paid before 2018-05-01
| `['and', '>= 2018-04-04', '< 2018-05-01']` | that were completed between 2018-04-01 and 2018-05-01.

::: code
```twig
{# Fetch orders that were paid for recently #}
{% set aWeekAgo = date('7 days ago')|atom %}

{% set orders = craft.orders()
    .datePaid(">= #{aWeekAgo}")
    .all() %}
```

```php
// Fetch orders that were paid for recently
$aWeekAgo = new \DateTime('7 days ago')->format(\DateTime::ATOM);

$orders = \craft\commerce\elements\Order::find()
    ->datePaid(">= {$aWeekAgo}")
    ->all();
```
:::

### `dateUpdated`

Narrows the query results based on the orders’ last-updated dates.

Possible values include:

| Value | Fetches orders…
| - | -
| `'>= 2018-04-01'` | that were updated on or after 2018-04-01.
| `'< 2018-05-01'` | that were updated before 2018-05-01
| `['and', '>= 2018-04-04', '< 2018-05-01']` | that were updated between 2018-04-01 and 2018-05-01.

::: code
```twig
{# Fetch orders updated in the last week #}
{% set lastWeek = date('1 week ago')|atom %}

{% set orders = craft.orders()
    .dateUpdated(">= #{lastWeek}")
    .all() %}
```

```php
// Fetch orders updated in the last week
$lastWeek = (new \DateTime('1 week ago'))->format(\DateTime::ATOM);

$orders = \craft\commerce\elements\Order::find()
    ->dateUpdated(">= {$lastWeek}")
    ->all();
```
:::

### `email`

Narrows the query results based on the customers’ email addresses.

Possible values include:

| Value | Fetches orders with customers…
| - | -
| `'foo@bar.baz'` | with an email of `foo@bar.baz`.
| `'not foo@bar.baz'` | not with an email of `foo@bar.baz`.
| `'*@bar.baz'` | with an email that ends with `@bar.baz`.

::: code
```twig
{# Fetch orders from customers with a .co.uk domain on their email address #}
{% set orders = craft.orders()
    .email('*.co.uk')
    .all() %}
```

```php
// Fetch orders from customers with a .co.uk domain on their email address
$orders = \craft\commerce\elements\Order::find()
    ->email('*.co.uk')
    ->all();
```
:::

### `expiryDate`

Narrows the query results based on the orders’ expiry dates.

Possible values include:

| Value | Fetches orders…
| - | -
| `'>= 2020-04-01'` | that will expire on or after 2020-04-01.
| `'< 2020-05-01'` | that will expire before 2020-05-01
| `['and', '>= 2020-04-04', '< 2020-05-01']` | that will expire between 2020-04-01 and 2020-05-01.

::: code
```twig
{# Fetch orders expiring this month #}
{% set nextMonth = date('first day of next month')|atom %}

{% set orders = craft.orders()
    .expiryDate("< #{nextMonth}")
    .all() %}
```

```php
// Fetch orders expiring this month
$nextMonth = new \DateTime('first day of next month')->format(\DateTime::ATOM);

$orders = \craft\commerce\elements\Order::find()
    ->expiryDate("< {$nextMonth}")
    ->all();
```
:::

### `fixedOrder`

Causes the query results to be returned in the order specified by [id](#id).

::: code
```twig
{# Fetch orders in a specific order #}
{% set orders = craft.orders()
    .id([1, 2, 3, 4, 5])
    .fixedOrder()
    .all() %}
```

```php
// Fetch orders in a specific order
$orders = \craft\commerce\elements\Order::find()
    ->id([1, 2, 3, 4, 5])
    ->fixedOrder()
    ->all();
```
:::

### `gateway`

Narrows the query results based on the gateway.

Possible values include:

| Value | Fetches orders…
| - | -
| a [Gateway](commerce3:craft\commerce\base\Gateway) object | with a gateway represented by the object.

### `gatewayId`

Narrows the query results based on the gateway, per its ID.

Possible values include:

| Value | Fetches orders…
| - | -
| `1` | with a gateway with an ID of 1.
| `'not 1'` | not with a gateway with an ID of 1.
| `[1, 2]` | with a gateway with an ID of 1 or 2.
| `['not', 1, 2]` | not with a gateway with an ID of 1 or 2.

### `hasLineItems`

Narrows the query results to only orders that have line items. (If empty, defaults to `true`.)

Possible values include:

| Value | Fetches orders…
| - | -
| `true` | that have line items.
| `false` | that do not have any line items.

::: code
```twig
{# Fetch orders that do or do not have line items #}
{% set orders = craft.orders()
    .hasLineItems()
    .all() %}
```

```php
// Fetch unpaid orders
$orders = \craft\commerce\elements\Order::find()
    ->hasLineItems()
    ->all();
```
:::

### `hasPurchasables`

Narrows the query results to only orders that have certain purchasables.

Possible values include:

| Value | Fetches orders…
| - | -
| `1` | with a purchasable with an ID of 1.
| `[1, 2]` | with a purchasable with an ID of 1 or 2.
| a [PurchasableInterface](commerce3:craft\commerce\base\PurchasableInterface) object | with a purchasable represented by the object.
| an array of [PurchasableInterface](commerce3:craft\commerce\base\PurchasableInterface) objects | with all the purchasables represented by the objects.

::: code
```twig
{# Fetch carts that have attempted payments #}
{% set orders = craft.orders()
    .hasPurchasables(1)
    .all() %}
```

```php
// Fetch carts that have attempted payments
$orders = \craft\commerce\elements\Order::find()
    ->hasPurchasables(1)
    ->all();
```
:::

### `hasTransactions`

Narrows the query results to only carts that have at least one transaction. (If empty, defaults to `true`.)

Possible values include:

| Value | Fetches orders…
| - | -
| `true` | that have transactions.
| `false` | that do not have any transactions.

::: code
```twig
{# Fetch carts that have attempted payments #}
{% set orders = craft.orders()
    .hasTransactions()
    .all() %}
```

```php
// Fetch carts that have attempted payments
$orders = \craft\commerce\elements\Order::find()
    ->hasTransactions()
    ->all();
```
:::

### `id`

Narrows the query results based on the orders’ IDs.

Possible values include:

| Value | Fetches orders…
| - | -
| `1` | with an ID of 1.
| `'not 1'` | not with an ID of 1.
| `[1, 2]` | with an ID of 1 or 2.
| `['not', 1, 2]` | not with an ID of 1 or 2.

::: code
```twig
{# Fetch the order by its ID #}
{% set order = craft.orders()
    .id(1)
    .one() %}
```

```php
// Fetch the order by its ID
$order = \craft\commerce\elements\Order::find()
    ->id(1)
    ->one();
```
:::

::: tip
This can be combined with [fixedOrder](#fixedorder) if you want the results to be returned in a specific order.
:::

### `ignorePlaceholders`

Causes the query to return matching orders as they are stored in the database, ignoring matching placeholder
elements that were set by [craft\services\Elements::setPlaceholderElement()](https://docs.craftcms.com/api/v3/craft-services-elements.html#method-setplaceholderelement).

### `inReverse`

Causes the query results to be returned in reverse order.

::: code
```twig
{# Fetch orders in reverse #}
{% set orders = craft.orders()
    .inReverse()
    .all() %}
```

```php
// Fetch orders in reverse
$orders = \craft\commerce\elements\Order::find()
    ->inReverse()
    ->all();
```
:::

### `isCompleted`

Narrows the query results to only orders that are completed.

::: code
```twig
{# Fetch completed orders #}
{% set orders = craft.orders()
    .isCompleted()
    .all() %}
```

```php
// Fetch completed orders
$orders = \craft\commerce\elements\Order::find()
    ->isCompleted()
    ->all();
```
:::

### `isPaid`

Narrows the query results to only orders that are paid.

::: code
```twig
{# Fetch paid orders #}
{% set orders = craft.orders()
    .isPaid()
    .all() %}
```

```php
// Fetch paid orders
$orders = \craft\commerce\elements\Order::find()
    ->isPaid()
    ->all();
```
:::

### `isUnpaid`

Narrows the query results to only orders that are not paid.

::: code
```twig
{# Fetch unpaid orders #}
{% set orders = craft.orders()
    .isUnpaid()
    .all() %}
```

```php
// Fetch unpaid orders
$orders = \craft\commerce\elements\Order::find()
    ->isUnpaid()
    ->all();
```
:::

### `limit`

Determines the number of orders that should be returned.

::: code
```twig
{# Fetch up to 10 orders  #}
{% set orders = craft.orders()
    .limit(10)
    .all() %}
```

```php
// Fetch up to 10 orders
$orders = \craft\commerce\elements\Order::find()
    ->limit(10)
    ->all();
```
:::

### `number`

Narrows the query results based on the order number.

Possible values include:

| Value | Fetches orders…
| - | -
| `'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'` | with a matching order number

::: code
```twig
{# Fetch the requested order #}
{% set orderNumber = craft.app.request.getQueryParam('number') %}
{% set order = craft.orders()
    .number(orderNumber)
    .one() %}
```

```php
// Fetch the requested order
$orderNumber = Craft::$app->request->getQueryParam('number');
$order = \craft\commerce\elements\Order::find()
    ->number($orderNumber)
    ->one();
```
:::

### `offset`

Determines how many orders should be skipped in the results.

::: code
```twig
{# Fetch all orders except for the first 3 #}
{% set orders = craft.orders()
    .offset(3)
    .all() %}
```

```php
// Fetch all orders except for the first 3
$orders = \craft\commerce\elements\Order::find()
    ->offset(3)
    ->all();
```
:::

### `orderBy`

Determines the order that the orders should be returned in. (If empty, defaults to `id ASC`.)

::: code
```twig
{# Fetch all orders in order of date created #}
{% set orders = craft.orders()
    .orderBy('dateCreated asc')
    .all() %}
```

```php
// Fetch all orders in order of date created
$orders = \craft\commerce\elements\Order::find()
    ->orderBy('dateCreated asc')
    ->all();
```
:::

### `orderStatus`

Narrows the query results based on the order statuses.

Possible values include:

| Value | Fetches orders…
| - | -
| `'foo'` | with an order status with a handle of `foo`.
| `'not foo'` | not with an order status with a handle of `foo`.
| `['foo', 'bar']` | with an order status with a handle of `foo` or `bar`.
| `['not', 'foo', 'bar']` | not with an order status with a handle of `foo` or `bar`.
| a [OrderStatus](commerce3:craft\commerce\models\OrderStatus) object | with an order status represented by the object.

::: code
```twig
{# Fetch shipped orders #}
{% set orders = craft.orders()
    .orderStatus('shipped')
    .all() %}
```

```php
// Fetch shipped orders
$orders = \craft\commerce\elements\Order::find()
    ->orderStatus('shipped')
    ->all();
```
:::

### `orderStatusId`

Narrows the query results based on the order statuses, per their IDs.

Possible values include:

| Value | Fetches orders…
| - | -
| `1` | with an order status with an ID of 1.
| `'not 1'` | not with an order status with an ID of 1.
| `[1, 2]` | with an order status with an ID of 1 or 2.
| `['not', 1, 2]` | not with an order status with an ID of 1 or 2.

::: code
```twig
{# Fetch orders with an order status with an ID of 1 #}
{% set orders = craft.orders()
    .authorGroupId(1)
    .all() %}
```

```php
// Fetch orders with an order status with an ID of 1
$orders = \craft\commerce\elements\Order::find()
    ->authorGroupId(1)
    ->all();
```
:::

### `origin`

Narrows the query results based on the origin.

Possible values include:

| Value | Fetches orders…
| - | -
| `'web'` | with an origin of `web`.
| `'not remote'` | not with an origin of `remote`.
| `['web', 'cp']` | with an order origin of `web` or `cp`.
| `['not', 'remote', 'cp']` | not with an origin of `web` or `cp`.

::: code
```twig
{# Fetch shipped orders #}
{% set orders = craft.orders()
    .origin('web')
    .all() %}
```

```php
// Fetch shipped orders
$orders = \craft\commerce\elements\Order::find()
    ->origin('web')
    ->all();
```
:::

### `preferSites`

If [unique()](https://docs.craftcms.com/api/v3/craft-elements-db-elementquery.html#method-unique) is set, this determines which site should be selected when querying multi-site elements.

For example, if element “Foo” exists in Site A and Site B, and element “Bar” exists in Site B and Site C,
and this is set to `['c', 'b', 'a']`, then Foo will be returned for Site C, and Bar will be returned
for Site B.

If this isn’t set, then preference goes to the current site.

::: code
```twig
{# Fetch unique orders from Site A, or Site B if they don’t exist in Site A #}
{% set orders = craft.orders()
    .site('*')
    .unique()
    .preferSites(['a', 'b'])
    .all() %}
```

```php
// Fetch unique orders from Site A, or Site B if they don’t exist in Site A
$orders = \craft\commerce\elements\Order::find()
    ->site('*')
    ->unique()
    ->preferSites(['a', 'b'])
    ->all();
```
:::

### `reference`

Narrows the query results based on the order reference.

Possible values include:

| Value | Fetches orders…
| - | -
| `'xxxx'` | with a matching order reference

::: code
```twig
{# Fetch the requested order #}
{% set orderReference = craft.app.request.getQueryParam('ref') %}
{% set order = craft.orders()
    .reference(orderReference)
    .one() %}
```

```php
// Fetch the requested order
$orderReference = Craft::$app->request->getQueryParam('ref');
$order = \craft\commerce\elements\Order::find()
    ->reference($orderReference)
    ->one();
```
:::

### `relatedTo`

Narrows the query results to only orders that are related to certain other elements.

See [Relations](https://craftcms.com/docs/3.x/relations.html) for a full explanation of how to work with this parameter.

::: code
```twig
{# Fetch all orders that are related to myCategory #}
{% set orders = craft.orders()
    .relatedTo(myCategory)
    .all() %}
```

```php
// Fetch all orders that are related to $myCategory
$orders = \craft\commerce\elements\Order::find()
    ->relatedTo($myCategory)
    ->all();
```
:::

### `search`

Narrows the query results to only orders that match a search query.

See [Searching](https://craftcms.com/docs/3.x/searching.html) for a full explanation of how to work with this parameter.

::: code
```twig
{# Get the search query from the 'q' query string param #}
{% set searchQuery = craft.app.request.getQueryParam('q') %}

{# Fetch all orders that match the search query #}
{% set orders = craft.orders()
    .search(searchQuery)
    .all() %}
```

```php
// Get the search query from the 'q' query string param
$searchQuery = \Craft::$app->request->getQueryParam('q');

// Fetch all orders that match the search query
$orders = \craft\commerce\elements\Order::find()
    ->search($searchQuery)
    ->all();
```
:::

### `shortNumber`

Narrows the query results based on the order short number.

Possible values include:

| Value | Fetches orders…
| - | -
| `'xxxxxxx'` | with a matching order number

::: code
```twig
{# Fetch the requested order #}
{% set orderNumber = craft.app.request.getQueryParam('shortNumber') %}
{% set order = craft.orders()
    .shortNumber(orderNumber)
    .one() %}
```

```php
// Fetch the requested order
$orderNumber = Craft::$app->request->getQueryParam('shortNumber');
$order = \craft\commerce\elements\Order::find()
    ->shortNumber($orderNumber)
    ->one();
```
:::

### `trashed`

Narrows the query results to only orders that have been soft-deleted.

::: code
```twig
{# Fetch trashed orders #}
{% set orders = craft.orders()
    .trashed()
    .all() %}
```

```php
// Fetch trashed orders
$orders = \craft\commerce\elements\Order::find()
    ->trashed()
    ->all();
```
:::

### `uid`

Narrows the query results based on the orders’ UIDs.

::: code
```twig
{# Fetch the order by its UID #}
{% set order = craft.orders()
    .uid('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx')
    .one() %}
```

```php
// Fetch the order by its UID
$order = \craft\commerce\elements\Order::find()
    ->uid('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx')
    ->one();
```
:::

### `user`

Narrows the query results based on the customer’s user account.

Possible values include:

| Value | Fetches orders…
| - | -
| `1` | with a customer with a user account ID of 1.
| a [User](https://docs.craftcms.com/api/v3/craft-elements-user.html) object | with a customer with a user account represented by the object.

::: code
```twig
{# Fetch the current user's orders #}
{% set orders = craft.orders()
    .user(currentUser)
    .all() %}
```

```php
// Fetch the current user's orders
$user = Craft::$app->user->getIdentity();
$orders = \craft\commerce\elements\Order::find()
    ->user($user)
    ->all();
```
:::

### `with`

Causes the query to return matching orders eager-loaded with related elements.

See [Eager-Loading Elements](https://craftcms.com/docs/3.x/dev/eager-loading-elements.html) for a full explanation of how to work with this parameter.

::: code
```twig
{# Fetch orders eager-loaded with the "Related" field’s relations #}
{% set orders = craft.orders()
    .with(['related'])
    .all() %}
```

```php
// Fetch orders eager-loaded with the "Related" field’s relations
$orders = \craft\commerce\elements\Order::find()
    ->with(['related'])
    ->all();
```
:::

<!-- END PARAMS -->
