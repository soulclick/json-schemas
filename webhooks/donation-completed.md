# Donation Completed Webhook

The `donation.completed` webhook is triggered when a donation has been successfully processed and completed in the system.

## Event Structure

The webhook payload follows a consistent structure with event metadata at the root level and donation-specific data nested within the `data` object.

### Root Properties

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | integer | Yes | Unique event identifier |
| `event` | string | Yes | Event type, always `"donation.completed"` |
| `created_at` | string | Yes | ISO 8601 timestamp when the event was created |
| `environment` | string | Yes | Environment where the event occurred (`test`, `stage`, `prod`) |
| `data` | object | Yes | Donation-specific data |

### Donation Data Properties

The `data` object contains detailed information about the completed donation:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | integer | Yes | Unique donation identifier |
| `created_at` | string | Yes | ISO 8601 timestamp when the donation was created |
| `amount` | number | Yes | Donation amount (minimum 0, multiple of 0.01) |
| `currency` | string | Yes | ISO 4217 currency code (3 uppercase letters) |
| `campaign` | string | Yes | Campaign or product name |
| `purpose` | string | Yes | Donation purpose or category |
| `donor` | object | Yes | Donor information |
| `payment` | object | Yes | Payment information |

### Donor Information

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Donor full name (minimum 1 character) |
| `email` | string | Yes | Donor email address (valid email format) |
| `title` | string | No | Donor title (e.g., "Herr", "Frau") |
| `address` | string | No | Street address |
| `city` | string | No | City with postal code |
| `country` | string | No | ISO 3166-1 alpha-2 country code (2 uppercase letters) |

### Payment Information

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `method` | string | Yes | Payment method used |
| `status` | string | Yes | Payment status |
| `invoice_number` | string | Conditional | Invoice number (required only for invoice payments) |

#### Payment Methods

The following payment methods are supported:

- `twint` - TWINT mobile payment
- `visa` - Visa credit card
- `mastercard` - Mastercard credit card
- `postfinance` - PostFinance payment
- `invoice` - Invoice payment
- `bank_transfer` - Bank transfer

#### Payment Status

- `completed` - Payment has been successfully processed
- `pending` - Payment is being processed
- `failed` - Payment processing failed

#### Conditional Validation

When the payment method is `invoice`, the `invoice_number` field becomes required.

## Example Payloads

### TWINT Payment

```json
{
  "id": 1,
  "event": "donation.completed",
  "created_at": "2025-08-27T14:04:39Z",
  "environment": "prod",
  "data": {
    "id": 188,
    "created_at": "2025-08-25T10:30:00Z",
    "amount": 51.05,
    "currency": "CHF",
    "campaign": "Schule statt Fabrik – Hoffnung statt Ausbeutung",
    "purpose": "Freie Spende",
    "donor": {
      "title": "Frau",
      "name": "Jana Dockter",
      "address": "Jubiläumsstrasse 23",
      "city": "3005 Bern",
      "country": "CH",
      "email": "janadockter@web.de"
    },
    "payment": {
      "method": "twint",
      "status": "completed"
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
    "id": 189,
    "created_at": "2025-08-27T09:45:00Z",
    "amount": 100.00,
    "currency": "CHF",
    "campaign": "Wildhunde Patenschaft",
    "purpose": "Tierpatenschaft",
    "donor": {
      "name": "Max Müller",
      "address": "Bahnhofstrasse 1",
      "city": "8001 Zürich",
      "country": "CH",
      "email": "max.mueller@example.ch"
    },
    "payment": {
      "method": "visa",
      "status": "completed"
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
    "id": 190,
    "created_at": "2025-08-27T08:20:00Z",
    "amount": 250.00,
    "currency": "CHF",
    "campaign": "Bildung für alle",
    "purpose": "Bildungsprojekt",
    "donor": {
      "title": "Herr",
      "name": "Peter Schmidt",
      "address": "Musterstrasse 42",
      "city": "4000 Basel",
      "country": "CH",
      "email": "peter.schmidt@company.ch"
    },
    "payment": {
      "method": "invoice",
      "status": "pending",
      "invoice_number": "INV-2025-001190"
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