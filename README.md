# Soulclick JSON Schemas

This repository contains JSON schemas and examples for Soulclick's APIs and webhook events.

## Webhook Events

### donation.completed

Schema for donation completion webhook events sent to integration partners.

- **Schema**: [webhooks/donation-completed.json](webhooks/donation-completed.json)
- **Documentation**: [webhooks/donation-completed.md](webhooks/donation-completed.md)

#### Examples

- [TWINT Payment](examples/donation-twint-example.json)
- [Visa Payment](examples/donation-visa-example.json)
- [Invoice Payment](examples/donation-invoice-example.json)

#### Online Schema Viewer

You can view the donation.completed schema in a user-friendly format using the Atlassian JSON Schema Viewer:

**[View Schema Documentation](https://json-schema.app/view/%23?url=https%3A%2F%2Fraw.githubusercontent.com%2Fsoulclick%2Fjson-schemas%2Fmain%2Fwebhooks%2Fdonation-completed.json)**

### shop_order.completed

Schema for shop order completion webhook events sent to integration partners.

- **Schema**: [webhooks/shop-order-completed.json](webhooks/shop-order-completed.json)
- **Documentation**: [webhooks/shop-order-completed.md](webhooks/shop-order-completed.md)

#### Examples

- [TWINT Payment](examples/shop-order-twint-example.json)
- [Visa Payment](examples/shop-order-visa-example.json)
- [Invoice Payment](examples/shop-order-invoice-example.json)

#### Online Schema Viewer

You can view the shop_order.completed schema in a user-friendly format using the Atlassian JSON Schema Viewer:

**[View Schema Documentation](https://json-schema.app/view/%23?url=https%3A%2F%2Fraw.githubusercontent.com%2Fsoulclick%2Fjson-schemas%2Fmain%2Fwebhooks%2Fshop-order-completed.json)**

## Repository Structure

```
json-schemas/
├── webhooks/     # JSON Schema definitions and documentation
└── examples/     # Example webhook payloads for testing
```

## Usage

### Validation

You can validate webhook payloads against the schema using any JSON Schema validator that supports Draft 7.

### Integration

Reference the schemas directly from this repository in your integration code:

**Donation webhooks:**
```
https://raw.githubusercontent.com/soulclick/json-schemas/main/webhooks/donation-completed.json
```

**Shop order webhooks:**
```
https://raw.githubusercontent.com/soulclick/json-schemas/main/webhooks/shop-order-completed.json
```

## Support

For questions about webhook integration or schema usage, please contact our integration team or open an issue in this repository.
