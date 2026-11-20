OVHAN GLOBAL ADMIN — ADVERTISEMENTS + CUSTOMER NOTIFICATIONS

This package contains the private admin website update with its existing administrator login, product catalogue, image uploads, size-level stock management, stock alerts, inventory history, order management and slideshow advertisement manager.

CUSTOMER NOTIFICATIONS
- Admins can publish a title and message from Customer notifications.
- The announcement action calls public.publish_ovhan_announcement(p_title, p_body).
- Order status changes create a notification using orders.user_id and orders.status.
- Customers who have disabled notifications are excluded from new announcements and automatic order-status alerts.
- Customer notifications are stored in the existing public.customer_notifications table.

DATABASE STATUS
The compatible notification SQL and advertisement SQL have already been run successfully in the Supabase project. Notifications_Setup.sql is included for reference/recovery and matches the existing customer_notifications and notification_preferences tables. Do not run an older Notifications_Setup.sql from a different package that creates user_notifications/read_at/body; that schema is not used by this update.

DEPLOYMENT
1. Back up the currently deployed admin website.
2. Deploy index.html and logo-cart-plane.png together to the private admin website.
3. Deploy the matching Ovhan_Global_Customer_Notifications_Update package to the customer website.
4. Sign in as an authorised admin, open Customer notifications and send a test announcement.
5. Sign in to a customer account, open Settings, and enable Notifications. The preference is saved to Supabase.
6. Keep the customer marketplace open and signed in to receive live updates. Change a test order status to verify the automatic order notification.

LIMITATION
Live notifications work while the customer marketplace is open and signed in. Background push delivery while the browser is closed is not included; that requires a service worker and push subscription/server delivery.

SECURITY
The announcement function checks public.is_ovhan_admin(). No service-role key is used in either website. Keep the admin site private and do not place a Supabase service-role key in browser code.

TEST STATUS
The source has been updated to use the confirmed database column names. This package has not yet been deployed or tested against the live website/browser session.
