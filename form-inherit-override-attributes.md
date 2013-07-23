Override element attributes in inherited forms
---

You inherit the form for `purchase.order` and you'd like every line with a `0` quantity to be red.
You could use the `replace` position on the order lines' `<tree>`, but by doing so you'd have to
redefine all of the tree's sub-elements. Did you know that there's an `attributes` position? Well,
now you do! With this position type, you can do what you want with:

```xml
<xpath expr="//field[@name='order_line']/tree" position="attributes">
    <attribute name="colors">red:product_qty==0</attribute>
</xpath>
```

Note: Don't forget to do xml escaping when appropriate (`> == &gt; < == &lt;`).
