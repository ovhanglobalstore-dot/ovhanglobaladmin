OVHAN GLOBAL ADMIN — ADVERTISEMENT + CUSTOMER NOTIFICATIONS UPDATE

This package preserves the recent private admin website features: administrator login, product catalogue, image uploads, size-level stock management, stock alerts, inventory history, order management and slideshow advertisement management.

NEW: CUSTOMER NOTIFICATIONS
- A Customer notifications section lets authorized administrators send a title and message to customer accounts.
- Customers who have opted in can receive announcements while signed in to the marketplace.
- An order-status database trigger creates a notification when an administrator changes an order's status.
- Existing admin browser stock alerts remain separate and unchanged.

DATABASE SETUP
1. Back up the currently deployed admin and customer websites.
2. Open the existing Supabase project's SQL Editor.
3. Run Advertisements_Setup.sql from this package. It includes the existing advertisement setup plus notification tables, policies, RPC and order-status trigger.
   Alternatively, if advertisement setup has already been run, run Notifications_Setup.sql only.
4. Deploy index.html and logo-cart-plane.png together to the private admin website.
5. Deploy the matching customer index.html package.
6. Sign in as an authorized admin, open Customer notifications and send a test announcement.
7. Sign in to a customer account, open Settings, enable Notifications and allow browser permission. Keep the marketplace open to receive live notifications.
8. Change a test order's status in admin and confirm the customer receives an order update.

SECURITY
The announcement RPC checks public.is_ovhan_admin(). Customer notification rows have row-level security. Do not add a Supabase service-role key to either website. Admin login remains restricted to verified users in admin_users.

IMPORTANT LIMITATION
Notifications are delivered live while the customer marketplace is open and the customer is signed in. Background push delivery while the browser is closed is not included; it requires a service worker and a push subscription/server delivery service.

TEST STATUS
Both HTML JavaScript blocks passed Node.js syntax checks. The SQL has not been run and the features have not been tested against the live Supabase project or deployed websites.
