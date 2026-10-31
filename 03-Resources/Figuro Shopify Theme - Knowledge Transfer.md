# Figuro Shopify Theme — Knowledge Transfer

- **Site:** https://getfiguro.com
- **Base theme:** Wokiee 3.0.0 (Envato, activation key required)
- **Repo:** https://github.com/Nirmhjejeuirueuh/figuro-shopify-theme.git (main branch)
- **Prepared by:** Sasidu Rajapaksha
- **Date:** 2026/09/14

## Overview

Figuro is a custom 3D figurine e-commerce store built on Shopify, using the Wokiee 3.0 theme (Envato marketplace theme, requires an activation key tied to the Envato purchase) as the base, heavily customized with Figuro-specific sections, blocks, and snippets.

## Theme Structure

```text
root/
├─ assets/      : JS/CSS/images. Notable custom JS: chat-widget.js, compare-page.js, personalize-order-popup.js
├─ blocks/      : Reusable content blocks for specific sections like main-product, Featured product, etc.
├─ config/      : settings_schema.json, settings_data.json, markets.json
├─ extensions/  : empty
├─ layout/      : theme.liquid (main), ecom.liquid, password.liquid
├─ locales/     : Multi-language support (en, ar, de, da, etc.) for theme local contents
├─ sections/    : Wokiee's stock sections + Figuro-specific ones (prefixed figuro-/custom-*, e.g. hero slideshow, customizer, gallery, FAQ, why-us, comparison)
├─ snippets/    : Shared partials. Figuro-specific: figuro-addons, figuro-login-popup, figuro-price-discount, figuro-scarcity-banner
└─ templates/   : Page/product/collection/blog templates
```

## Pulling the Theme from Shopify

**Prerequisite:** Shopify CLI installed (`npm install -g @shopify/cli @shopify/theme`)

1. Open a terminal and navigate to where you want the theme folder.
2. Log in and pull the theme: `shopify theme pull --store=figuro-5.myshopify.com`
3. If pulling a specific theme (not the live one), add the theme ID: `shopify theme pull --store=figuro-5.myshopify.com --theme=<theme-id>` (Find the theme ID in Admin → Online Store → Themes → ⋯ → Edit code, it's in the URL)
4. You'll be prompted to log in via browser and select the store if not already authenticated.
5. This pulls all theme files (sections, templates, config, etc.) into your local folder — compare against the GitHub repo before committing, in case admin-side edits haven't been pushed back to source control yet.

## Important Sections

### snippets/snippet-image-chat-section.liquid

The custom "Figuro Design Studio" chat + photo-upload widget shown on product pages. It lets customers upload reference photos, generates an AI preview of the figurine, and doubles as a live chat interface with human agent takeover.

**IIFE wrapper**

The whole script is wrapped in a self-invoking function:

```js
(function() {
  if (window.figuroUploaderInitLiquid) return;
  window.figuroUploaderInitLiquid = true;
  ...
})();
```

This does two things: it keeps every variable/function private to this snippet (no global namespace pollution across sections), and the `window.figuroUploaderInitLiquid` flag guards against the script re-initializing if the snippet is accidentally rendered more than once on a page (e.g. included in both a section and a template).

**How photo upload → line item properties works**

Four hidden inputs (drive url 1, 2, 3, AI image url) sit in the DOM, tied to Shopify's line-item property naming convention. When a customer uploads a photo, it's POSTed to an AWS Lambda endpoint (API_URL), which returns a Google Drive URL. That URL gets written into the next available drive-url-N input.

These inputs live inside the chat widget, not inside the product's `<form action="/cart/add">`. A `teleport()` function runs on load, on visibility change, and on a retry schedule (500ms/1s/2s/3s) to find the actual cart form and clone the hidden inputs into it, so their values get submitted along with the product when "Add to Cart" is clicked.

Net result: the order ends up with Image URL 1/2/3 and AI Processed Image as visible line-item properties, which is exactly what appears on that customer order.

**Chat → Lambda backend**

Two separate AWS API Gateway endpoints are hardcoded as constants near the top of the script.

- `CHAT_API_URL` is the figuro-chat-backend Lambda — this is what handles the actual chat messages: `sendMessage()` POSTs `{ sessionId, message, customerName, webhookUrl }` to it.
- Chat state/history is stored and synced live via Firebase Firestore (lazy-loaded from gstatic.com, project figuro-web-chat), not through the Lambda directly.

Both `snippets/snippet-image-chat-section` and `sections/chat-widget` use the same Lambda backend for AI replies.

### snippets/personalize-order-popup.liquid

**Purpose:** Added as a UX improvement. Customers found Shopify's default product page (separate variant pickers, then a separate add-on section, then add to cart) confusing for a custom/personalized product like a figurine. This wraps the whole thing into a single guided 3-step modal popup: Upload → Preferences → Cart, so the flow feels linear instead of scattered across the page.

**How it works:**

It's a modal (`personalize-order-popup`) triggered by a button, with a 3-step progress indicator (`data-personalize-steps` / `data-step="upload|preferences|cart"`) that highlights the active step as the customer scrolls/progresses through the popup.

Step 1 (Upload) and Step 2 (Preferences) are just sections inside the same popup markup — no page reload, no separate forms. All the actual "Add to cart" logic lives in `assets/personalize-order-popup.js`.

Nested snippets used inside it (not hardcoded — reused from elsewhere in the theme):
- `{% render 'snippet-image-chat-section' %}`: the same photo-upload/AI-preview chat widget covered earlier is reused here for Step 1, just rendered inside the popup instead of inline on the page.
- `{% render 'figuro-addons' %}`: pulls in the add-on picker (gift box, wooden base, levitation stand) for Step 2, same snippet used elsewhere for add-ons.
- `{% render 'icon-arrow-down-small' %}`: small UI icon, purely visual.

**What actually adds to cart:**

`assets/personalize-order-popup.js` builds the cart payload in one `/cart/add.js` call with multiple line items:

- **Main product item:** variant ID pulled from the underlying product form, with line-item properties collected from every `[data-figuro-input]` field in the popup (this is how Image URL 1/2/3, AI Processed Image, and Engraving Text end up on the order — same hidden-input pattern as the chat snippet, just scoped to the popup instead of the page).
- **Add-on items:** for each add-on selected in Step 2, a separate line item is pushed using that add-on's own variant ID (`getAddonCartItems()`), each with its own Engraving Text property if applicable.
- All selected add-on names are joined and written as a `_addon` property on the main item as well (e.g. `_addon: Custom Gift Box`) — same pattern as figuro-customizer, so the same edge case applies: if an add-on's variantId is ever missing/blank, its line item silently won't get pushed even though `_addon` still gets written onto the main item.

It validates that at least Image URL 1 or AI Processed Image is present before allowing add-to-cart — so a customer can't complete this flow without uploading a photo first.

## Templates Reference

This section details the custom Shopify templates applied across products and pages. Theme functionality and layout structures vary depending on the template assigned to a specific resource in Shopify Admin.

### Product Templates

All product templates reside in `templates/product*.json` (or .liquid). Each handles specific layout requirements, review displays, or customization flows:

| Template | Purpose | Example product |
|---|---|---|
| `product.json` (Default Product) | Standard catalog products that do not require an AI photo upload or dynamic image-generation process. Renders standard gallery, variant selector, and default Shopify add-to-cart behavior. | Wooden base |
| `product.customize-product.json` | Primary base template for core custom figurine offerings. Integrates the photo upload widget, AI preview hooks, and customizer sections. | From Photo to Pedestal: Custom 3D Bronze Portrait Figurines |
| `product.accessories.json` | Extends the standard customization template with a dedicated add-ons section, so customers can select complementary items (gift boxes, engravable wooden bases) on the product page. | Turn Your Photo Into a Lifelike 3D Figurine |
| `product.corporate-figurine-product.json` | Dedicated to corporate/bulk figurine offerings; isolated structure for B2B content sections, custom inquiry blocks, or corporate media without altering standard B2C pages. | Custom Corporate Realistic Standing Figurine |
| `product.keychain-products.json` | Keychain product lines; core customizer layout plus a keychain-specific social-proof section (reviews, short video testimonials). | Custom 3D Keychain Figure from Your Photo (Cartoon) |
| `product.levitation-products.json` | Levitating figurines; core customizer structure with levitation-specific video showcases and specialized reviews. | Custom Magnetic Levitation Figurine |
| `product.product-accessories.json` | Standalone accessories sold as individual products (levitation display bases, individual gift boxes), bypassing figurine customization steps. | Turn Your Photo Into a Lifelike 3D Figurine |
| `product.testing.json` | Staging template for testing UX improvements; includes the Guided Order Personalization Popup to test step-by-step modal navigation before rolling out to main templates. | Custom Bridesmaid & Groomsman Mini-Me Gifts |

### Page Templates

Except where noted, all page templates use Shopify's JSON format (`templates/page.*.json`).

| Template | Purpose |
|---|---|
| `page.about-us.json` | Primary company overview page; custom branded blocks for Figuro's origin story and core values. https://getfiguro.com/pages/about-us |
| `page.contact-us.json` | Primary customer inquiry form (name, email, phone, comments). |
| `page.dynamic-hero-page.json` | Themed pages (e.g. Superhero figurines); dynamic hero driven by the `dynamic_page_data` metafield to swap graphics and copy per page. |
| `page.faq.json` | Categorized accordion: General Questions, Ordering & Customization, Refunds & Guarantees. |
| `page.figuro-careers.json` | Employment opportunities, company culture, job application pathways. |
| `page.our-process.json` | Step-by-step figurine creation workflow: photo submission → 3D sculpting → printing → hand-painting. |
| `page.our-work.json` / `page.our-work-2.json` / `page.our-work-3.json` | Portfolio galleries of completed figurines with high-resolution images and verified customer comments. |
| `page.page-comparison-template.json` | Comparative breakdown of Figuro's craftsmanship, materials, and pricing vs competitors. |
| `page.partners.json` | Corporate partners, collaborators, affiliate logos. |
| `page.payment-methods.json` | Supported payment gateways, checkout options, localized transaction methods. |
| `page.photo-guidlines.json` | Photo quality requirements (lighting, angles, facial clarity) for optimal 3D generation output. |
| `page.privacy-choices.json` | Customer opt-out controls for data collection, tracking, and promotional emails. |
| `page.render-content.liquid` | Standard Liquid template; typography-focused layout for legal/compliance documents (Return Policy, Terms of Service, Privacy Policy). |
| `page.testimonials.json` | Social-proof page: general ratings, written reviews, customer showcase photos. |
| `page.usecase-page-template.json` | Occasion-based landing pages (Anniversary, Graduation, Weddings); dynamic hero via programmatic/location page metafields. |

## Active Metafields Reference

Active Shopify Metafields and Metafield Definitions used across products, pages, and dynamic sections to drive custom UI logic, photo generator parameters, and review metrics.

### Product Metafields

| Metafield | Purpose | Utility |
|---|---|---|
| `custom.figurine_style` | Figurine style category (Realistic, Upper-Body, Cartoon/Keychain). Read by `snippet-image-chat-section.liquid` and the image generation service to apply correct AI generation parameters. | Image Preview → AI prompt |
| `custom.custom_product_variant_position` | Exact variant index/position where custom variant icons render on the product page. | Top, Bottom → order of the variant |
| `custom.engravable` | Boolean/config flag on accessories (e.g. wooden display bases) to toggle custom text engraving inputs in product forms and modals. | Text box appear |
| `custom.review_count` & `custom.custom_rating_value` | Custom review ratings and purchase count metrics for static or synced social-proof badges on product cards and customizer banners. | Social proof badges |
| `custom.variant_image` | Image assets for custom variant selection icons within the product option picker. | Variant icons |
| `custom.variant_badge` | **Legacy.** Previously displayed promotional badges ("Recommended", "Money-Back") on variants. Currently inactive on active products. | — |

### How Line Item Properties Work

See the `snippets/personalize-order-popup.liquid` section above — hidden inputs (Image URL 1/2/3, AI Processed Image, Engraving Text) are cloned into the cart form (via `teleport()` in the chat widget) or collected from `[data-figuro-input]` fields (popup flow), so they appear as line-item properties on the order.

### Metafield Definitions (Page & Global)

| Definition | Purpose |
|---|---|
| `custom.programmatic_page_creation` | Powers dynamic location/programmatic landing pages with automated content injection. |
| `custom.our_work` | Connects portfolio images and verified customer comments to the our-work gallery sections. |
| `custom.figuro_comparison` | Feeds comparative tables and feature breakdowns on the comparison template. |
| `custom.dynamic_page` | Custom hero banner assets, copy, layout settings for character-specific pages (Superman, Batman). |
| `custom.testimonial_reviews` | **Legacy.** Previously held structured review data for testimonial sections. Currently inactive. |
| `custom.faq` | Categorized Q&A pairs displayed on the FAQ page. |
| `custom.usecase` | Drives occasion-based landing pages (Mother's Day, Father's Day, Anniversaries). |

## Translation

**translator.py** (project root, with `.env`. Available at the Figuro OMS repository: https://github.com/Nirmhjejeuirueuh/figuro-oms.git)

**Purpose:** Translates the Shopify theme content into other languages in bulk, so we don't have to translate every section by hand in the admin. It reads a CSV exported from Shopify, translates it with DeepL, and writes a new CSV that can be imported back.

### How to Operate

1. **Export the CSV from Shopify:** Settings → Languages → Translate & adapt → pick the language → Export (theme content).
2. **Run the script from the project folder:** `python translator.py file_name.csv lang_tag` (e.g. `python translator.py export.csv it`). This creates `export-it.csv` next to the input file.
3. **Spot-check a few rows**, especially ones with links, Liquid, or brand names.
4. **Import the translated CSV back:** Translate & adapt → same language → Import → upload the file, and choose to overwrite existing translations if needed.

### How It Works

- It goes through the CSV row by row and sends the "Default content" text to DeepL, writing the result into "Translated content".
- Rows that already have a translation are left as they are.
- Fields listed in `SKIPPED_FIELDS` (images, handles, URLs) are copied over, not translated.
- Before translating, Liquid code (`{{ }}`, `{% %}`), URLs, hex colors, and brand names (Figuro) are swapped for placeholders, then restored afterwards, so they don't get translated or broken.
- Requests are rate-limited, and the script saves progress to the output file as it runs.
- If the DeepL quota runs out, the script stops instead of writing untranslated text as if it were translated.

### Setup / Dependencies

- `pip install deepl python-dotenv`
- `.env` file in the project root containing `DEEP_API_KEY=...` (**never commit this file**).

### Things to Watch

- Use the standard language tag for `lang_tag` (it, fr, de, es).
- If a new field holds a URL, image, or other non-text value and gets translated by mistake, add its field name to `SKIPPED_FIELDS`.
