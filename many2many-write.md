Write values to a many2many field
---

`many2many` fields represent a relation between two models `foo` and `bar` where `foo` can be in
relation with many `bar` and inversely. When you write to a `many2many` field for a particular
record, you're editing a list of IDs.

This list of IDs is manipulated by sending a list of tuples, each containing an action (a number
between 1 and 6) and the arguments for that action. This is the list of possible actions:

    (0, 0,  { values }) # Create relation to new record
    (1, ID, { values }) # Update values of existing relation
    (2, ID)             # Delete relation and linked record
    (3, ID)             # Remove link and keep record
    (4, ID)             # Link to an existing record
    (5)                 # Remove all links, but keep records
    (6, 0, [IDs])       # Replace all links with the supplied list of IDs

For example, if we have a `foo` and want to create a new `bar` (`bar_ids` being our many2many field)
and link to it, we'd do:

```python
vals = {'name': "Name for our new bar object"}
foo_obj.write(cr, uid, foo_id, {'bar_ids': [(1, 0, vals)]}, context=context)
```

Another example: take all the ids from another `foo` record and have the same relations in another
`foo` record:

```python
other_foo = foo_obj.browse(cr, uid, other_foo_id, context=context)
wanted_bar_ids = [bar_id.id for bar_id in other_foo.bar_ids]
foo_obj.write(cr, uid, foo_id, {'bar_ids': [(6, 0, wanted_bar_ids)]}, context=context)
```
