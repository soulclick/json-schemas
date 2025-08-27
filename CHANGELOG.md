# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Comprehensive webhook documentation for donation.completed event

### Changed
- Updated environment naming to use short forms (test, stage, prod)
- Replaced 'production' with 'prod' in schema and examples
- Added 'test' environment to enum

### Added
- Added `company` field to donor object for company donations
- Added structured address fields: `street_name`, `street_number`, `po_box`

### Changed
- Wrapped donation data in `donation` object within `event.data`
- Updated schema structure from `event.data.*` to `event.data.donation.*`
- Replaced single `address` field with separate `street_name` and `street_number` fields

### Removed
- Removed 'bank_transfer' from payment methods enum

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