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

## Repository Structure

```
json-schemas/
├── README.md
├── webhooks/
│   ├── donation-completed.json     # JSON Schema
│   └── donation-completed.md       # Human-readable documentation
└── examples/
    ├── donation-twint-example.json
    ├── donation-visa-example.json
    └── donation-invoice-example.json
```

## Usage

### Validation

You can validate webhook payloads against the schema using any JSON Schema validator that supports Draft 7.

### Integration

Reference the schema directly from this repository in your integration code:

```
https://raw.githubusercontent.com/soulclick/json-schemas/main/webhooks/donation-completed.json
```

## Support

For questions about webhook integration or schema usage, please contact our integration team or open an issue in this repository.
