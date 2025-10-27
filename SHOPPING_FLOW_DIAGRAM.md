# 🛍️ AI Pro Shopping - Complete Visual Flow

## **User Journey Overview**

```
┌─────────────────────────────────────────────────────────────────┐
│                     USER SHOPPING JOURNEY                        │
└─────────────────────────────────────────────────────────────────┘

    START
      ↓
┌──────────────┐
│  1. SEARCH   │  "Find me headphones under $300"
└──────┬───────┘
       ↓
┌──────────────────────────────────────────────────────────────┐
│  PRODUCT DISPLAY (APL)                                       │
│  ┌────────────────────────────────────────────────────┐     │
│  │  🛍️ AI Pro Shop              🛒 0 items          │     │
│  │  ─────────────────────────────────────────────────  │     │
│  │                                                     │     │
│  │  [📷] Sony WH-1000XM5                             │     │
│  │       $398.00  ⭐ 4.5/5 (12,543 reviews)          │     │
│  │       Industry-leading noise canceling...          │     │
│  │       [🛒 Say: Add item 1]                        │     │
│  │                                                     │     │
│  │  [📷] Bose QuietComfort Ultra                     │     │
│  │       $429.00  ⭐ 4.3/5 (8,921 reviews)           │     │
│  │       Premium noise canceling...                   │     │
│  │       [🛒 Say: Add item 2]                        │     │
│  └────────────────────────────────────────────────────┘     │
└──────────────────────────────────────────────────────────────┘
       ↓
    "Add item 1"
       ↓
┌──────────────┐
│  2. ADD TO   │  ✅ Added Sony WH-1000XM5 to cart ($398)
│     CART     │  "You now have 1 item. Say 'view cart' to review"
└──────┬───────┘
       ↓
    "View cart"
       ↓
┌──────────────────────────────────────────────────────────────┐
│  SHOPPING CART (APL)                                         │
│  ┌────────────────────────────────────────────────────┐     │
│  │  🛒 Your Shopping Cart                            │     │
│  │  ─────────────────────────────────────────────────  │     │
│  │                                                     │     │
│  │  ✓ [📷] Sony WH-1000XM5        $398.00           │     │
│  │                                                     │     │
│  │  ╔═══════════════════════════════════════════╗     │     │
│  │  ║  Total:                      $398.00      ║     │     │
│  │  ║                                            ║     │     │
│  │  ║  [💳 Say: Checkout now]                  ║     │     │
│  │  ╚═══════════════════════════════════════════╝     │     │
│  └────────────────────────────────────────────────────┘     │
└──────────────────────────────────────────────────────────────┘
       ↓
    "Checkout now"
       ↓
┌──────────────┐
│  3. CHECKOUT │  🔔 "You're about to purchase 1 item for $398"
│     CONFIRM  │  "Say 'yes, buy it' to confirm or 'cancel'"
└──────┬───────┘
       ↓
    "Yes, buy it"
       ↓
┌──────────────────────────────────────────────────────────────┐
│  ORDER CONFIRMATION (APL)                                    │
│  ┌────────────────────────────────────────────────────┐     │
│  │                      ✅                            │     │
│  │                                                     │     │
│  │              Order Confirmed!                       │     │
│  │                                                     │     │
│  │              Order #A1B2C3D4                       │     │
│  │                                                     │     │
│  │         ╔═════════════════════════════╗            │     │
│  │         ║  Total Paid:      $398.00   ║            │     │
│  │         ╚═════════════════════════════╝            │     │
│  │                                                     │     │
│  │         Tracking: AIPRO-A1B2C3D4                   │     │
│  │         Delivery: 3-5 business days                │     │
│  │                                                     │     │
│  │    Check your Alexa app for details               │     │
│  └────────────────────────────────────────────────────┘     │
└──────────────────────────────────────────────────────────────┘
       ↓
     END
```

---

## **Technical Architecture**

```
┌─────────────────────────────────────────────────────────────────┐
│                      SYSTEM ARCHITECTURE                         │
└─────────────────────────────────────────────────────────────────┘

┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│    Alexa     │  Voice  │   Lambda     │  Data   │   DynamoDB   │
│    Skill     │ ──────> │   Function   │ ──────> │              │
└──────┬───────┘         └──────┬───────┘         └──────────────┘
       │                        │
       │ Visual                 │ Product Data
       ↓                        ↓
┌──────────────┐         ┌──────────────┐
│     APL      │         │  Shopping    │
│  Documents   │         │    Tools     │
└──────────────┘         └──────┬───────┘
                                │
                                │ Affiliate Links
                                ↓
                         ┌──────────────┐
                         │   Amazon     │
                         │  Associates  │
                         └──────────────┘
```

---

## **Data Flow Diagrams**

### **1. Product Search Flow**

```
User Query
    ↓
[Lambda Handler]
    ↓
Detect Shopping Intent
    ↓
[product_search_tool()]
    ↓
┌─────────────────────┐
│  Search Products    │
│  • Amazon API       │
│  • Filter by price  │
│  • Add ratings      │
└─────────┬───────────┘
          ↓
┌─────────────────────┐
│  Inject Affiliate   │
│  • Add tracking ID  │
│  • Build full URL   │
└─────────┬───────────┘
          ↓
┌─────────────────────┐
│  Return to Lambda   │
│  • Product list     │
│  • Structured JSON  │
└─────────┬───────────┘
          ↓
┌─────────────────────┐
│  Build APL Display  │
│  • Product cards    │
│  • Images & prices  │
│  • Add to cart CTAs │
└─────────┬───────────┘
          ↓
Return to User
(Voice + Visual)
```

### **2. Cart Management Flow**

```
"Add item 1"
    ↓
[Lambda Handler]
    ↓
[AddToCartIntent]
    ↓
┌─────────────────────┐
│  Get User Cart      │
│  • Query DynamoDB   │
│  • Load cart data   │
└─────────┬───────────┘
          ↓
┌─────────────────────┐
│  Validate Product   │
│  • Check if exists  │
│  • Check duplicates │
└─────────┬───────────┘
          ↓
┌─────────────────────┐
│  Add to Cart        │
│  • Append product   │
│  • Recalculate $    │
└─────────┬───────────┘
          ↓
┌─────────────────────┐
│  Save to DynamoDB   │
│  • Update cart      │
│  • Update timestamp │
└─────────┬───────────┘
          ↓
Confirmation Message
"Added [product] to cart"
```

### **3. Checkout Flow**

```
"Checkout now"
    ↓
[Lambda Handler]
    ↓
[CheckoutIntent]
    ↓
┌─────────────────────┐
│  Get User Cart      │
│  • Retrieve items   │
│  • Calculate total  │
└─────────┬───────────┘
          ↓
┌─────────────────────┐
│  Validate Cart      │
│  • Items exist?     │
│  • Prices valid?    │
└─────────┬───────────┘
          ↓
Set Session Attribute
"pending_checkout: true"
    ↓
Ask for Confirmation
"Say yes to buy"
    ↓
  "Yes, buy it"
    ↓
[ConfirmPurchaseIntent]
    ↓
┌─────────────────────┐
│  Create Order       │
│  • Generate ID      │
│  • Set status       │
│  • Add tracking #   │
└─────────┬───────────┘
          ↓
┌─────────────────────┐
│  Save to DynamoDB   │
│  • Store order      │
│  • Add to history   │
└─────────┬───────────┘
          ↓
┌─────────────────────┐
│  Clear Cart         │
│  • Empty items      │
│  • Reset total      │
└─────────┬───────────┘
          ↓
┌─────────────────────┐
│  Show Confirmation  │
│  • APL document     │
│  • Order details    │
│  • Tracking info    │
└─────────┬───────────┘
          ↓
Success! ✅
```

---

## **Voice Command Map**

```
┌─────────────────────────────────────────────────────────────────┐
│                      VOICE COMMANDS                              │
└─────────────────────────────────────────────────────────────────┘

🔍 SEARCH & BROWSE
├─ "Find me [product]"                    → ShoppingIntent
├─ "Search for [product]"                 → ShoppingIntent
├─ "Show me [product] under [price]"      → ShoppingIntent
└─ "What's the best [product]"            → ShoppingIntent

🛒 CART MANAGEMENT
├─ "Add item [number]"                    → AddToCartIntent
├─ "Put item [number] in my cart"         → AddToCartIntent
├─ "View cart"                            → ViewCartIntent
├─ "Show my cart"                         → ViewCartIntent
├─ "Remove item [number]"                 → RemoveFromCartIntent
└─ "Clear cart"                           → ClearCartIntent

💳 CHECKOUT & PURCHASE
├─ "Checkout now"                         → CheckoutIntent
├─ "Proceed to checkout"                  → CheckoutIntent
├─ "Yes, buy it"                          → ConfirmPurchaseIntent
├─ "Confirm purchase"                     → ConfirmPurchaseIntent
├─ "Cancel"                               → CancelPurchaseIntent
└─ "No thanks"                            → CancelPurchaseIntent

📦 ORDER TRACKING
├─ "Track my order"                       → TrackOrderIntent
├─ "Where is my order"                    → TrackOrderIntent
└─ "Show my orders"                       → ViewOrderHistoryIntent

❓ HELP & NAVIGATION
├─ "Help"                                 → AMAZON.HelpIntent
├─ "Stop"                                 → AMAZON.StopIntent
└─ "Cancel"                               → AMAZON.CancelIntent
```

---

## **State Machine Diagram**

```
┌─────────────────────────────────────────────────────────────────┐
│                    SESSION STATE FLOW                            │
└─────────────────────────────────────────────────────────────────┘

        ┌─────────────┐
        │   LAUNCH    │
        └──────┬──────┘
               │
        ┌──────▼──────┐
        │   BROWSING  │ ◄──────────────┐
        └──────┬──────┘                │
               │                       │
      "Add item X"                     │
               │                       │
        ┌──────▼──────┐                │
        │  SHOPPING   │                │
        │  (has cart) │                │
        └──────┬──────┘                │
               │                       │
        ┌──────▼──────┐                │
        │  View Cart  │                │
        └──────┬──────┘                │
               │                       │
      "Checkout now"                   │
               │                       │
        ┌──────▼──────┐                │
        │  CHECKOUT   │                │
        │  (pending)  │ ─"Cancel"──────┘
        └──────┬──────┘
               │
      "Yes, buy it"
               │
        ┌──────▼──────┐
        │  CONFIRMED  │
        └──────┬──────┘
               │
          ORDER SAVED
               │
        ┌──────▼──────┐
        │     END     │
        └─────────────┘
```

---

## **APL Visual States**

```
┌─────────────────────────────────────────────────────────────────┐
│                     APL DOCUMENT TYPES                           │
└─────────────────────────────────────────────────────────────────┘

1. PRODUCT DISPLAY
   ├─ Header: Logo + Cart Counter
   ├─ Body: Scrollable Product Cards
   │  ├─ Product Image
   │  ├─ Name & Description
   │  ├─ Price (large, green)
   │  ├─ Rating (stars)
   │  └─ "Add to Cart" CTA
   └─ Footer: Status + Commands

2. SHOPPING CART VIEW
   ├─ Header: "Your Shopping Cart"
   ├─ Body: Cart Items List
   │  ├─ Checkmark icon
   │  ├─ Product thumbnail
   │  ├─ Product name
   │  └─ Price
   ├─ Total Section
   │  ├─ Grand Total (large)
   │  └─ Checkout Button
   └─ Footer: Instructions

3. ORDER CONFIRMATION
   ├─ Centered Container
   ├─ Success Icon (✅)
   ├─ "Order Confirmed!" Title
   ├─ Order Number
   ├─ Total Paid
   ├─ Tracking Information
   └─ Delivery Estimate
```

---

## **Monetization Flow**

```
┌─────────────────────────────────────────────────────────────────┐
│                    REVENUE GENERATION                            │
└─────────────────────────────────────────────────────────────────┘

User Searches
     ↓
Products Found
     ↓
┌─────────────────────────┐
│  Affiliate Link Added   │
│  ?tag=yourname-20       │
└────────────┬────────────┘
             │
User Clicks/Purchases
             ↓
┌─────────────────────────┐
│  Amazon Tracks Click    │
│  via Affiliate Tag      │
└────────────┬────────────┘
             │
Purchase Made (24hr window)
             ↓
┌─────────────────────────┐
│  Commission Credited    │
│  1-10% of sale price    │
└────────────┬────────────┘
             │
Monthly Payout
             ↓
┌─────────────────────────┐
│  Earnings Dashboard     │
│  affiliate-program      │
│  .amazon.com            │
└─────────────────────────┘
```

**Affiliate Commission Rates:**
- Electronics: 1-4%
- Home & Garden: 3-8%
- Fashion & Accessories: 4-10%
- Books: 4.5%
- Luxury Beauty: 10%

---

## **Error Handling Flow**

```
┌─────────────────────────────────────────────────────────────────┐
│                     ERROR SCENARIOS                              │
└─────────────────────────────────────────────────────────────────┘

API Failure
     ↓
Try-Catch Block
     ↓
Log Error (CloudWatch)
     ↓
User-Friendly Message
     ↓
"I'm having trouble right now. Please try again."

Empty Cart Checkout
     ↓
Check cart.items.length
     ↓
If 0: "Your cart is empty"
     ↓
Redirect to Search

Duplicate Item
     ↓
Check existing items
     ↓
If found: "Already in cart"
     ↓
Don't add again

Session Timeout
     ↓
Session expires (8hr)
     ↓
Cart persists in DynamoDB
     ↓
Next session: Cart restored
```

---

## **Testing Flow**

```
┌─────────────────────────────────────────────────────────────────┐
│                      TEST SEQUENCE                               │
└─────────────────────────────────────────────────────────────────┘

Step 1: Launch
  Input: "Alexa, open AI Pro"
  Expected: Welcome message
  ✓ Pass / ✗ Fail

Step 2: Search
  Input: "Find me headphones"
  Expected: Product list with APL
  ✓ Pass / ✗ Fail

Step 3: Add to Cart
  Input: "Add item 1"
  Expected: Confirmation + cart count
  ✓ Pass / ✗ Fail

Step 4: View Cart
  Input: "Show my cart"
  Expected: Cart APL with total
  ✓ Pass / ✗ Fail

Step 5: Checkout
  Input: "Checkout now"
  Expected: Confirmation prompt
  ✓ Pass / ✗ Fail

Step 6: Confirm
  Input: "Yes, buy it"
  Expected: Order confirmation APL
  ✓ Pass / ✗ Fail

Step 7: Verify DynamoDB
  Check: Order saved in database
  ✓ Pass / ✗ Fail

Step 8: Verify Cart Cleared
  Input: "Show my cart"
  Expected: "Cart is empty"
  ✓ Pass / ✗ Fail
```

---

## **Performance Metrics**

```
┌─────────────────────────────────────────────────────────────────┐
│                    KEY METRICS TO TRACK                          │
└─────────────────────────────────────────────────────────────────┘

📊 CONVERSION FUNNEL
┌──────────────────────┬─────────────┬─────────────┐
│ Stage                │ Count       │ Drop-off %  │
├──────────────────────┼─────────────┼─────────────┤
│ Searches             │ 1,000       │ -           │
│ Products Viewed      │ 850         │ 15%         │
│ Items Added to Cart  │ 425         │ 50%         │
│ Checkout Started     │ 340         │ 20%         │
│ Orders Completed     │ 289         │ 15%         │
└──────────────────────┴─────────────┴─────────────┘

Conversion Rate: 28.9% (orders/searches)
Add-to-Cart Rate: 42.5% (adds/views)
Checkout Complete: 85% (orders/checkouts)

💰 REVENUE METRICS
┌──────────────────────┬─────────────┐
│ Metric               │ Value       │
├──────────────────────┼─────────────┤
│ Avg Order Value      │ $247.50     │
│ Total Orders         │ 289         │
│ Total GMV            │ $71,527.50  │
│ Avg Commission (4%)  │ $2,861.10   │
│ Affiliate Clicks     │ 1,200       │
│ Click-through Rate   │ 141% (1.2)  │
└──────────────────────┴─────────────┘
```

---

## **Success Checklist**

```
┌─────────────────────────────────────────────────────────────────┐
│                    LAUNCH READINESS                              │
└─────────────────────────────────────────────────────────────────┘

FUNCTIONALITY
☐ Product search returns results
☐ APL displays correctly on Echo Show
☐ Add to cart updates session
☐ Cart persists in DynamoDB
☐ Checkout prompts confirmation
☐ Orders save to database
☐ Cart clears after purchase
☐ Affiliate links include tracking ID

TESTING
☐ Tested on Echo Show
☐ Tested on Echo Spot
☐ Tested in Alexa app
☐ Tested all voice commands
☐ Tested error scenarios
☐ Verified DynamoDB writes
☐ Checked CloudWatch logs

SETUP
☐ Lambda function deployed
☐ Interaction model updated
☐ APL interface enabled
☐ Environment variables set
☐ Affiliate account created
☐ DynamoDB table configured

DOCUMENTATION
☐ Privacy policy created
☐ Skill description written
☐ Test instructions provided
☐ Screenshots captured

MONITORING
☐ CloudWatch alarms set
☐ Error tracking enabled
☐ Conversion tracking ready
☐ Revenue dashboard accessible
```

---

**🎉 Your complete shopping flow is ready to deploy!**

See `COMPLETE_SHOPPING_GUIDE.md` for detailed instructions.

