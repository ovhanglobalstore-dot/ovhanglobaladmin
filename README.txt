OVHAN GLOBAL — ADMIN WEBSITE

1. Package contents
- index.html: separate administrator website.
- README.txt: setup, schema compatibility and usage notes.

2. Supabase connection
The website connects to the Ovhan Global Supabase project configured in
index.html using the project's publishable key. A publishable key is intended
for browser use; database access must remain protected by Supabase RLS.

The product and inventory code is aligned with the verified schema:
- public.products: id, name, description, category, price_ugx, price_usd,
  image_url, stock_quantity, is_active, created_at, updated_at, updated_by
- public.inventory_movements: id, product_id, change_quantity, reason,
  created_at
- public.admin_users: user_id, created_at, is_active, role

The admin website writes product prices to price_ugx, reads product images
from image_url, and records stock changes in inventory_movements.
Product IDs are displayed instead of SKUs because the verified products
schema does not contain a sku column.

The low-stock alert threshold is currently 5 units in the website. It is a
display rule and is not stored per product because the verified products
schema does not contain a reorder_level column.

3. Administrator access
There is no public admin sign-up. Sign in with an existing Supabase Auth
account whose email is confirmed and whose auth user ID has an active entry
in public.admin_users. The website checks user_id and is_active before
showing the admin dashboard. RLS policies and the public.is_ovhan_admin()
function must remain enabled and correctly configured in Supabase.

4. Product images
Images are uploaded to the Supabase Storage bucket named product-images.
The public image URL is saved in products.image_url. The bucket and its
Storage policies must allow the intended authenticated administrator upload.
The database schema does not include an image_path column, so the website
does not attempt to store or delete storage object paths.

5. Orders
The admin page includes an orders panel that attempts to read the existing
public.orders table and update its status. The precise orders table columns
and update policies were not part of the verified products/inventory schema
used for this compatibility update. If the orders panel reports an error,
the orders query/status update must be matched to the actual orders schema
before relying on it.

6. Hosting and use
Host index.html over HTTPS, or test on localhost. Configure Supabase Auth
redirect URLs for the hosted admin URL where required.

- Dashboard: product and stock totals, plus recent stock movements.
- Products & inventory: add, edit, adjust stock, delete and search products.
- Product images: tap a thumbnail to view the full image.
- Stock alerts: products at or below 5 units.
- Inventory history: recorded stock changes.
- Orders: view/update orders if the existing orders schema and RLS policies
  support the fields used by the page.

7. Security and production checks
- Never place a service_role key or other secret key in index.html.
- Keep RLS enabled and retain administrator-only policies.
- Use a strong password and multi-factor authentication where available.
- Test sign-in, product creation, editing, stock adjustments, image uploads,
  and deletion using non-critical test data before production use.

This package changes the admin HTML only. It does not execute SQL or alter
Supabase settings, policies, functions, or Storage configuration.

Ovhan Global | Admin website documentation
