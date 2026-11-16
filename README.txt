OVHAN GLOBAL — JUMIA-INSPIRED RESPONSIVE ADMIN

PACKAGE CONTENTS
- index.html: administrator dashboard.
- Ovhan_Global_Size_Stock_Orders_Setup.sql: one-time Supabase migration and order-stock automation.
- README.txt: installation and testing notes.

DESIGN
- Desktop keeps the permanent left-hand sidebar and wide management layout.
- Phones use a top-bar menu button that opens a vertical dropdown navigation panel. The desktop sidebar is not changed.
- Layouts, product forms, stock controls, orders and tables adapt to small screens and touch input.
- Uses Ovhan Global's navy, orange and white brand identity, matching the updated Jumia-inspired customer storefront without copying Jumia branding.

PRESERVED ADMIN FEATURES
- Supabase email/password sign-in and admin_users authorization.
- Dashboard totals and recent inventory movements.
- Product add, edit, search, category filter and delete.
- Product image upload to the existing product-images Storage bucket, preview and full-size view.
- Manual restock/stock adjustment and inventory movement history.
- Customer order list and order status updates.
- Realtime refresh when products, inventory movements or orders change. The setup SQL also adds these tables to the supabase_realtime publication where available.

SIZE-LEVEL INVENTORY
- Enable “Track stock separately by size” when adding or editing clothing, shoes or other variants.
- Add each size/variant and its own stock quantity. Example: S=4, M=8, L=2.
- Total stock is calculated from the size rows and saved in products.stock_quantity.
- Products without variants continue using the normal single stock quantity field.
- Restocking a size updates the product total and records an inventory movement.

REQUIRED SUPABASE SETUP — IMPORTANT
1. Open the Supabase project used by the customer website.
2. Open SQL Editor.
3. Run the complete file: Ovhan_Global_Size_Stock_Orders_Setup.sql.
4. Confirm the query succeeds before adding size-based products or testing checkout.

The SQL adds products.sizes (JSONB), a private order reservation ledger and a database trigger. The trigger reads the existing customer checkout's orders.items array. Each item includes product_id, quantity and selected_size.

AUTOMATIC STOCK BEHAVIOUR
- When a customer submits an order, the database checks available stock and reserves the quantity in the same transaction as the order insert.
- A size item reduces that exact size's stock and recalculates total stock. A non-size item reduces the normal product stock.
- If stock is insufficient, the order insert is rejected; it cannot silently oversell.
- If an order status is changed to “cancelled”, its reserved stock is returned automatically.
- If a cancelled order is reactivated, stock is reserved again, subject to availability.
- Admin stock edits/restocks update products. Supabase Realtime refreshes the dashboard and stock totals automatically.
- Orders placed before the trigger is installed are not deducted retroactively.

CUSTOMER WEBSITE COMPANION
Use the companion customer package supplied with this admin update. It adds the products.sizes field to the catalogue read and shows available quantity beside each size, disabling sizes with zero stock. Keep the admin and customer versions paired so size stock is visible and order items carry the selected size.

LOW-STOCK NOTIFICATIONS
- The alert threshold is 5 units (or fewer), based on total product stock and on each individually tracked size. A product is flagged if its total or any size reaches the threshold.
- Current low/out-of-stock items remain visible in Stock alerts after sign-in.
- Press “Enable alerts” and allow notifications in the browser. While the admin page is open and signed in, new drops to the threshold produce an in-app alert and a browser notification on supported devices.
- Notifications require HTTPS (Cloudflare Pages is HTTPS) and browser permission.
- These are foreground browser notifications, not guaranteed background push or email alerts when the browser is closed. Email/background push requires a separately configured notification provider or Supabase Edge Function and credentials; none are embedded in this ZIP.

DATABASE SCHEMA USED
- products: id bigint, name, description, category, price_ugx, price_usd (optional), image_url, stock_quantity, is_active, updated_at, updated_by, and new sizes JSONB.
- inventory_movements: product_id bigint, change_quantity, reason, created_at.
- admin_users: user_id uuid, is_active, role.
- orders: id, created_at, status, subtotal, currency, customer_name, customer_phone, items JSON/JSONB.

SECURITY
- The HTML uses the existing Supabase project URL and browser publishable key. A publishable key is not a service secret; Row Level Security remains the security boundary.
- Never put a service_role key in browser code.
- The SQL migration creates its reservation ledger with RLS enabled and denies direct anon/authenticated access. Its trigger functions run with database-owner privileges.
- Keep administrator-only product and order policies enabled. Do not make inventory tables publicly writable.

TEST PLAN
1. Run the SQL migration.
2. Sign in with the verified administrator account already authorized in admin_users.
3. Add a test clothing product with S=2, M=3, L=1. Confirm total stock is 6.
4. Open the paired customer site and confirm the sizes and quantities display.
5. Submit a test order for one M. Confirm M becomes 2 and total stock becomes 5 in the admin dashboard.
6. Try ordering more than the remaining size stock; the database should reject the order.
7. Cancel the test order in admin. Confirm the reserved size stock is returned.
8. Restock a size in admin and confirm the customer site/admin totals refresh.
9. Enable browser notifications, then adjust a product from above 5 units to 5 or fewer.
10. Test desktop and phone layouts, including the phone dropdown navigation.

IMPORTANT ORDER NOTE
The customer checkout currently creates an order request and opens WhatsApp for follow-up; it does not collect online payment. This setup reserves stock when that order request is submitted. Cancel the order to release the reservation if the customer does not proceed.

Ovhan Global | SHOP THE WORLD
