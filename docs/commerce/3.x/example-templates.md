# Example Templates

Commerce provides [example templates](https://github.com/craftcms/commerce/tree/master/example-templates) for you to reference when building out your commerce solution.

You’ll find these in the `vendor/craftcms/commerce/example-templates/` folder, and you may want to copy them to your project’s top level `templates/` folder:

```bash
cp -r vendor/craftcms/commerce/example-templates/* ./templates
```

If your system supports it, you could also symlink these folders into your project’s `templates/` folder so you always have up-to-date examples _while in development_:

```bash
ln -s vendor/craftcms/commerce/example-templates/shop ./templates/shop
ln -s vendor/craftcms/commerce/example-templates/buy ./templates/buy
```

The two main sets of templates are in the `shop` and `buy` folders.

## Shop

The `shop` folder contains a robust set of templates that do pretty much everything with Commerce. This includes a full checkout flow, address handling, order management, subscription management, and cart management.

Copy or symlink the `shop` folder to your templates and navigate your browser to `yoursite.test/shop` to see them in action.

## Buy

The `buy` folder contains a simple set of templates that work well for Commerce Lite, where you would only have a “buy now” button and a simple checkout.

Copy or symlink the `buy` folder to your templates and navigate your browser to `yoursite.test/buy` to see it in action.
