Write values to a many2many field
---

`many2many` fields represent a relation between two models, for example `res.partner` and `res.partner.category` where `res.partner` can be in
relation with many `res.partner.category` and inversely. When you write to a `many2many` field for a particular
record, you're editing a list of IDs.

This list of IDs is manipulated by sending a list of tuples, each containing an action (a number
between 0 and 6) and the arguments for that action. This is the list of possible actions:

    (0, 0,  { values }) # Create relation to new record
    (1, ID, { values }) # Update values of existing relation
    (2, ID)             # Delete relation and linked record
    (3, ID)             # Remove link and keep record
    (4, ID)             # Link to an existing record
    (5)                 # Remove all links, but keep records
    (6, 0, [IDs])       # Replace all links with the supplied list of IDs

For example, if we want to assign a new `res.partner.category` (which we create) to a `res.partner`,
we'd do:

```python
res_partner = self.pool.get('res.partner')
vals = {'name': "My Category"}
# Despite its name, category_id really is a many2many, not a many2one.
res_partner.write(cr, uid, partner_id, {'category_id': [(0, 0, vals)]}, context=context)
```

Another example: take all the categories from some partner and have them applied to a another
partner:

```python
res_partner = self.pool.get('res.partner')
other_partner = res_partner.browse(cr, uid, other_partner_id, context=context)
# It's numerical IDs we need, not browse_record() wrappers
wanted_category_ids = [category_id.id for category_id in other_partner.category_id]
vals = {'category_id': [(6, 0, wanted_category_ids)]}
res_partner.write(cr, uid, other_partner_id, vals, context=context)
```
