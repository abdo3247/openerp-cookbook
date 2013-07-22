Show a form in "create" mode with pre-filled values
---

Let's say that you want to add a button in the `product.product` form to quickly let you create a
`purchase.order` with this product as its first line. You could create the purchase order right
when the button is clicked, but that wouldn't allow for an easy cancellation of the action: The user
would have to delete the purchase order if the click of the button turned out to be a mistake.

Besides, you also need to ask the user for a supplier for the PO before you create the record, so
you can't actually create the PO without setting a default supplier, which would be kind of hackish.

What you'd like to do is to create the purchase order after the user edited a pre-filled purchase
order and clicked Save. To do that, you can return a `ir.actions.act_window` with no `res_id` and
the prefill values in `context`.

```python
def action_purchase_order_from_product(self, cr, uid, ids, context=None):
    product_id = ids[0]
    context = {
        'default_notes': "Generated from my super button!",
        'default_order_lines': [(0, 0, {'product_id': product_id, 'product_qty': 1})],   
    }
    result = {
        'type': 'ir.actions.act_window',
        'name': 'Purchase Orders',
        'res_model': 'purchase.order',
        'view_type': 'form',
        'view_mode': 'form',
        'view_id': False,
        'target': 'current',
        'context': context,
    }
```

As you can see, we prefix our prefill field names in `context` by `default_`. That's to avoid name
clash with other names used in the context dict. The `default_order_lines` value is a list of tuples
because that field is a [many2many](many2many-write.md).
