# Product Model

Whenever you’re dealing with an product in your template, you’re actually working with an `Commerce_ProductModel` object.

## Simple Output

Outputting an `Commerce_ProductModel` object in your template without attaching a property or method will return the product’s name:

```
<h1>{{ product }}</h1>
```
ProductModel objects have the following attributes and methods:

## Attributes

### title

The products name/title.

### type

The product’s product type.

### typeId

The product’s product type ID.

### status

`live`, `pending` or `expired` based on `postDate` and `expiryDate` dates. Pending are products
with a future `postDate` date.

### enabled

true or false

### taxCategory

The tax category all variants of this product use when their tax calculations are made.

### promotable

true or false.
Is this product and its variants able to be on sale or at a discount.

### freeShipping

true or false.

Should the shipping calculator skip this product and its variants when adding costs to the order.

This flag only works on shipping cost that is `per item` or `weight based`. Any order level base shipping costs in a shipping rule will be added to the order regardless of this checkbox.

### postDate

The date this product is available for sale.

### expiryDate

The date this product will no longer be available for sale.

### totalStock

The total stock of all variants. Will show zero if all variants are set to unlimited quantity.

### unlimitedStock

Does one or more variant have unlimited stock.

### cpEditUrl

The URL to edit this product.

### variants

Returns an array of [Variant models](variant-model.md).
Gets all variants that are for sale with any applicable [sales](sales.md) applied to them.
Only returns an array with a single variant if the product’s type has not been set to contain multiple variants.

### defaultVariant

Returns the default [Variant model](variant-model.md). If no variant is set as the default, it returns the first variant found. For product types without multiple variants, this is the product’s implicit variant.

## The default variant helpers

Instead of calling `{{product.defaultVariant.id}}` which could perform a database query to get the products default variant, we cache the default variant when saving the product, to the product itself. You can use the following attributes to get the default variant’s information:

### defaultVariantId

### defaultSku
### defaultPrice

### defaultHeight
### defaultLength

### defaultWidth
### defaultWeight
