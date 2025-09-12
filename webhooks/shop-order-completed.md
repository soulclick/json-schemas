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

| Field        | Type   | Required | Description   |
| ------------ | ------ | -------- | ------------- |
| `shop_order` | object | Yes      | Order details |

### Order Properties

The `shop_order` object contains detailed information about the completed order:

| Field               | Type   | Required | Description                                   |
| ------------------- | ------ | -------- | --------------------------------------------- |
| `id`                | string | Yes      | Order number (unique order identifier)        |
| `created_at`        | string | Yes      | ISO 8601 timestamp when the order was created |
| `customer`          | object | Yes      | Customer information                          |
| `items`             | array  | Yes      | Array of ordered items                        |
| `delivery_address`  | object | Yes      | Delivery address information                  |
| `invoice_address`   | object | Yes      | Invoice address information                   |
| `currency`          | string | Yes      | ISO 4217 currency code (3 uppercase letters)  |
| `total_discount`    | number | Yes      | Total discount amount applied to order        |
| `total_vat`         | number | Yes      | Total VAT/tax amount                          |
| `total_price_gross` | number | Yes      | Final total including tax and shipping        |
| `payment`           | object | Yes      | Payment information                           |
| `shipping`          | object | Yes      | Shipping information                          |

### Customer Information

| Field           | Type        | Required | Description                                                                  |
| --------------- | ----------- | -------- | ---------------------------------------------------------------------------- |
| `id`            | integer     | No       | Customer identifier                                                          |
| `gender`        | string      | No       | Gender (`male`, `female`, `non-binary`, `prefer-not-to-say`)                 |
| `first_name`    | string      | No       | First name                                                                   |
| `last_name`     | string      | No       | Last name                                                                    |
| `email`         | string      | Yes      | Customer email address (valid email format)                                  |
| `phone`         | string/null | No       | Customer phone number                                                        |
| `company`       | string      | No       | Company name                                                                 |
| `department`    | string      | No       | Department                                                                   |
| `care_of`       | string      | No       | Care of / attention to (z.Hd.)                                               |
| `po_box`        | string      | No       | PO Box                                                                       |
| `street`        | string      | Yes      | Street name                                                                  |
| `street_number` | string      | Yes      | Street number                                                                |
| `postal_code`   | string      | Yes      | Postal code                                                                  |
| `city`          | string      | Yes      | City                                                                         |
| `country`       | string      | Yes      | ISO 3166-1 alpha-2 country code (2 uppercase letters)                        |
| `language`      | string      | Yes      | Customer's preferred language (ISO 639-1 language code, 2 lowercase letters) |

### Item Information

Each item in the `items` array contains:

| Field                       | Type    | Required | Description                                       |
| --------------------------- | ------- | -------- | ------------------------------------------------- |
| `article_number`            | string  | Yes      | Article number                                    |
| `quantity`                  | integer | Yes      | Quantity ordered (minimum 1)                      |
| `unit_price_net`            | number  | Yes      | Unit price without tax                            |
| `unit_price_gross`          | number  | Yes      | Unit price including tax                          |
| `original_unit_price_net`   | number  | No       | Original unit price without tax before discount   |
| `original_unit_price_gross` | number  | No       | Original unit price including tax before discount |
| `total_price_gross`         | number  | Yes      | Total price for this item including tax           |
| `discount_percentage`       | number  | No       | Discount percentage applied to this item          |

### Address Information

Both `delivery_address` and `invoice_address` have the same structure:

| Field           | Type    | Required | Description                                           |
| --------------- | ------- | -------- | ----------------------------------------------------- |
| `id`            | integer | No       | Address identifier                                    |
| `first_name`    | string  | No       | First name                                            |
| `last_name`     | string | No       | Last name                                             |
| `company`       | string | No       | Company name                                          |
| `department`    | string | No       | Department                                            |
| `care_of`       | string | No       | Care of / attention to (z.Hd.)                        |
| `po_box`        | string | No       | PO Box                                                |
| `street`        | string | Yes      | Street name                                           |
| `street_number` | string | Yes      | Street number                                         |
| `postal_code`   | string | Yes      | Postal code                                           |
| `city`          | string | Yes      | City                                                  |
| `country`       | string | Yes      | ISO 3166-1 alpha-2 country code (2 uppercase letters) |

### Payment Information

| Field            | Type   | Required | Description                              |
| ---------------- | ------ | -------- | ---------------------------------------- |
| `transaction_id` | string | Yes      | Payment processor transaction identifier |
| `method`         | string | Yes      | Payment method used                      |
| `status`         | string | Yes      | Payment status                           |

#### Payment Methods

The following payment method codes are supported (see [Datatrans documentation](https://docs.datatrans.ch/docs/payment-methods)):

- `TWI` - TWINT mobile payment
- `PAY` - Google Pay
- `PFC` - PostFinance Card
- `PEF` - PostFinance E-Finance
- `VIS` - Visa credit card
- `ECA` - MasterCard
- `APL` - Apple Pay
- `PAP` - PayPal
- `INV` - Invoice payment
- `AMX` - American Express
- `ALP` - Alipay+
- `AZP` - Amazon Pay
- `CFY` - Cofidis Pay
- `KLN` - Klarna
- `DIB` - Diners Club
- `PSC` - Paysafecard
- `REK` - Reka
- `SAM` - Samsung Pay
- `ELV` - SEPA

#### Payment Status

- `completed` - Payment has been successfully processed
- `pending` - Payment is being processed
- `failed` - Payment processing failed

### Shipping Information

| Field    | Type    | Required | Description         |
| -------- | ------- | -------- | ------------------- |
| `id`     | integer | No       | Shipping identifier |
| `cost`   | number  | Yes      | Shipping cost       |
| `method` | string  | Yes      | Shipping method     |

## Example Payloads

### TWINT Payment

```json
{
  "id": 1,
  "event": "shop_order.completed",
  "created_at": "2025-08-29T14:04:39Z",
  "environment": "prod",
  "data": {
    "shop_order": {
      "id": "2708202500000",
      "created_at": "2025-08-27T13:17:47Z",
      "customer": {
        "id": 12345,
        "email": "myriam.udry@gmail.com",
        "phone": null,
        "language": "fr",
        "gender": "female",
        "first_name": "Anna",
        "last_name": "Mueller",
        "company": "",
        "care_of": "",
        "department": "",
        "street": "Chemin des Courtis",
        "street_number": "2a",
        "po_box": "",
        "postal_code": "1976",
        "city": "Daillon",
        "country": "CH"
      },
      "items": [
        {
          "article_number": "6304",
          "quantity": 12,
          "unit_price_net": 0.0,
          "unit_price_gross": 0.0,
          "original_unit_price_net": 0.0,
          "original_unit_price_gross": 0.0,
          "discount_percentage": 0,
          "total_price_gross": 0.0
        },
        {
          "article_number": "3598",
          "quantity": 12,
          "unit_price_net": 4.63,
          "unit_price_gross": 5.0,
          "original_unit_price_net": 4.63,
          "original_unit_price_gross": 5.0,
          "discount_percentage": 0,
          "total_price_gross": 60.0
        }
      ],
      "delivery_address": {
        "id": 2001,
        "first_name": "Anna",
        "last_name": "Mueller",
        "company": "",
        "department": "",
        "care_of": "",
        "po_box": "",
        "street": "Chemin des Courtis",
        "street_number": "2a",
        "postal_code": "1976",
        "city": "Daillon",
        "country": "CH"
      },
      "invoice_address": {
        "id": 3001,
        "first_name": "",
        "last_name": "",
        "company": "",
        "department": "",
        "care_of": "",
        "po_box": "",
        "street": "Chemin des Courtis",
        "street_number": "2a",
        "postal_code": "1976",
        "city": "Daillon",
        "country": "CH"
      },
      "currency": "CHF",
      "total_price_gross": 67.0,
      "total_vat": 5.31,
      "total_discount": 5.75,
      "payment": {
        "transaction_id": "twi_2708202500000",
        "method": "TWI",
        "status": "completed"
      },
      "shipping": {
        "id": 1001,
        "cost": 12.0,
        "method": "Service-Pauschale B-Post Paket"
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
    "shop_order": {
      "id": "2708202500002",
      "created_at": "2025-08-27T16:37:20Z",
      "customer": {
        "id": 11111,
        "email": "sanela.zeba@kath-berneck.ch",
        "phone": null,
        "language": "de",
        "first_name": "",
        "last_name": "",
        "company": "Beispiel AG",
        "care_of": "",
        "department": "",
        "street": "Büntstrasse",
        "street_number": "4",
        "po_box": "",
        "postal_code": "9442",
        "city": "Berneck",
        "country": "CH"
      },
      "items": [
        {
          "article_number": "9104",
          "quantity": 1,
          "unit_price_net": 0.0,
          "unit_price_gross": 0.0,
          "original_unit_price_net": 0.0,
          "original_unit_price_gross": 0.0,
          "discount_percentage": 0,
          "total_price_gross": 0.0
        }
      ],
      "delivery_address": {
        "id": 2004,
        "first_name": "",
        "last_name": "",
        "company": "Beispiel AG",
        "department": "",
        "care_of": "",
        "po_box": "",
        "street": "Büntstrasse",
        "street_number": "4",
        "postal_code": "9442",
        "city": "Berneck",
        "country": "CH"
      },
      "invoice_address": {
        "id": 3004,
        "first_name": "",
        "last_name": "",
        "company": "Beispiel AG",
        "department": "",
        "care_of": "",
        "po_box": "",
        "street": "Büntstrasse",
        "street_number": "4",
        "postal_code": "9442",
        "city": "Berneck",
        "country": "CH"
      },
      "currency": "CHF",
      "total_price_gross": 0.0,
      "total_vat": 0.0,
      "total_discount": 0.0,
      "payment": {
        "transaction_id": "qr-2708202500002",
        "method": "INV",
        "status": "pending"
      },
      "shipping": {
        "id": 1004,
        "cost": 0.0,
        "method": "Service-Pauschale B-Post Paket"
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
total_price_gross = sum_of_item_totals + total_vat + shipping.cost
```

### Financial Fields

| Field               | Description                                   | Example |
| ------------------- | --------------------------------------------- | ------- |
| `shipping.cost`     | Shipping cost (included in total_price_gross) | 9.00    |
| `total_discount`    | Total discount amount applied to order        | 11.65   |
| `total_vat`         | Total VAT/tax amount                          | 13.27   |
| `total_price_gross` | Final total including tax and shipping        | 65.90   |

### Calculation Example

For an order with:

- Items subtotal: 60.00 CHF
- Tax (calculated): 5.75 CHF
- Shipping cost: 12.00 CHF

**Final total**: 60.00 + 5.75 + 12.00 = **77.75 CHF**

### Discount Information

- Discounts are applied at the order level, not per item
- The `total_discount` field shows the total discount amount
- Item totals already reflect any applied discounts
- A value of `0.0` indicates no discounts were applied

## Related Files

- [JSON Schema](./shop-order-completed.json) - Complete JSON Schema definition
- [Example Files](../examples/) - Individual example files for testing
