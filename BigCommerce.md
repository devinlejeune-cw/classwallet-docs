# BigCommerce Vendor Integration Guide

### Create the API account that will allow the ClassWallet service to be connected. 
- Go to settings → Store-level API Accounts
- Click “Create API account”
- Name: ClassWallet Connector
- Token Type: V2/V3 API token
- Select the following OAuth scopes

| Scope | Permission |
| :---- | :----: |
|Content| Modify|
|Checkout Content| Modify|
|Customers|Modify|
|Customers Login|Login|
|Information & Settings|Read-only|
|Marketing|None|
|Orders|Modify|
|Order Transactions|Modify|
|Create Payments|None|
|Get Payment Methods|None|
|Stored Payment Instruments|None
|Products|Read-only|
|Themes|None
|Carts|Modify|
|Checkouts|Modify|
|Sites & Routes|Read-only|
|Channel Settings|Read-only|
|Channel Listings|Read-only|
|Storefront API Tokens|Manage| 
|Storefront API Customer impersonation tokens|None|
|Store Logs|None|
|Store Locations|None|
|Store Inventory|Read-only|
|Fulfillment Methods|Read-only|
|Order Fulfillment|None|
|Metafields Ownership|Manage|
|Metafields Access|Full|
|Store Translations|None|

Click save and provide the credentials to ClassWallet 

---

# BigCommerce & ClassWallet Integration Guide

This guide outlines the end-to-end process of how your BigCommerce store integrates with ClassWallet, from the initial technical setup to the final order fulfillment.

>  Our integration is **Storefront Aware** and fully compatible with BigCommerce's **Multi-Storefront (MSF)** architecture, enabling functionality across multiple storefronts if desired.


## 1. Initial Setup
Once you provide ClassWallet with your BigCommerce API credentials, our team handles the plugin initialization. This process introduces two key technical components to your store:
* **Hidden Webpage:** We generate a dedicated, hidden page on your site designed to securely capture shopper details when a ClassWallet session begins.
* **Checkout Script:** We add a script that loads during the checkout process. This identifies active ClassWallet sessions, hides alternative payment methods, and displays a customized **Checkout with ClassWallet** button.

## 2. Store Entry & Authentication
The shopper's journey begins in the ClassWallet Marketplace rather than directly on your homepage.
* **Redirection:** When a shopper clicks **Shop** on your store's ClassWallet tile, they are routed to the hidden page on your BigCommerce site.
* **Data Capture:** A JavaScript snippet on this hidden page retrieves the shopper's information directly from ClassWallet.
* **Auto-Login & Account Creation:** The system checks for an existing store account. If one exists, the shopper is automatically logged in. If not, an account is instantly created for them, followed by an automatic login.

> **Routing Customization:** By default, shoppers are redirected to `/account.php` immediately after they are logged in. If you prefer to redirect them to your home page or a specific landing page, let our team know and we can update the desired destination.

## 3. The Checkout Experience
Shoppers browse and add items to their cart just like standard customers. When ready, they proceed to your native BigCommerce checkout.
* **Streamlined UI:** Our script recognizes the ClassWallet shopper and automatically hides any non-ClassWallet checkout options (like credit card or PayPal buttons).
* **Standard Calculations:** Shipping and tax are calculated normally based on your BigCommerce settings.
* **Payment Step:** At the final payment stage, the shopper is presented exclusively with the ClassWallet payment method and a **Proceed to ClassWallet** button.

## 4. Order Hand-off & Approval
Clicking the final checkout button ends the native 'BigCommerce Checkout' flow.
* **Order Staging:** When the shopper clicks **Proceed to ClassWallet**, an **Incomplete Order** is generated in your BigCommerce backend and the Shopper is routed to ClassWallet Checkout.
* **Data Transfer:** We retrieve the order contents, shipping costs, and tax data from this incomplete order to populate the ClassWallet checkout.
* **Fund Allocation:** The shopper allocates their required funds within ClassWallet. The order then enters an **Awaiting Approval** state in the ClassWallet system.

## 5. Finalization & Fulfillment
Once the order receives official approval within ClassWallet, the integration updates your store automatically.
* **Status Update:** The incomplete order in BigCommerce is converted to a completed order and shifted to the **Awaiting Fulfillment** status.
* **Record Keeping:** The official ClassWallet PO Number is automatically added to the BigCommerce order as the Transaction ID.
* **Fulfillment:** Your team can now proceed with picking, packing, and shipping the order using your standard fulfillment workflow.

---

## 6. Important Considerations

* **Inventory & Product Availability During Approval:** Because there is a waiting period while the order is **Awaiting Approval** in ClassWallet, the inventory is not permanently secured immediately. If any products within the incomplete order run out of stock, are disabled, or are removed from your store during this approval window, the integration **will fail** to convert the incomplete order into a completed order once approved.
