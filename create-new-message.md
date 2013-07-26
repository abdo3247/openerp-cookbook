Create a new message programmatically
---

Most models in OpenERP have discussion threads attached to them. That's because they inherit from
`mail.thread`. Programmatically attaching a new message to one of such models is easy, it's only
a matter of finding which method to call, which you'll learn right away: it's
`mail_thread.message_post()`.

Because your model inherits from `mail.thread`, you can call that method directly from it. Here's an
example snippet attaching a new message to a Sale Order:

```python
sale_order = self.pool.get('sale.order')
sale_order.message_post(cr, uid, my_sale_order_id, subject="My Subject", body="Message Body")
```
