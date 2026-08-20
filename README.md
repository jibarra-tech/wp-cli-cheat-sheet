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

---

## Production Troubleshooting

Production troubleshooting requires a methodical approach. Rather than immediately changing configuration or clearing caches, begin by identifying the symptoms, gathering evidence, and narrowing the issue down to its root cause.

A useful troubleshooting workflow is:

**Symptom → Investigation → Evidence → Root Cause → Resolution → Verification**

> **Production principle:** Avoid making changes before understanding the problem. Whenever possible, gather logs, metrics, and other evidence first.

---

### 1. Identify the Symptoms

Start by clearly defining what is actually failing.

Common symptoms include:

- Site completely unavailable
- HTTP 502, 503, or 504 responses
- Slow page loads
- WordPress admin issues
- PHP errors
- Database errors
- High server resource usage
- Unexpected redirects
- Intermittent connectivity
- Large increases in traffic
- Scheduled tasks not executing
- Specific requests consuming excessive resources

Determine whether the problem affects:

- The entire site
- Only the WordPress admin
- A specific URL
- A specific user
- A specific geographic location
- A specific time period
- A specific type of request

Defining the scope of the problem can significantly reduce the number of possible causes.

---

### 2. Check Monitoring and Server Health

Monitoring can provide an important overview of what is happening at the server level.

When investigating an incident, review available metrics for:

- CPU utilization
- Memory utilization
- Disk usage
- Apache activity
- Nginx activity
- PHP processes
- Database activity
- Request volume
- Response times

A sudden increase in resource usage can help establish when an incident began and whether it correlates with increased traffic or a specific type of request.

---

### 3. Review Web Server Logs

Logs are one of the most valuable sources of information during production troubleshooting.

Common commands for reviewing logs include:

    tail -f access.log

    tail -f error.log

    grep "500" access.log

    grep "POST" access.log

    grep "wp-cron" access.log

    grep "admin-ajax.php" access.log

For larger logs, tools such as `grep`, `awk`, `sed`, `sort`, and `uniq` can be combined to identify patterns and high-volume requests.

Example:

    grep "admin-ajax.php" access.log | sort | uniq -c | sort -nr

This can help identify repeated requests to `admin-ajax.php` and determine whether a particular request pattern is contributing to increased server activity.

---

### 4. Investigate HTTP Errors

#### 502 Bad Gateway

A 502 response can indicate that an upstream service did not provide a valid response.

Potential areas to investigate include:

- PHP processes
- PHP-FPM
- Upstream connectivity
- Resource exhaustion
- Application errors
- Web server configuration

The HTTP status code alone does not identify the root cause, so logs and server metrics should be reviewed.

#### 503 Service Unavailable

A 503 response generally indicates that the service is temporarily unable to handle the request.

Possible areas of investigation include:

- Resource exhaustion
- Application-level failures
- Maintenance states
- Upstream service availability
- Excessive request volume

#### 504 Gateway Timeout

A 504 response indicates that a gateway or proxy did not receive a timely response from an upstream service.

Investigate:

- Slow PHP requests
- Slow database queries
- External API calls
- Long-running WordPress operations
- Resource constraints
- Application behavior

> **Troubleshooting principle:** Treat the HTTP status code as a symptom rather than a diagnosis.

---

### 5. Investigate High Traffic and Suspicious Requests

A sudden increase in traffic can significantly affect server resources.

When traffic appears abnormal, investigate:

- Source IP addresses
- User agents
- Requested URLs
- Request methods
- Request frequency
- Geographic patterns
- Response status codes

Useful log-analysis commands include:

    grep "pattern" access.log

    awk '{print $1}' access.log | sort | uniq -c | sort -nr

The second command can be used to identify IP addresses generating large numbers of requests.

When investigating suspicious traffic, correlate request volume with server metrics to determine whether the traffic is contributing to the reported symptoms.

---

### 6. Investigate DDoS or Automated Traffic

Large volumes of automated requests can cause increased server utilization even when individual requests appear relatively normal.

Indicators may include:

- Sudden increases in request volume
- Large numbers of requests from a small number of IP addresses
- Repeated requests to the same endpoint
- Unusual or outdated user agents
- High Apache or PHP utilization
- Requests bypassing expected caching behavior

When investigating suspected abusive traffic:

1. Identify the request pattern.
2. Determine which IPs or user agents are generating the traffic.
3. Determine which URLs are being requested.
4. Compare request volume with server resource utilization.
5. Determine whether the traffic is cached or reaching the application.
6. Apply appropriate mitigation.
7. Monitor the system after mitigation.

> **Production principle:** Blocking traffic should be based on evidence whenever possible. Avoid blocking legitimate crawlers, monitoring services, or customers without first validating the traffic pattern.

---

### 7. Investigate WordPress Admin Performance

When the front end of a site is functioning but `/wp-admin/` is slow or unavailable, investigate the administrative requests separately.

Areas to examine include:

- Plugin behavior
- Database queries
- `admin-ajax.php`
- WordPress cron activity
- PHP resource usage
- Large or expensive administrative operations

Example:

    grep "admin-ajax.php" access.log | sort | uniq -c | sort -nr

High request volume to `admin-ajax.php` may indicate that a plugin, theme, or external process is generating repeated asynchronous requests.

---

### 8. Investigate Database-Related Problems

When a WordPress site is experiencing database-related errors or performance issues, investigate:

- Database connectivity
- Slow queries
- Database size
- Large tables
- Plugin-generated queries
- Locking or contention
- Recent database changes

Useful WP-CLI commands include:

    wp db size

    wp db tables

    wp db query "SELECT ..."

For database imports or migrations, verify:

- The correct database is being targeted.
- A valid backup exists.
- The import completed successfully.
- Database tables are present.
- WordPress configuration references the expected database.

---

### 9. Investigate Cache Behavior

Caching problems can sometimes appear as application problems.

When content appears stale or inconsistent, identify the cache layer involved:

1. Browser cache
2. CDN cache
3. Page cache
4. Plugin cache
5. WordPress object cache
6. Transients

Useful WP-CLI commands include:

    wp cache flush

    wp transient list

    wp transient delete --expired

Avoid clearing every cache layer immediately. First determine which layer is responsible for the behavior.

---

### 10. Correlate Multiple Sources of Evidence

The most useful troubleshooting information often comes from combining multiple sources.

For example:

**Grafana**

→ identifies when resource utilization increased

**Access logs**

→ identify which requests increased

**Error logs**

→ identify application or web-server errors

**WP-CLI**

→ provides application-level information

**Database investigation**

→ identifies expensive or problematic queries

Combining these sources can help distinguish between a traffic problem, application problem, infrastructure problem, and database problem.

---

### 11. Verify the Resolution

After making a change, verify that the original problem has actually been resolved.

Check:

- HTTP response codes
- Page load behavior
- Server resource utilization
- Request volume
- Error logs
- Application functionality
- Customer-reported symptoms

Continue monitoring after the immediate symptoms disappear to ensure the issue does not return.

---

## Example Incident Investigation

A useful incident investigation can follow this pattern:

**Symptom**

Website becomes slow or intermittently unavailable.

**Investigation**

Monitoring shows a significant increase in Apache activity.

**Evidence**

Access logs show a large number of repeated requests from a small number of IP addresses and unusual user agents.

**Correlation**

The increase in request volume occurs at the same time Apache utilization increases.

**Root Cause**

Automated traffic is generating enough uncached requests to place significant load on the application stack.

**Resolution**

Mitigate the abusive traffic and continue monitoring server metrics and request volume.

**Verification**

Apache utilization returns toward normal levels and the reported site performance improves.

---

## Troubleshooting Principles

- Gather evidence before making changes.
- Identify the scope of the problem.
- Correlate logs with monitoring data.
- Treat symptoms and root causes separately.
- Change one variable at a time when practical.
- Prefer reversible changes.
- Verify the result after making a change.
- Document the findings and resolution.
- Consider whether the issue could recur and whether preventative measures are appropriate.

> **Important:** Production environments can contain sensitive customer information. Never publish real customer domains, IP addresses, credentials, log entries containing personal information, or other confidential data in public documentation.
