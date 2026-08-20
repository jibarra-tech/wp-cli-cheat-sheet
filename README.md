# WP-CLI Cheat Sheet

A practical collection of WP-CLI commands for WordPress administration, troubleshooting, and maintenance.

## About This Project
## Command Categories

- [Getting Started](#getting-started)
- [Plugin Management](#plugin-management)
- [Database Operations](#database-operations)
- [Search & Replace](#search--replace)
- [User Management](#user-management)
- [Cache & Transients](#cache--transients)
- [Cron](#cron)
- [Production Troubleshooting](#production-troubleshooting)

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

---

## User Management

### List WordPress Users

    wp user list

Lists all users on the current WordPress installation.

### List Only Administrator Accounts

    wp user list --role=administrator

Displays users assigned the Administrator role.

This can be useful when auditing administrative access or investigating unexpected accounts.

### List User IDs

    wp user list --field=ID

Returns only the user IDs.

This is useful when passing user IDs into other WP-CLI commands or scripting administrative tasks.

### Get Specific User Information

    wp user get 123

Displays information about the specified user.

A user can be identified by their ID, login, or email address.

### Check Whether a User Exists

    wp user exists 123

Checks whether a specified user exists.

### Create a User

    wp user create username user@example.com --role=author

Creates a new WordPress user and assigns the specified role.

### Update a User

    wp user update 123 --display_name="Jane Smith"

Updates information associated with an existing user.

### Change a User's Role

    wp user set-role 123 editor

Changes the specified user's WordPress role.

### Reset a User Password

    wp user reset-password 123

Resets the password for the specified user.

The command can also be used with a username, email address, or multiple users.

### List User Capabilities

    wp user list-caps 123

Displays the capabilities associated with a specific user.

This can be useful when investigating unexpected permissions or access issues.

### List User Metadata

    wp user meta list 123

Displays metadata associated with a specific user.

Specific metadata keys can be requested:

    wp user meta list 123 --keys=nickname,description

### Delete a User

    wp user delete 123

Deletes the specified user.

When deleting a user who owns content, posts can be reassigned to another user:

    wp user delete 123 --reassign=456

> **Production note:** Always verify the user and target environment before deleting accounts or changing roles. When deleting a user, consider whether their posts, pages, or other content need to be reassigned first.

### Security Considerations

User management commands can make significant changes to site access and permissions.

When troubleshooting or auditing users:

- Verify the target user before making changes.
- Review administrator accounts carefully.
- Avoid exposing passwords or sensitive user information in command history or documentation.
- Prefer read-only commands such as `wp user list`, `wp user get`, and `wp user list-caps` when investigating an issue.
- Use additional caution when working with production environments.

---

## Cache & Transients

### Flush the WordPress Object Cache

    wp cache flush

Flushes the WordPress Object Cache.

This can be useful when troubleshooting stale or inconsistent cached data.

> **Production note:** Flushing an object cache can have a performance impact, particularly on multisite installations using a persistent object cache. Use caution when performing this operation on production sites.

### Check the Object Cache Type

    wp cache type

Attempts to determine which object cache implementation is being used.

This can help identify whether a site is using the default WordPress object cache or a persistent object cache implementation.

### Check Object Cache Feature Support

    wp cache supports <feature>

Determines whether the current object cache implementation supports a particular feature.

### Get a Cached Value

    wp cache get my_key my_group

Retrieves a value from the WordPress Object Cache.

### Set a Cached Value

    wp cache set my_key my_value my_group 300

Stores a value in the WordPress Object Cache with an expiration time in seconds.

### Delete a Cached Value

    wp cache delete my_key my_group

Removes a specific value from the WordPress Object Cache.

---

## Transients

### List Transients

    wp transient list

Lists transients and their values, including their expiration information.

This can be useful when investigating plugins or themes that are storing large numbers of transient values.

### Get a Transient

    wp transient get transient_name

Retrieves the value of a specific transient.

### Set a Transient

    wp transient set transient_name "example value" 3600

Creates or updates a transient with an expiration time specified in seconds.

### Delete a Transient

    wp transient delete transient_name

Deletes a specific transient.

### Delete Expired Transients

    wp transient delete --expired

Deletes expired transients.

This can be useful when cleaning up expired transient data during troubleshooting or maintenance.

### Delete All Transients

    wp transient delete --all

Deletes all transients.

> **Warning:** Deleting all transients can cause plugins and themes to regenerate cached data. Use caution when performing this operation on production sites.

### Delete Network Transients

    wp transient delete --all --network

Deletes all network/site transients.

This option is particularly relevant when working with WordPress multisite installations.

---

## Cache Troubleshooting

When investigating a caching-related issue, consider the different layers that may be involved:

1. WordPress Object Cache
2. Transients
3. Plugin-level caching
4. Page caching
5. CDN caching
6. Browser caching

Clearing one cache layer does not necessarily clear the others.

A useful troubleshooting approach is to first identify which layer is actually responsible for the stale or unexpected content before clearing caches unnecessarily.

> **Best practice:** Avoid using cache flushes as the first troubleshooting step. When possible, identify the specific cache layer and determine whether the cached data is actually contributing to the issue.

---

## Cron

WP-Cron is WordPress's built-in system for scheduling tasks such as publishing scheduled posts, processing queued actions, sending notifications, and running scheduled plugin or theme tasks.

### List Scheduled Cron Events

    wp cron event list

Lists scheduled WP-Cron events, including their scheduled time, recurrence, and hook name.

### Run a Cron Event

    wp cron event run hook_name

Runs a specific scheduled cron event immediately.

This can be useful when troubleshooting a task that appears to be scheduled but is not executing as expected.

### Run All Due Cron Events

    wp cron event run --due-now

Runs all cron events that are currently due.

### Schedule a Cron Event

    wp cron event schedule <timestamp> <hook>

Schedules a new cron event for a specific Unix timestamp.

For example:

    wp cron event schedule 1735689600 my_custom_hook

### Delete a Cron Event

    wp cron event delete hook_name

Deletes a scheduled cron event.

> **Production note:** Be careful when deleting cron events. Plugins and themes may depend on scheduled hooks to perform important background tasks.

### Check Cron Status

    wp cron test

Tests whether WP-Cron is functioning correctly.

This can be useful when investigating scheduled tasks that are not running.

### Check the WordPress Cron System

    wp cron event list --fields=hook,next_run,next_run_gmt,recurrence

Displays selected information about scheduled events and can make it easier to identify events that are overdue or scheduled unusually frequently.

---

## Cron Troubleshooting

When investigating a cron-related issue, consider:

1. Is the expected cron event actually scheduled?
2. Is the event overdue?
3. Can the event be executed manually?
4. Are there errors when the associated plugin or task runs?
5. Is WordPress Cron disabled?
6. Is the server configured to run an alternative system cron?
7. Is the site experiencing high traffic or resource constraints that could affect scheduled tasks?

Useful commands for investigating cron activity include:

    wp cron event list

    wp cron test

    wp cron event run --due-now

> **Best practice:** Avoid deleting or repeatedly running scheduled events simply to clear a backlog. First determine why the event is not executing as expected.
