# WP-CLI Cheat Sheet

A practical collection of WP-CLI commands for WordPress administration, troubleshooting, and maintenance.

## About This Project
## Command Categories

- [Getting Started](#getting-started)
- [Plugin Management](#plugin-management)
- [Database Operations](#database-operations)
- [Search & Replace](#search--replace)
- [User Management](#user-management)
- [Cache Management](#cache-management)
- [Cron](#cron)
- [Troubleshooting](#troubleshooting)

This cheat sheet documents commonly used WP-CLI commands for managing and troubleshooting WordPress installations from the command line.

The focus is on practical commands used for day-to-day WordPress administration, production troubleshooting, database management, plugin and theme management, and user administration.

> **Note:** Commands should be tested in a safe environment before being run against production sites. Always verify the target site and command before performing destructive operations.

---

## Getting Started

### Check WP-CLI Version

```bash
wp cli version
```

Displays the currently installed WP-CLI version.

### Display WP-CLI Environment Information

```bash
wp cli info
```

Displays information about the WP-CLI environment, including the operating system, shell, PHP binary, PHP version, configuration, and WP-CLI version.

### Get Help

```bash
wp help
```

Displays general WP-CLI help.

Get help for a specific command:

```bash
wp help plugin
```

Get help for a specific subcommand:

```bash
wp help plugin list
```

---

## Plugin Management

### List Installed Plugins

```bash
wp plugin list
```

Lists the plugins installed on the WordPress site, including their status and version.

### List Only Active Plugins

```bash
wp plugin list --status=active
```

Displays only currently active plugins.

### Check Plugin Status

```bash
wp plugin status
```

Displays information about installed plugins and their current status.

### Activate a Plugin

```bash
wp plugin activate plugin-name
```

Activates a specific plugin.

### Deactivate a Plugin

```bash
wp plugin deactivate plugin-name
```

Deactivates a specific plugin.

### Deactivate All Plugins

```bash
wp plugin deactivate --all
```

Deactivates all installed plugins.

### Update a Plugin

```bash
wp plugin update plugin-name
```

Updates a specific plugin.

### Update All Plugins

```bash
wp plugin update --all
```

Updates all installed plugins.

### Install a Plugin

```bash
wp plugin install plugin-name
```

Downloads and installs a plugin.

Install and activate it immediately:

```bash
wp plugin install plugin-name --activate
```

### Delete a Plugin

```bash
wp plugin delete plugin-name
```

Deletes a specific plugin.

> **Production note:** Be careful with plugin activation, updates, and deletion on production sites. Verify the target environment and understand the potential impact before making changes.

---

---

## Database Operations

### Export the WordPress Database

    wp db export backup.sql

Exports the WordPress database to a SQL file.

A timestamp can be included in the filename to help organize multiple backups:

    wp db export backup-$(date +%Y%m%d-%H%M%S).sql

### Import a Database

    wp db import backup.sql

Imports a SQL database dump into the current WordPress installation.

> **Production note:** Database imports can overwrite existing data. Always verify the target environment and database backup before performing an import.

### List Database Tables

    wp db tables

Lists the database tables associated with the WordPress installation.

To include all tables matching the WordPress table prefix:

    wp db tables --all-tables-with-prefix

### Check Database Size

    wp db size

Displays the database name and size.

To display the size of individual database tables:

    wp db size --tables

### Run a SQL Query

    wp db query "SELECT * FROM wp_options LIMIT 10;"

Executes a SQL query against the WordPress database.

> **Warning:** Use caution when running SQL queries against production databases. Read-only `SELECT` queries are generally safer for investigation, while `UPDATE`, `DELETE`, and other write operations can modify or remove data.

### Optimize Database Tables

    wp db optimize

Optimizes the database tables.

### Repair Database Tables

    wp db repair

Attempts to repair database tables when supported by the database engine.

### Open the MySQL Console

    wp db cli

Opens a MySQL console using the database credentials configured for the WordPress installation.

### Search Database Content

    wp db search "search-term"

Searches the database for a specific string.

This can be useful when investigating where a particular value exists within a WordPress database.

For example:

    wp db search "example.com"

> **Production note:** Always verify the environment before running database commands. When troubleshooting production issues, start with read-only operations whenever possible and create or verify a backup before making changes.

---

## Search & Replace

### Basic Search & Replace

    wp search-replace 'old-domain.com' 'new-domain.com'

Searches the WordPress database for a specific value and replaces it with a new value.

This is commonly used when changing domains, migrating WordPress sites, or updating URLs within a database.

### Preview Changes Without Making Them

    wp search-replace 'old-domain.com' 'new-domain.com' --dry-run

Performs a search and reports the changes that would be made without modifying the database.

> **Best practice:** Use `--dry-run` first when performing a search-and-replace operation, especially on production environments.

### Search and Replace Specific Tables

    wp search-replace 'old-domain.com' 'new-domain.com' wp_posts wp_postmeta

Limits the search-and-replace operation to the specified database tables.

### Search Without Replacing

    wp search-replace 'old-domain.com' 'new-domain.com' --dry-run --verbose

Performs a dry run and provides additional information about the changes that would be made.

### Export the Database Before a Search & Replace

    wp db export pre-search-replace.sql

Creates a database backup before performing a search-and-replace operation.

### Skip Specific Columns

    wp search-replace 'old-domain.com' 'new-domain.com' --skip-columns=guid

Allows specific database columns to be excluded from the search-and-replace operation.

> **Production note:** Always create or verify a recent database backup before performing a search-and-replace operation. Use `--dry-run` to review the expected changes before modifying production data.

### Serialized Data

WP-CLI's `search-replace` command is designed to handle serialized PHP data correctly. This is important because WordPress stores serialized data in many plugins and themes.

Using a database tool that performs a simple text replacement can corrupt serialized data. WP-CLI's search-and-replace functionality accounts for serialized data when performing replacements.
