# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository contains JSON schemas and webhook documentation for Soulclick's integration APIs. It defines the structure of webhook events sent to external partners for donation and shop order completion events.

## Common Commands

### Code Formatting
```bash
pnpm prettier -w webhooks examples
```

### Git Workflow
Standard git operations are used. All changes should be committed with detailed commit messages following the established pattern with co-authorship attribution.

## Architecture & Structure

### Webhook Event Pattern
All webhook events follow a consistent structure:
- **Root level**: Event metadata (`id`, `event`, `created_at`, `environment`, `data`)
- **Data level**: Nested domain object (`donation` or `order`) containing the actual event data
- **Examples**: Each schema has corresponding examples for different payment methods (TWINT, Visa, Invoice)

### Schema Standards
- **JSON Schema Draft 7**: All schemas use JSON Schema Draft 7 specification
- **Field Naming**: Follows Stripe-like conventions (e.g., `amount_total`, `unit_amount`)
- **Address Structure**: Swiss addressing conventions with optional `care_of` (z.Hd.) support
- **Nested Objects**: Customer, payment, and shipping information use nested object patterns for consistency

### Two Main Webhook Types

#### donation.completed
- Simple donation events with donor information
- Supports individual and company donations
- Payment methods: TWINT, Visa, Mastercard, PostFinance, Invoice
- Donor addresses are optional

#### shop_order.completed  
- Complex e-commerce orders with line items
- Supports both registered users and guest customers
- Separate delivery and invoice addresses (both required)
- Multiple items per order with product details
- Financial breakdown: subtotal, tax, total amounts

### Field Consistency Rules
- **Currency**: ISO 4217 3-letter uppercase codes
- **Country**: ISO 3166-1 alpha-2 2-letter uppercase codes  
- **Language**: ISO 639-1 2-letter lowercase codes
- **Timestamps**: ISO 8601 date-time format with UTC timezone
- **Amounts**: Numbers with `multipleOf: 0.01` for currency precision

### Payment Methods
Standardized payment method enums across both schemas:
- `twint` - TWINT mobile payment
- `visa` - Visa credit card
- `mastercard` - Mastercard credit card
- `postfinance` - PostFinance payment
- `invoice` - Invoice payment (may have `pending` status initially)

### Address Structure
Swiss addressing with required and optional fields:
- **Required**: `street`, `street_number`, `postal_code`, `city`, `country`
- **Optional**: `title`, `first_name`, `last_name`, `company_name`, `care_of`, `department`, `po_box`

### Documentation Pattern
Each schema has corresponding markdown documentation following a consistent structure:
1. Event description and trigger conditions
2. Schema structure tables with field descriptions
3. Example payloads for different payment methods
4. Integration notes and technical considerations

### Example Files
Individual JSON example files in `/examples/` directory demonstrate real webhook payloads for each payment method and event type. These are used for testing and integration reference.