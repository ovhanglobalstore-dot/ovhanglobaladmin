OVHAN GLOBAL ADMIN WEBSITE

README and setup guide

1. Overview

This package contains only the Ovhan Global admin website and this README.
The admin website connects to the Supabase project configured in its HTML
file and provides an interface for managing products and inventory.

The updated interface includes:
- Product catalogue actions for editing, adjusting stock and deleting products.
- Clickable product thumbnails that open a larger image.
- Category options matching the marketplace categories.
- Automatic SKU generation when a new product is created.
- An email-confirmation check before an administrator can access the dashboard.

2. Files in this ZIP

- index.html — the Ovhan Global admin website.
- README.txt — this guide.

No SQL file is included. We will review and run the required Supabase SQL
together separately. Do not run any SQL based only on this package.

3. Supabase and administrator access

The website uses the Supabase project URL and publishable key configured in
index.html. The database tables, functions and security policies must be
prepared in Supabase before the admin functions can work correctly. We will
handle and verify that SQL setup together.

The admin website checks the signed-in user's ID against public.admin_users.
The administrator account must be email-confirmed and authorized in the admin
allow-list before dashboard access is granted. We will confirm the exact
allow-list setup when we run through the SQL together.

4. Email verification

The HTML checks the signed-in user's email_confirmed_at value. Supabase must
also have email confirmation enabled:

1. Open the correct project in the Supabase Dashboard.
2. Go to Authentication → Providers → Email (menu wording may vary).
3. Enable Confirm email and save the setting.
4. Ensure email delivery is configured.
5. Test registration and verification before using the admin account.

The HTML check alone does not enable Supabase email confirmation.

5. Hosting

Host index.html on an HTTPS website, or use localhost during development.
Configure the Supabase site's allowed redirect URLs as required by your
authentication setup. Avoid publishing the admin page over unsecured HTTP.

6. Using the admin website

- Products: add products, edit existing details, adjust stock or delete products.
- Product images: select a product thumbnail to view the larger image.
- Categories: Fashion, Accessories, Industry and machine, Electronic, Baby
  and toddler, Sports and travel, Home living, Groceries and food, and Toys,
  games and hobbies.
- SKU: a SKU is generated automatically for a new product; existing SKUs are
  retained when editing.

7. Security and testing

- Keep database access protected by correctly configured Supabase Row Level
  Security (RLS) policies.
- Never put a Supabase service_role key or other secret key in this HTML file.
- Restrict administrator access to trusted accounts.
- Use strong passwords and enable multi-factor authentication where available.
- Before production use, test sign-in, product creation, editing, stock changes,
  deletion, and image viewing with non-critical data.

Important: this package does not execute SQL or change Supabase settings. We
will complete the database setup together and verify the results before you
rely on the admin website for live inventory management.

Ovhan Global | Admin website documentation
