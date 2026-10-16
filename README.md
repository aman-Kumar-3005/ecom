# PHP storefront and order-management codebase

## Overview

A PHP storefront and administration codebase with product browsing, inventory, customer accounts, orders, referrals, and payment adapters. The repository is useful for inspecting application structure and data flows. Its existing audit records unresolved payment and access-control issues; it should not be presented as a production-certified system.

## System architecture and modules

| Path | Responsibility |
| --- | --- |
| `index.php / category.php / product_details.php` | Catalog pages. |
| `cart.php / checkout.php / orders.php` | Cart, checkout, and order views. |
| `admin_*.php / inventory.php / users.php` | Administration screens. |
| `includes/auth.php / includes/csrf.php` | Authentication and CSRF helpers. |
| `includes/db.php / includes/env.php` | PDO connection and custom environment loader. |
| `includes/payment/` | PaymentProviderInterface, PaymentService, NativeUPIEngine, and CashfreeEngine. |
| `api/` | Payment callbacks, status checks, and manual/simulated confirmation endpoints. |
| `migrate.php / ecom_db.sql` | Schema/migration code and a historical database export. |
| `AUDIT.md` | Existing findings and the exact scope of earlier verification. |

## Tech stack

Plain PHP, PDO MySQL, MySQL/MariaDB, HTML, CSS, and JavaScript. No Laravel application or Composer dependency manifest is included.

## Local setup

For a source-only local review:

```bash
git clone https://github.com/shreyasingh3005/ecom.git
cd ecom
cp .env.example .env
find . -type f -name '*.php' -not -path './.git/*' -exec php -l {} \;
```

The PHP CLI is required for linting. The template's `DB_PASSWORD` line has an unmatched quote; replace it with a complete local value before use. The relevant keys are `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`, `APP_URL`, `APP_ENV`, and `APP_DEBUG`.

Database work must use a disposable local database and synthetic records. Inspect `migrate.php`, its imported JSON files, and `ecom_db.sql` before choosing an initialization path: they include data and side effects, not just schema definitions. Do not point these scripts at a shared or live database. There is no verified one-command application installation in this revision.

## Implementation notes and limits

- SQL tables include `admins`, `users`, `user_addresses`, `products`, `product_images`, `categories`, `orders`, `order_items`, `payments`, `inventory_transactions`, `referrals`, `wallet_transactions`, `settings`, and `audit_logs`. `migrate.php` also creates content-management tables.
- Read [AUDIT.md](AUDIT.md) before evaluating this repository. Manual and simulated payment confirmation, transaction ownership checks, webhook state transitions, and embedded database configuration remain material review items.
- Do not use the included payment/E2E scripts against real orders, stock, mail, or payment services. No live transaction or checkout verification is claimed here.
- The source includes regulated-product content. This README documents the software for code review; it does not provide a commercial deployment or sales guide.
