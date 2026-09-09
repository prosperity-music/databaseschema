# Prosperity Music Database Schema

Canonical Declarative JSON Database Schema for Prosperity Music (Cloudflare D1 / SQLite).

## Usage
- **Schema File:** [schema.json](./schema.json)
- **Raw URL:** https://raw.githubusercontent.com/prosperity-music/databaseschema/main/schema.json

Both the Prosperity Central Auth Worker and the Harmony Music mobile clients consume this schema for:
1. **New Account Provisioning:** Automatically generating all D1 core tables on user registration.
2. **Automated Schema Migrations:** Auto-detecting missing columns/tables and safely applying ALTER TABLE / CREATE TABLE without manual SQL scripts.
