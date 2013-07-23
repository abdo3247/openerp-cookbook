Show the field of a many2one relation in a view
---

You're inheriting the Purchase Order view and you want to add the supplier's fax number right under
the supplier's selector. You think that it should be an easy task, that all you have to do is to add
a field with the name `partner_id.fax` (to indicate that you want to display the `fax` field of the
`res.partner` relation). Nope, sorry, it's a bit more complicated.

You have to add a `related` field to `purchase.order`, which you'll inherit. A related field is a
field that it fetched from an existing relation. In the situation that interests us here, we'd add
our fax field like this:

```python
class purchase_order(orm.Model):
    _inherit = 'purchase.order'
    _columns = {
        'partner_fax': fields.related('partner_id', 'fax', type='char', string='Fax', readonly=True),
    }
```

What this does is that it "fetches" the `fax` field from the partner that is currently linked to
the record and then puts it in `purchase.order`'s `partner_fax` field. When you inherit
`purchase.order`'s form view, you can then add this field with a simple:

```xml
<field name="partner_fax"/>
```
