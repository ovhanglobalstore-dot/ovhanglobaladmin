OVHAN GLOBAL ADMIN — ADVERTISEMENT MANAGEMENT UPDATE

This package is based on the recent private admin application and preserves its existing administrator login, Supabase connection, product catalogue, product image upload, size-level stock management, stock alerts, inventory history and order management.

NEW: ADVERTISEMENT MANAGEMENT
- Add and edit homepage slideshow advertisements.
- Upload a banner image, heading, description, button label and destination link.
- Set display order, publish/unpublish status, and optional start/end dates.
- Delete advertisements.
- Published advertisements are read by the customer storefront slideshow.

IMPORTANT DATABASE SETUP
1. Back up the currently deployed admin and customer websites.
2. In the existing Supabase project, open SQL Editor and run Advertisements_Setup.sql once.
3. This creates public.advertisements, its row-level security policies, and the public advertisement-images storage bucket with administrator-only upload/update/delete access.
4. Deploy the admin index.html and logo-cart-plane.png together to the private admin site.
5. Deploy the paired customer index.html to the customer website so it loads the advertisements from Supabase.
6. Sign in to the admin site, open Advertisements, add an image and publish it. Check the customer homepage slideshow.

The SQL uses the existing public.is_ovhan_admin() authorization function. Do not put a Supabase service-role key in either website. Existing admin login remains restricted to verified users listed in admin_users.

TEST STATUS
Both HTML JavaScript blocks passed Node.js syntax checks. The changes have not been tested against the live Supabase project or deployed websites. Run the SQL first, then test adding, publishing, unpublishing and deleting a test advertisement.
