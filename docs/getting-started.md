# Getting Started

## Logging in

Open the Ledgyx Admin URL in your browser. You can sign in with:
- **Email and password** — enter your credentials on the login screen
- **Google** — click "Continue with Google"
- **GitHub** — click "Continue with GitHub"

<p align="center">
  <img src="../../images/login.png" alt="Login page" width="90%">
</p>

---

## Selecting a configuration

After logging in you'll land on the Home page. Before you can do anything useful, you need to **select an active configuration**.

A configuration is your tenant workspace — all events, entities, API endpoints, templates, and agents belong to a specific configuration. You can have multiple configurations (for different projects, clients, or environments).

**To select a configuration:**
1. Go to **Settings** (bottom of the left sidebar)
2. Click the **Configurations** tab (it's the default)
3. Click **Activate** on the configuration card you want to work with — it gets a violet border and an "Active" badge

<p align="center">
  <img src="../../images/settings-configurations.png" alt="Configurations tab — active config" width="90%">
</p>

Once a configuration is active, its name appears in the header. You can click it anytime to go back to Settings and switch.

> **Note:** Without an active configuration, data pages like Playground, Events, and Entities will show empty states.

---

## Navigating the interface

The **left sidebar** gives you access to all sections. The order matches the typical workflow:

1. Start with **Settings** to pick your configuration
2. Use **Playground** to explore your data model
3. Build **Events** (your SQL handlers)
4. Expose them through **API** (endpoint groups)
5. Design your **Entities** (data schema)
6. Create **Templates** for HTML pages
7. Set up the **Sitemap** to route URLs
8. Configure **Subscriptions** and **Webhooks** for automations
9. Upload assets in **File Manager**
10. Build AI workflows in **AI Agents**

The **header** shows:
- The active configuration name (click to go to Settings)
- The AI Builder chat button (Bot icon — opens the chat panel)
- Your profile menu (Settings / Log out)

---

## Your first workflow

Here's a typical flow for setting up a simple data-driven API:

### 1. Create an entity (data model)

Go to **Entities**, create a new entity (e.g. `Product` of type Dictionary), and add fields: `title` (String), `price` (Number), `description` (String).

### 2. Write event handlers

Go to **Events**, create a new event `products/list`. Add a GET method with a SQL query that returns your products.

### 3. Expose as an API endpoint

Go to **API**, create a group `products`, then add an endpoint `list` linked to your `products/list` event. Your API is now live at `/rest/v2/{api-key}/products/list`.

### 4. Test it

Use the **Test** button in API Builder — fill in headers, method, and body; click Send; see the response inline.

---

## Tips

- **Config badge in the header** is always visible — red means no config selected, violet means active.
- **AI Builder** (Bot icon in the header) can build an entire project for you through a chat conversation — entity + events + endpoints + templates + sitemap in one go.
- **Versions** let you stage event changes before pushing to production — useful for teams.
- **Playground** is your sandbox — run any SQL query against your data model without touching production.
