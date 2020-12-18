# Product Queries

You can fetch products using **product queries**.

::: code
```twig
{# Create a new product query #}
{% set myProductQuery = craft.products() %}
```
```php
// Create a new product query
$myProductQuery = \craft\commerce\elements\Product::find();
```
```graphql
# Create a new product query
{
  products {
    # ...
  }
}
```
:::

Once you’ve created a product query, you can set [parameters](#parameters) on it to narrow down the results, and then [execute it](https://craftcms.com/docs/3.x/element-queries.html#executing-element-queries) by calling `.all()`. An array of [Product](commerce3:craft\commerce\elements\Product) objects will be returned.

::: tip
See [Element Queries](https://craftcms.com/docs/3.x/element-queries.html) in the Craft docs to learn about how element queries work.
:::

## Example

We can use Twig to display the ten most recent Clothing products:

1. Create a product query with `craft.products()`.
2. Set the [type](#type) an [limit](#limit) parameters on it.
3. Fetch the products with `.all()`.
4. Loop through the products using a [for](https://twig.symfony.com/doc/2.x/tags/for.html) tag to output their HTML.

```twig
{# Create a product query with the 'type' and 'limit' parameters #}
{% set myProductQuery = craft.products()
    .type('clothing')
    .limit(10) %}

{# Fetch the products #}
{% set products = myProductQuery.all() %}

{# Display the products #}
{% for product in products %}
    <h1><a href="{{ product.url }}">{{ product.title }}</a></h1>
    {{ product.summary }}
    <a href="{{ product.url }}">Learn more</a>
{% endfor %}
```

To fetch the same information with GraphQL, we could write a query like this:

```graphql
{
  products(limit: 10, type: "clothing") {
    title
    uri
    ... on clothing_Product {
      summary
    }
  }
}
```

## Parameters

Product queries support the following parameters:

<!-- BEGIN PARAMS -->

| Param                                         | Description
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| [after](#after)                               | Narrows the query results to only products that were posted on or after a certain date.
| [anyStatus](#anystatus)                       | Clears out the [status](#status) and [enabledForSite()](https://docs.craftcms.com/api/v3/craft-elements-db-elementquery.html#method-enabledforsite) parameters.
| [asArray](#asarray)                           | Causes the query to return matching products as arrays of data, rather than [Product](commerce3:craft\commerce\elements\Product) objects.
| [availableForPurchase](#availableforpurchase) | Narrows the query results to only products that are available for purchase.
| [before](#before)                             | Narrows the query results to only products that were posted before a certain date.
| [clearCachedResult](#clearcachedresult)       | Clears the cached result.
| [dateCreated](#datecreated)                   | Narrows the query results based on the products’ creation dates.
| [dateUpdated](#dateupdated)                   | Narrows the query results based on the products’ last-updated dates.
| [expiryDate](#expirydate)                     | Narrows the query results based on the products’ expiry dates.
| [fixedOrder](#fixedorder)                     | Causes the query results to be returned in the order specified by [id](#id).
| [hasVariant](#hasvariant)                     | Narrows the query results to only products that have certain variants.
| [id](#id)                                     | Narrows the query results based on the products’ IDs.
| [ignorePlaceholders](#ignoreplaceholders)     | Causes the query to return matching products as they are stored in the database, ignoring matching placeholder elements that were set by [craft\services\Elements::setPlaceholderElement()](https://docs.craftcms.com/api/v3/craft-services-elements.html#method-setplaceholderelement).
| [inReverse](#inreverse)                       | Causes the query results to be returned in reverse order.
| [limit](#limit)                               | Determines the number of products that should be returned.
| [offset](#offset)                             | Determines how many products should be skipped in the results.
| [orderBy](#orderby)                           | Determines the order that the products should be returned in. (If empty, defaults to `postDate DESC`.)
| [postDate](#postdate)                         | Narrows the query results based on the products’ post dates.
| [preferSites](#prefersites)                   | If [unique](#unique) is set, this determines which site should be selected when querying multi-site elements.
| [relatedTo](#relatedto)                       | Narrows the query results to only products that are related to certain other elements.
| [search](#search)                             | Narrows the query results to only products that match a search query.
| [site](#site)                                 | Determines which site(s) the products should be queried in.
| [siteId](#siteid)                             | Determines which site(s) the products should be queried in, per the site’s ID.
| [slug](#slug)                                 | Narrows the query results based on the products’ slugs.
| [status](#status)                             | Narrows the query results based on the products’ statuses.
| [title](#title)                               | Narrows the query results based on the products’ titles.
| [trashed](#trashed)                           | Narrows the query results to only products that have been soft-deleted.
| [type](#type)                                 | Narrows the query results based on the products’ types.
| [typeId](#typeid)                             | Narrows the query results based on the products’ types, per the types’ IDs.
| [uid](#uid)                                   | Narrows the query results based on the products’ UIDs.
| [unique](#unique)                             | Determines whether only elements with unique IDs should be returned by the query.
| [uri](#uri)                                   | Narrows the query results based on the products’ URIs.
| [with](#with)                                 | Causes the query to return matching products eager-loaded with related elements.

### `after`

Narrows the query results to only products that were posted on or after a certain date.

Possible values include:

| Value | Fetches products…
| - | -
| `'2018-04-01'` | that were posted after 2018-04-01.
| a [DateTime](http://php.net/class.datetime) object | that were posted after the date represented by the object.



::: code
```twig
{# Fetch products posted this month #}
{% set firstDayOfMonth = date('first day of this month') %}

{% set products = craft.products()
    .after(firstDayOfMonth)
    .all() %}
```

```php
// Fetch products posted this month
$firstDayOfMonth = new \DateTime('first day of this month');

$products = \craft\commerce\elements\Product::find()
    ->after($firstDayOfMonth)
    ->all();
```
:::


### `anyStatus`

Clears out the [status](#status) and [enabledForSite()](https://docs.craftcms.com/api/v3/craft-elements-db-elementquery.html#method-enabledforsite) parameters.





::: code
```twig
{# Fetch all products, regardless of status #}
{% set products = craft.products()
    .anyStatus()
    .all() %}
```

```php
// Fetch all products, regardless of status
$products = \craft\commerce\elements\Product::find()
    ->anyStatus()
    ->all();
```
:::


### `asArray`

Causes the query to return matching products as arrays of data, rather than [Product](commerce3:craft\commerce\elements\Product) objects.





::: code
```twig
{# Fetch products as arrays #}
{% set products = craft.products()
    .asArray()
    .all() %}
```

```php
// Fetch products as arrays
$products = \craft\commerce\elements\Product::find()
    ->asArray()
    ->all();
```
:::


### `availableForPurchase`

Narrows the query results to only products that are available for purchase.



::: code
```twig
{# Fetch products that are available for purchase #}
{% set products = {twig-function}
    .availableForPurchase()
    .all() %}
```

```php
// Fetch products that are available for purchase
$products = \craft\commerce\elements\Product::find()
    ->availableForPurchase()
    ->all();
```
:::


### `before`

Narrows the query results to only products that were posted before a certain date.

Possible values include:

| Value | Fetches products…
| - | -
| `'2018-04-01'` | that were posted before 2018-04-01.
| a [DateTime](http://php.net/class.datetime) object | that were posted before the date represented by the object.



::: code
```twig
{# Fetch products posted before this month #}
{% set firstDayOfMonth = date('first day of this month') %}

{% set products = craft.products()
    .before(firstDayOfMonth)
    .all() %}
```

```php
// Fetch products posted before this month
$firstDayOfMonth = new \DateTime('first day of this month');

$products = \craft\commerce\elements\Product::find()
    ->before($firstDayOfMonth)
    ->all();
```
:::


### `clearCachedResult`

Clears the cached result.






### `dateCreated`

Narrows the query results based on the products’ creation dates.



Possible values include:

| Value | Fetches products…
| - | -
| `'>= 2018-04-01'` | that were created on or after 2018-04-01.
| `'< 2018-05-01'` | that were created before 2018-05-01
| `['and', '>= 2018-04-04', '< 2018-05-01']` | that were created between 2018-04-01 and 2018-05-01.



::: code
```twig
{# Fetch products created last month #}
{% set start = date('first day of last month')|atom %}
{% set end = date('first day of this month')|atom %}

{% set products = craft.products()
    .dateCreated(['and', ">= #{start}", "< #{end}"])
    .all() %}
```

```php
// Fetch products created last month
$start = (new \DateTime('first day of last month'))->format(\DateTime::ATOM);
$end = (new \DateTime('first day of this month'))->format(\DateTime::ATOM);

$products = \craft\commerce\elements\Product::find()
    ->dateCreated(['and', ">= {$start}", "< {$end}"])
    ->all();
```
:::


### `dateUpdated`

Narrows the query results based on the products’ last-updated dates.



Possible values include:

| Value | Fetches products…
| - | -
| `'>= 2018-04-01'` | that were updated on or after 2018-04-01.
| `'< 2018-05-01'` | that were updated before 2018-05-01
| `['and', '>= 2018-04-04', '< 2018-05-01']` | that were updated between 2018-04-01 and 2018-05-01.



::: code
```twig
{# Fetch products updated in the last week #}
{% set lastWeek = date('1 week ago')|atom %}

{% set products = craft.products()
    .dateUpdated(">= #{lastWeek}")
    .all() %}
```

```php
// Fetch products updated in the last week
$lastWeek = (new \DateTime('1 week ago'))->format(\DateTime::ATOM);

$products = \craft\commerce\elements\Product::find()
    ->dateUpdated(">= {$lastWeek}")
    ->all();
```
:::


### `expiryDate`

Narrows the query results based on the products’ expiry dates.

Possible values include:

| Value | Fetches products…
| - | -
| `'>= 2020-04-01'` | that will expire on or after 2020-04-01.
| `'< 2020-05-01'` | that will expire before 2020-05-01
| `['and', '>= 2020-04-04', '< 2020-05-01']` | that will expire between 2020-04-01 and 2020-05-01.



::: code
```twig
{# Fetch products expiring this month #}
{% set nextMonth = date('first day of next month')|atom %}

{% set products = craft.products()
    .expiryDate("< #{nextMonth}")
    .all() %}
```

```php
// Fetch products expiring this month
$nextMonth = new \DateTime('first day of next month')->format(\DateTime::ATOM);

$products = \craft\commerce\elements\Product::find()
    ->expiryDate("< {$nextMonth}")
    ->all();
```
:::


### `fixedOrder`

Causes the query results to be returned in the order specified by [id](#id).





::: code
```twig
{# Fetch products in a specific order #}
{% set products = craft.products()
    .id([1, 2, 3, 4, 5])
    .fixedOrder()
    .all() %}
```

```php
// Fetch products in a specific order
$products = \craft\commerce\elements\Product::find()
    ->id([1, 2, 3, 4, 5])
    ->fixedOrder()
    ->all();
```
:::


### `hasVariant`

Narrows the query results to only products that have certain variants.

Possible values include:

| Value | Fetches products…
| - | -
| a [VariantQuery](commerce3:craft\commerce\elements\db\VariantQuery) object | with variants that match the query.




### `id`

Narrows the query results based on the products’ IDs.



Possible values include:

| Value | Fetches products…
| - | -
| `1` | with an ID of 1.
| `'not 1'` | not with an ID of 1.
| `[1, 2]` | with an ID of 1 or 2.
| `['not', 1, 2]` | not with an ID of 1 or 2.



::: code
```twig
{# Fetch the product by its ID #}
{% set product = craft.products()
    .id(1)
    .one() %}
```

```php
// Fetch the product by its ID
$product = \craft\commerce\elements\Product::find()
    ->id(1)
    ->one();
```
:::



::: tip
This can be combined with [fixedOrder](#fixedorder) if you want the results to be returned in a specific order.
:::


### `ignorePlaceholders`

Causes the query to return matching products as they are stored in the database, ignoring matching placeholder
elements that were set by [craft\services\Elements::setPlaceholderElement()](https://docs.craftcms.com/api/v3/craft-services-elements.html#method-setplaceholderelement).










### `inReverse`

Causes the query results to be returned in reverse order.





::: code
```twig
{# Fetch products in reverse #}
{% set products = craft.products()
    .inReverse()
    .all() %}
```

```php
// Fetch products in reverse
$products = \craft\commerce\elements\Product::find()
    ->inReverse()
    ->all();
```
:::


### `limit`

Determines the number of products that should be returned.



::: code
```twig
{# Fetch up to 10 products  #}
{% set products = craft.products()
    .limit(10)
    .all() %}
```

```php
// Fetch up to 10 products
$products = \craft\commerce\elements\Product::find()
    ->limit(10)
    ->all();
```
:::


### `offset`

Determines how many products should be skipped in the results.



::: code
```twig
{# Fetch all products except for the first 3 #}
{% set products = craft.products()
    .offset(3)
    .all() %}
```

```php
// Fetch all products except for the first 3
$products = \craft\commerce\elements\Product::find()
    ->offset(3)
    ->all();
```
:::


### `orderBy`

Determines the order that the products should be returned in. (If empty, defaults to `postDate DESC`.)



::: code
```twig
{# Fetch all products in order of date created #}
{% set products = craft.products()
    .orderBy('dateCreated asc')
    .all() %}
```

```php
// Fetch all products in order of date created
$products = \craft\commerce\elements\Product::find()
    ->orderBy('dateCreated asc')
    ->all();
```
:::


### `postDate`

Narrows the query results based on the products’ post dates.

Possible values include:

| Value | Fetches products…
| - | -
| `'>= 2018-04-01'` | that were posted on or after 2018-04-01.
| `'< 2018-05-01'` | that were posted before 2018-05-01
| `['and', '>= 2018-04-04', '< 2018-05-01']` | that were posted between 2018-04-01 and 2018-05-01.



::: code
```twig
{# Fetch products posted last month #}
{% set start = date('first day of last month')|atom %}
{% set end = date('first day of this month')|atom %}

{% set products = craft.products()
    .postDate(['and', ">= #{start}", "< #{end}"])
    .all() %}
```

```php
// Fetch products posted last month
$start = new \DateTime('first day of next month')->format(\DateTime::ATOM);
$end = new \DateTime('first day of this month')->format(\DateTime::ATOM);

$products = \craft\commerce\elements\Product::find()
    ->postDate(['and', ">= {$start}", "< {$end}"])
    ->all();
```
:::


### `preferSites`

If [unique](#unique) is set, this determines which site should be selected when querying multi-site elements.



For example, if element “Foo” exists in Site A and Site B, and element “Bar” exists in Site B and Site C,
and this is set to `['c', 'b', 'a']`, then Foo will be returned for Site C, and Bar will be returned
for Site B.

If this isn’t set, then preference goes to the current site.



::: code
```twig
{# Fetch unique products from Site A, or Site B if they don’t exist in Site A #}
{% set products = craft.products()
    .site('*')
    .unique()
    .preferSites(['a', 'b'])
    .all() %}
```

```php
// Fetch unique products from Site A, or Site B if they don’t exist in Site A
$products = \craft\commerce\elements\Product::find()
    ->site('*')
    ->unique()
    ->preferSites(['a', 'b'])
    ->all();
```
:::


### `relatedTo`

Narrows the query results to only products that are related to certain other elements.



See [Relations](https://craftcms.com/docs/3.x/relations.html) for a full explanation of how to work with this parameter.



::: code
```twig
{# Fetch all products that are related to myCategory #}
{% set products = craft.products()
    .relatedTo(myCategory)
    .all() %}
```

```php
// Fetch all products that are related to $myCategory
$products = \craft\commerce\elements\Product::find()
    ->relatedTo($myCategory)
    ->all();
```
:::


### `search`

Narrows the query results to only products that match a search query.



See [Searching](https://craftcms.com/docs/3.x/searching.html) for a full explanation of how to work with this parameter.



::: code
```twig
{# Get the search query from the 'q' query string param #}
{% set searchQuery = craft.app.request.getQueryParam('q') %}

{# Fetch all products that match the search query #}
{% set products = craft.products()
    .search(searchQuery)
    .all() %}
```

```php
// Get the search query from the 'q' query string param
$searchQuery = \Craft::$app->request->getQueryParam('q');

// Fetch all products that match the search query
$products = \craft\commerce\elements\Product::find()
    ->search($searchQuery)
    ->all();
```
:::


### `site`

Determines which site(s) the products should be queried in.



The current site will be used by default.

Possible values include:

| Value | Fetches products…
| - | -
| `'foo'` | from the site with a handle of `foo`.
| `['foo', 'bar']` | from a site with a handle of `foo` or `bar`.
| `['not', 'foo', 'bar']` | not in a site with a handle of `foo` or `bar`.
| a [craft\models\Site](https://docs.craftcms.com/api/v3/craft-models-site.html) object | from the site represented by the object.
| `'*'` | from any site.

::: tip
If multiple sites are specified, elements that belong to multiple sites will be returned multiple times. If you
only want unique elements to be returned, use [unique](#unique) in conjunction with this.
:::



::: code
```twig
{# Fetch products from the Foo site #}
{% set products = craft.products()
    .site('foo')
    .all() %}
```

```php
// Fetch products from the Foo site
$products = \craft\commerce\elements\Product::find()
    ->site('foo')
    ->all();
```
:::


### `siteId`

Determines which site(s) the products should be queried in, per the site’s ID.



The current site will be used by default.



::: code
```twig
{# Fetch products from the site with an ID of 1 #}
{% set products = craft.products()
    .siteId(1)
    .all() %}
```

```php
// Fetch products from the site with an ID of 1
$products = \craft\commerce\elements\Product::find()
    ->siteId(1)
    ->all();
```
:::


### `slug`

Narrows the query results based on the products’ slugs.



Possible values include:

| Value | Fetches products…
| - | -
| `'foo'` | with a slug of `foo`.
| `'foo*'` | with a slug that begins with `foo`.
| `'*foo'` | with a slug that ends with `foo`.
| `'*foo*'` | with a slug that contains `foo`.
| `'not *foo*'` | with a slug that doesn’t contain `foo`.
| `['*foo*', '*bar*']` | with a slug that contains `foo` or `bar`.
| `['not', '*foo*', '*bar*']` | with a slug that doesn’t contain `foo` or `bar`.



::: code
```twig
{# Get the requested product slug from the URL #}
{% set requestedSlug = craft.app.request.getSegment(3) %}

{# Fetch the product with that slug #}
{% set product = craft.products()
    .slug(requestedSlug|literal)
    .one() %}
```

```php
// Get the requested product slug from the URL
$requestedSlug = \Craft::$app->request->getSegment(3);

// Fetch the product with that slug
$product = \craft\commerce\elements\Product::find()
    ->slug(\craft\helpers\Db::escapeParam($requestedSlug))
    ->one();
```
:::


### `status`

Narrows the query results based on the products’ statuses.

Possible values include:

| Value | Fetches products…
| - | -
| `'live'` _(default)_ | that are live.
| `'pending'` | that are pending (enabled with a Post Date in the future).
| `'expired'` | that are expired (enabled with an Expiry Date in the past).
| `'disabled'` | that are disabled.
| `['live', 'pending']` | that are live or pending.



::: code
```twig
{# Fetch disabled products #}
{% set products = {twig-function}
    .status('disabled')
    .all() %}
```

```php
// Fetch disabled products
$products = \craft\commerce\elements\Product::find()
    ->status('disabled')
    ->all();
```
:::


### `title`

Narrows the query results based on the products’ titles.



Possible values include:

| Value | Fetches products…
| - | -
| `'Foo'` | with a title of `Foo`.
| `'Foo*'` | with a title that begins with `Foo`.
| `'*Foo'` | with a title that ends with `Foo`.
| `'*Foo*'` | with a title that contains `Foo`.
| `'not *Foo*'` | with a title that doesn’t contain `Foo`.
| `['*Foo*', '*Bar*']` | with a title that contains `Foo` or `Bar`.
| `['not', '*Foo*', '*Bar*']` | with a title that doesn’t contain `Foo` or `Bar`.



::: code
```twig
{# Fetch products with a title that contains "Foo" #}
{% set products = craft.products()
    .title('*Foo*')
    .all() %}
```

```php
// Fetch products with a title that contains "Foo"
$products = \craft\commerce\elements\Product::find()
    ->title('*Foo*')
    ->all();
```
:::


### `trashed`

Narrows the query results to only products that have been soft-deleted.





::: code
```twig
{# Fetch trashed products #}
{% set products = craft.products()
    .trashed()
    .all() %}
```

```php
// Fetch trashed products
$products = \craft\commerce\elements\Product::find()
    ->trashed()
    ->all();
```
:::


### `type`

Narrows the query results based on the products’ types.

Possible values include:

| Value | Fetches products…
| - | -
| `'foo'` | of a type with a handle of `foo`.
| `'not foo'` | not of a type with a handle of `foo`.
| `['foo', 'bar']` | of a type with a handle of `foo` or `bar`.
| `['not', 'foo', 'bar']` | not of a type with a handle of `foo` or `bar`.
| an [ProductType](commerce3:craft\commerce\models\ProductType) object | of a type represented by the object.



::: code
```twig
{# Fetch products with a Foo product type #}
{% set products = craft.products()
    .type('foo')
    .all() %}
```

```php
// Fetch products with a Foo product type
$products = \craft\commerce\elements\Product::find()
    ->type('foo')
    ->all();
```
:::


### `typeId`

Narrows the query results based on the products’ types, per the types’ IDs.

Possible values include:

| Value | Fetches products…
| - | -
| `1` | of a type with an ID of 1.
| `'not 1'` | not of a type with an ID of 1.
| `[1, 2]` | of a type with an ID of 1 or 2.
| `['not', 1, 2]` | not of a type with an ID of 1 or 2.



::: code
```twig
{# Fetch products of the product type with an ID of 1 #}
{% set products = craft.products()
    .typeId(1)
    .all() %}
```

```php
// Fetch products of the product type with an ID of 1
$products = \craft\commerce\elements\Product::find()
    ->typeId(1)
    ->all();
```
:::


### `uid`

Narrows the query results based on the products’ UIDs.





::: code
```twig
{# Fetch the product by its UID #}
{% set product = craft.products()
    .uid('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx')
    .one() %}
```

```php
// Fetch the product by its UID
$product = \craft\commerce\elements\Product::find()
    ->uid('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx')
    ->one();
```
:::


### `unique`

Determines whether only elements with unique IDs should be returned by the query.



This should be used when querying elements from multiple sites at the same time, if “duplicate” results is not
desired.



::: code
```twig
{# Fetch unique products across all sites #}
{% set products = craft.products()
    .site('*')
    .unique()
    .all() %}
```

```php
// Fetch unique products across all sites
$products = \craft\commerce\elements\Product::find()
    ->site('*')
    ->unique()
    ->all();
```
:::


### `uri`

Narrows the query results based on the products’ URIs.



Possible values include:

| Value | Fetches products…
| - | -
| `'foo'` | with a URI of `foo`.
| `'foo*'` | with a URI that begins with `foo`.
| `'*foo'` | with a URI that ends with `foo`.
| `'*foo*'` | with a URI that contains `foo`.
| `'not *foo*'` | with a URI that doesn’t contain `foo`.
| `['*foo*', '*bar*']` | with a URI that contains `foo` or `bar`.
| `['not', '*foo*', '*bar*']` | with a URI that doesn’t contain `foo` or `bar`.



::: code
```twig
{# Get the requested URI #}
{% set requestedUri = craft.app.request.getPathInfo() %}

{# Fetch the product with that URI #}
{% set product = craft.products()
    .uri(requestedUri|literal)
    .one() %}
```

```php
// Get the requested URI
$requestedUri = \Craft::$app->request->getPathInfo();

// Fetch the product with that URI
$product = \craft\commerce\elements\Product::find()
    ->uri(\craft\helpers\Db::escapeParam($requestedUri))
    ->one();
```
:::


### `with`

Causes the query to return matching products eager-loaded with related elements.



See [Eager-Loading Elements](https://craftcms.com/docs/3.x/dev/eager-loading-elements.html) for a full explanation of how to work with this parameter.



::: code
```twig
{# Fetch products eager-loaded with the "Related" field’s relations #}
{% set products = craft.products()
    .with(['related'])
    .all() %}
```

```php
// Fetch products eager-loaded with the "Related" field’s relations
$products = \craft\commerce\elements\Product::find()
    ->with(['related'])
    ->all();
```
:::



<!-- END PARAMS -->
