# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- Comprehensive webhook documentation for donation.completed event
- Added `company` field to donor object for company donations
- Added structured address fields: `street_name`, `street_number`, `po_box`
- Added `language` field to donor object for preferred language (required)
- Added `invoice` field to donation object for invoice tracking
- Added shop_order.completed webhook schema with support for both registered users and guest customers
- Added `amount_discount` field to shop order schema for discount transparency
- Added comprehensive pricing calculation documentation with formula and examples
- Added `newsletter` field to donor object to indicate newsletter opt-in status
- Added `transaction_id` field to payment object for payment processor transaction tracking
- Added full address fields to shop order customer object
- Added `phone` field to shop order customer object
- Added `discount_percentage` field to shop order items

### Changed

- Replaced `title` field with `gender` field in donation and shop order schemas (values: `male`, `female`, `non-binary`, `prefer-not-to-say`)
- Improved address field patterns: company addresses omit personal fields entirely, personal addresses omit company fields entirely
- Updated donation campaign field from string to object with `id` (integer) and `title` (string in German)
- Updated environment naming to use short forms (test, stage, prod)
- Replaced 'production' with 'prod' in schema and examples
- Added 'test' environment to enum
- Wrapped donation data in `donation` object within `event.data`
- Updated schema structure from `event.data.*` to `event.data.donation.*`
- Replaced single `address` field with separate `street_name` and `street_number` fields
- Made address fields required: `street_name`, `street_number`, `city`, `postal_code`, `country`
- Separated `city` and `postal_code` into distinct fields
- Split donor `name` field into separate `first_name` and `last_name` fields
- Moved `invoice` from payment object to donation level
- Simplified payment object to only contain `method` and `status`
- Reordered address fields in shop order schema for better logical flow (personal info → company info → physical address → location)
- Updated shop order schema field names for consistency: `refers_to` → `care_of`, `address_2` → `po_box`, `zip` → `postal_code`
- Changed shop order ID from integer to string (order number)
- Updated shop order amount fields: `total_price` → `amount_total`, `vat_price` → `amount_tax`
- Updated shop order item fields to use Stripe-like naming: `product_name` → `description`, `unit_price` → `unit_amount`, `total_price` → `amount_total`
- Flattened shipping structure: replaced nested `shipment` object with flat `shipping_method` and `shipping_cost` fields
- Reordered order fields for logical flow: metadata → customer → items → addresses → financial summary → transaction details
- Updated shop order payment object to match donation schema with required `status` field
- Re-added `transaction_id` field to both donation and shop order payment objects
- Made address fields properly required: `street`, `street_number`, `postal_code`, `city`, `country`
- Added `amount_subtotal` field to order for financial transparency
- Updated payment methods to use Datatrans codes (TWI, PAY, PFC, PEF, VIS, ECA, APL, PAP, INV, AMX, ALP, AZP, CFY, KLN, DIB, PSC, REK, SAM, ELV) for both donation and shop order schemas
- Changed donation ID from integer to string for better compatibility
- Made donation `purpose` field optional (can be null)
- Changed shop order wrapper from `order` to `shop_order` for consistency
- Updated shop order item fields: `unit_amount` → `unit_price_net` and `unit_price_gross`, `amount_total` → `total_price_gross`
- Updated shop order financial fields: `amount_subtotal` → `total_price_net`, `amount_discount` → `total_discount`, `amount_tax` → `total_vat`, `amount_total` → `total_price_gross`
- Changed address field `company_name` to `company` for consistency across schemas
- Removed `type` field from shop order customer object
- Reordered shipping object properties (cost, method) for consistency
- Reordered field definitions to match Django serializer field ordering for better maintainability

### Removed

- Removed 'bank_transfer' from payment methods enum
- Removed conditional validation for `invoice` in payment object
- Removed `order.status` field from shop order schema
- Removed `canton` and `municipality` fields from shop order addresses

## [1.0.1] - 2025-08-27

### Changed

- Updated timestamp field names to use `_at` suffix for consistency
- Renamed `created` to `created_at` in webhook schema and examples
- Renamed donation `date` field to `created_at` with ISO date-time format
- Removed title/value wrapper from examples for Atlassian schema viewer compatibility

## [1.0.0] - 2025-08-27

### Added

- Initial donation.completed webhook JSON schema
- Three example files demonstrating different payment methods:
  - TWINT payment example
  - Visa payment example
  - Invoice payment example
- README.md with project documentation
- Support for multiple payment methods: twint, visa, mastercard, postfinance, invoice, bank_transfer
- Conditional validation requiring invoice_number for invoice payments
- Comprehensive donor information schema with optional fields
- ISO standard validation for currency codes (ISO 4217) and country codes (ISO 3166-1 alpha-2)

### Schema Features

- Event metadata: id, event type, timestamp, environment
- Donation data: amount, currency, campaign, purpose
- Donor information: name, email, optional address details
- Payment information: method, status, conditional invoice number
- Built-in examples for schema validation and documentation
