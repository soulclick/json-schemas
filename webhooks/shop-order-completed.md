# Shop Order Completed Webhook

The `shop_order.completed` webhook is triggered when a shop order has been successfully completed and processed in the system.

## Event Structure

The webhook payload follows a consistent structure with event metadata at the root level and order-specific data nested within the `data` object.

### Root Properties

| Field         | Type    | Required | Description                                                    |
| ------------- | ------- | -------- | -------------------------------------------------------------- |
| `id`          | integer | Yes      | Unique event identifier                                        |
| `event`       | string  | Yes      | Event type, always `"shop_order.completed"`                    |
| `created_at`  | string  | Yes      | ISO 8601 timestamp when the event was created                  |
| `environment` | string  | Yes      | Environment where the event occurred (`test`, `stage`, `prod`) |
| `data`        | object  | Yes      | Event data containing order information                        |

### Event Data Properties

The `data` object contains the order information:

| Field   | Type   | Required | Description   |
| ------- | ------ | -------- | ------------- |
| `order` | object | Yes      | Order details |

### Order Properties

The `order` object contains detailed information about the completed order:

| Field              | Type   | Required | Description                                   |
| ------------------ | ------ | -------- | --------------------------------------------- |
| `id`               | string | Yes      | Order number (unique order identifier)        |
| `created_at`       | string | Yes      | ISO 8601 timestamp when the order was created |
| `customer`         | object | Yes      | Customer information                          |
| `items`            | array  | Yes      | Array of ordered items                        |
| `delivery_address` | object | Yes      | Delivery address information                  |
| `invoice_address`  | object | Yes      | Invoice address information                   |
| `currency`         | string | Yes      | ISO 4217 currency code (3 uppercase letters)  |
| `amount_subtotal`  | number | Yes      | Order subtotal before tax and shipping        |
| `amount_tax`       | number | Yes      | Tax amount                                    |
| `amount_total`     | number | Yes      | Total order amount including tax and shipping |
| `payment`          | object | Yes      | Payment information                           |
| `shipping`         | object | Yes      | Shipping information                          |

### Customer Information

| Field      | Type   | Required | Description                                                                  |
| ---------- | ------ | -------- | ---------------------------------------------------------------------------- |
| `type`     | string | Yes      | Customer type (`registered` or `guest`)                                      |
| `email`    | string | Yes      | Customer email address (valid email format)                                  |
| `language` | string | Yes      | Customer's preferred language (ISO 639-1 language code, 2 lowercase letters) |

### Item Information

Each item in the `items` array contains:

| Field            | Type    | Required | Description                                          |
| ---------------- | ------- | -------- | ---------------------------------------------------- |
| `product_id`     | integer | Yes      | Product identifier                                   |
| `description`    | string  | Yes      | Product name or description                          |
| `quantity`       | integer | Yes      | Quantity ordered (minimum 1)                         |
| `unit_amount`    | number  | Yes      | Unit price per item                                  |
| `amount_total`   | number  | Yes      | Total amount for this item (quantity \* unit_amount) |
| `article_number` | string  | No       | Article number                                       |

### Address Information

Both `delivery_address` and `invoice_address` have the same structure:

| Field           | Type   | Required | Description                                           |
| --------------- | ------ | -------- | ----------------------------------------------------- |
| `street`        | string | Yes      | Street name                                           |
| `street_number` | string | Yes      | Street number                                         |
| `postal_code`   | string | Yes      | Postal code                                           |
| `city`          | string | Yes      | City                                                  |
| `country`       | string | Yes      | ISO 3166-1 alpha-2 country code (2 uppercase letters) |
| `title`         | string | No       | Title (e.g., "Herr", "Frau")                          |
| `first_name`    | string | No       | First name                                            |
| `last_name`     | string | No       | Last name                                             |
| `company_name`  | string | No       | Company name                                          |
| `care_of`       | string | No       | Care of / attention to (z.Hd.)                        |
| `department`    | string | No       | Department                                            |
| `po_box`        | string | No       | PO Box                                                |

### Payment Information

| Field    | Type   | Required | Description         |
| -------- | ------ | -------- | ------------------- |
| `method` | string | Yes      | Payment method used |
| `status` | string | Yes      | Payment status      |

#### Payment Methods

The following payment methods are supported:

- `twint` - TWINT mobile payment
- `visa` - Visa credit card
- `mastercard` - Mastercard credit card
- `postfinance` - PostFinance payment
- `invoice` - Invoice payment

#### Payment Status

- `completed` - Payment has been successfully processed
- `pending` - Payment is being processed
- `failed` - Payment processing failed

### Shipping Information

| Field    | Type   | Required | Description     |
| -------- | ------ | -------- | --------------- |
| `method` | string | Yes      | Shipping method |
| `cost`   | number | Yes      | Shipping cost   |

## Example Payloads

### TWINT Payment

```json
{
  "id": 1,
  "event": "shop_order.completed",
  "created_at": "2025-08-29T14:04:39Z",
  "environment": "prod",
  "data": {
    "order": {
      "id": "2708202500000",
      "created_at": "2025-08-27T13:17:47Z",
      "customer": {
        "type": "guest",
        "email": "myriam.udry@gmail.com",
        "language": "fr"
      },
      "items": [
        {
          "product_id": 86,
          "description": "Kleber \"Rund um die Welt\"",
          "article_number": "6304",
          "quantity": 12,
          "unit_amount": 0.0,
          "amount_total": 0.0
        },
        {
          "product_id": 18,
          "description": "Missions-Rosenkranz",
          "article_number": "3598",
          "quantity": 12,
          "unit_amount": 5.0,
          "amount_total": 60.0
        }
      ],
      "delivery_address": {
        "title": "Frau",
        "first_name": "Myriam",
        "last_name": "Udry",
        "company_name": "",
        "care_of": "",
        "department": "",
        "street": "Chemin des Courtis",
        "street_number": "2a",
        "po_box": "",
        "postal_code": "1976",
        "city": "Daillon",
        "country": "CH"
      },
      "invoice_address": {
        "title": "",
        "first_name": "",
        "last_name": "",
        "company_name": "",
        "care_of": "",
        "department": "",
        "street": "",
        "street_number": "",
        "po_box": "",
        "postal_code": "",
        "city": "",
        "country": "CH"
      },
      "currency": "CHF",
      "amount_subtotal": 60.0,
      "amount_tax": 5.75,
      "amount_total": 72.0,
      "payment": {
        "method": "twint",
        "status": "completed"
      },
      "shipping": {
        "method": "Service-Pauschale B-Post Paket",
        "cost": 12.0
      }
    }
  }
}
```

### Invoice Payment

```json
{
  "id": 2,
  "event": "shop_order.completed",
  "created_at": "2025-08-29T15:22:15Z",
  "environment": "prod",
  "data": {
    "order": {
      "id": "2708202500002",
      "created_at": "2025-08-27T16:37:20Z",
      "customer": {
        "type": "registered",
        "email": "sanela.zeba@kath-berneck.ch",
        "language": "de"
      },
      "items": [
        {
          "product_id": 45,
          "description": "Sample Product",
          "article_number": "9104",
          "quantity": 1,
          "unit_amount": 0.0,
          "amount_total": 0.0
        }
      ],
      "delivery_address": {
        "title": "Frau",
        "first_name": "",
        "last_name": "",
        "company_name": "Sanela Zeba",
        "care_of": "",
        "department": "",
        "street": "Büntstrasse",
        "street_number": "4",
        "po_box": "",
        "postal_code": "9442",
        "city": "Berneck",
        "country": "CH"
      },
      "invoice_address": {
        "title": "Frau",
        "first_name": "",
        "last_name": "",
        "company_name": "Sanela Zeba",
        "care_of": "",
        "department": "",
        "street": "Büntstrasse",
        "street_number": "4",
        "po_box": "",
        "postal_code": "9442",
        "city": "Berneck",
        "country": "CH"
      },
      "currency": "CHF",
      "amount_subtotal": 0.0,
      "amount_tax": 0.0,
      "amount_total": 0.0,
      "payment": {
        "method": "invoice",
        "status": "pending"
      },
      "shipping": {
        "method": "Service-Pauschale B-Post Paket",
        "cost": 0.0
      }
    }
  }
}
```

## Integration Notes

- All timestamps are in ISO 8601 format with UTC timezone
- Currency amounts are represented as decimal numbers with up to 2 decimal places
- The event is only triggered after successful order processing
- Customer type distinguishes between registered users and guest checkouts
- Address fields follow Swiss addressing conventions with optional care_of (z.Hd.) support
- For invoice payments, the status may initially be `pending` until payment is received
- Webhook deliveries include retry logic for failed deliveries
- Verify the webhook signature to ensure payload authenticity

## Pricing Calculation

The order total is calculated using the following formula:

```
amount_total = amount_subtotal - amount_discount + amount_tax + shipping.cost
```

### Financial Fields

| Field             | Description                                     | Example |
| ----------------- | ----------------------------------------------- | ------- |
| `amount_subtotal` | Sum of all item totals (quantity × unit_amount) | 60.00   |
| `amount_discount` | Total discount amount applied to order          | 5.75    |
| `amount_tax`      | Tax amount calculated on discounted subtotal    | 5.75    |
| `amount_total`    | Final total amount to charge                    | 72.00   |
| `shipping.cost`   | Shipping cost (included in amount_total)        | 12.00   |

### Calculation Example

For an order with:

- Items subtotal: 60.00 CHF
- Applied discount: 5.75 CHF
- Tax (calculated): 5.75 CHF
- Shipping cost: 12.00 CHF

**Final total**: 60.00 - 5.75 + 5.75 + 12.00 = **72.00 CHF**

### Discount Information

- Discounts are applied at the order level, not per item
- The `amount_discount` field shows the total discount amount
- Tax is calculated on the discounted subtotal
- A value of `0.0` indicates no discounts were applied

## Related Files

- [JSON Schema](./shop-order-completed.json) - Complete JSON Schema definition
- [Example Files](../examples/) - Individual example files for testing
