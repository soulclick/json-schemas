# Donation Completed Webhook

The `donation.completed` webhook is triggered when a donation has been successfully processed and completed in the system.

## Event Structure

The webhook payload follows a consistent structure with event metadata at the root level and donation-specific data nested within the `data` object.

### Root Properties

| Field         | Type    | Required | Description                                                    |
| ------------- | ------- | -------- | -------------------------------------------------------------- |
| `id`          | integer | Yes      | Unique event identifier                                        |
| `event`       | string  | Yes      | Event type, always `"donation.completed"`                      |
| `created_at`  | string  | Yes      | ISO 8601 timestamp when the event was created                  |
| `environment` | string  | Yes      | Environment where the event occurred (`test`, `stage`, `prod`) |
| `data`        | object  | Yes      | Event data containing donation information                     |

### Event Data Properties

The `data` object contains the donation information:

| Field      | Type   | Required | Description      |
| ---------- | ------ | -------- | ---------------- |
| `donation` | object | Yes      | Donation details |

### Donation Properties

The `donation` object contains detailed information about the completed donation:

| Field        | Type    | Required | Description                                      |
| ------------ | ------- | -------- | ------------------------------------------------ |
| `id`         | integer | Yes      | Unique donation identifier                       |
| `created_at` | string  | Yes      | ISO 8601 timestamp when the donation was created |
| `amount`     | number  | Yes      | Donation amount (minimum 0, multiple of 0.01)    |
| `currency`   | string  | Yes      | ISO 4217 currency code (3 uppercase letters)     |
| `campaign`   | object  | Yes      | Campaign information (id and German title)       |
| `purpose`    | string  | Yes      | Donation purpose or category                     |
| `donor`      | object  | Yes      | Donor information                                |
| `payment`    | object  | Yes      | Payment information                              |
| `invoice`    | string  | No       | Invoice number (null for non-invoice payments)   |

### Campaign Properties

The `campaign` object contains:

| Field   | Type    | Required | Description                |
| ------- | ------- | -------- | -------------------------- |
| `id`    | integer | Yes      | Unique campaign identifier |
| `title` | string  | Yes      | Campaign title (in German) |

### Donor Information

| Field           | Type   | Required | Description                                                               |
| --------------- | ------ | -------- | ------------------------------------------------------------------------- |
| `first_name`    | string | Yes      | Donor first name (minimum 1 character)                                    |
| `last_name`     | string | Yes      | Donor last name (minimum 1 character)                                     |
| `email`         | string | Yes      | Donor email address (valid email format)                                  |
| `street`        | string | Yes      | Street name                                                               |
| `street_number` | string | Yes      | Street number                                                             |
| `city`          | string | Yes      | City                                                                      |
| `postal_code`   | string | Yes      | Postal code                                                               |
| `country`       | string | Yes      | ISO 3166-1 alpha-2 country code (2 uppercase letters)                     |
| `language`      | string | Yes      | Donor's preferred language (ISO 639-1 language code, 2 lowercase letters) |
| `gender`        | string | No       | Donor gender (`male`, `female`, `non-binary`, `prefer-not-to-say`)        |
| `company`       | string | No       | Company name                                                              |
| `po_box`        | string | No       | PO Box                                                                    |

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

## Example Payloads

### TWINT Payment

```json
{
  "id": 1,
  "event": "donation.completed",
  "created_at": "2025-08-27T14:04:39Z",
  "environment": "prod",
  "data": {
    "donation": {
      "id": 188,
      "created_at": "2025-08-25T10:30:00Z",
      "amount": 51.05,
      "currency": "CHF",
      "campaign": {
        "id": 123,
        "title": "Schule statt Fabrik – Hoffnung statt Ausbeutung"
      },
      "purpose": "Freie Spende",
      "donor": {
        "gender": "female",
        "first_name": "Jana",
        "last_name": "Dockter",
        "street": "Jubiläumsstrasse",
        "street_number": "23",
        "city": "Bern",
        "postal_code": "3005",
        "country": "CH",
        "language": "de",
        "email": "janadockter@web.de"
      },
      "payment": {
        "method": "twint",
        "status": "completed"
      },
      "invoice": null
    }
  }
}
```

### Credit Card Payment (Visa)

```json
{
  "id": 2,
  "event": "donation.completed",
  "created_at": "2025-08-27T15:22:15Z",
  "environment": "prod",
  "data": {
    "donation": {
      "id": 189,
      "created_at": "2025-08-27T09:45:00Z",
      "amount": 100.0,
      "currency": "CHF",
      "campaign": {
        "id": 456,
        "title": "Wildhunde Patenschaft"
      },
      "purpose": "Tierpatenschaft",
      "donor": {
        "first_name": "Max",
        "last_name": "Müller",
        "company": "Tech Solutions AG",
        "street": "Bahnhofstrasse",
        "street_number": "1",
        "city": "Zürich",
        "postal_code": "8001",
        "country": "CH",
        "language": "de",
        "email": "max.mueller@example.ch"
      },
      "payment": {
        "method": "visa",
        "status": "completed"
      },
      "invoice": null
    }
  }
}
```

### Invoice Payment

```json
{
  "id": 3,
  "event": "donation.completed",
  "created_at": "2025-08-27T09:15:30Z",
  "environment": "prod",
  "data": {
    "donation": {
      "id": 190,
      "created_at": "2025-08-27T08:20:00Z",
      "amount": 250.0,
      "currency": "CHF",
      "campaign": {
        "id": 789,
        "title": "Bildung für alle"
      },
      "purpose": "Bildungsprojekt",
      "donor": {
        "gender": "male",
        "first_name": "Peter",
        "last_name": "Schmidt",
        "street": "Musterstrasse",
        "street_number": "42",
        "po_box": "Postfach 1234",
        "city": "Basel",
        "postal_code": "4000",
        "country": "CH",
        "language": "fr",
        "email": "peter.schmidt@company.ch"
      },
      "payment": {
        "method": "invoice",
        "status": "pending"
      },
      "invoice": "INV-2025-001190"
    }
  }
}
```

## Integration Notes

- All timestamps are in ISO 8601 format with UTC timezone
- Currency amounts are represented as decimal numbers with up to 2 decimal places
- The event is only triggered after successful donation processing
- For invoice payments, the status may initially be `pending` until payment is received
- Webhook deliveries include retry logic for failed deliveries
- Verify the webhook signature to ensure payload authenticity

## Related Files

- [JSON Schema](./donation-completed.json) - Complete JSON Schema definition
- [Example Files](../examples/) - Individual example files for testing
