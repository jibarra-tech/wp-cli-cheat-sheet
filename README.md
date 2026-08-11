# WP-CLI Cheat Sheet

A practical collection of WP-CLI commands for WordPress administration, troubleshooting, and maintenance.

## About This Project

This cheat sheet documents commonly used WP-CLI commands for managing and troubleshooting WordPress installations from the command line.

The focus is on practical commands used for day-to-day WordPress administration, production troubleshooting, database management, plugin and theme management, and user administration.

> **Note:** Commands should be tested in a safe environment before being run against production sites. Always verify the target site and command before performing destructive operations.
---

## Getting Started

### Check WP-CLI Version

```bash
wp cli version
wp cli info
wp help plugin
wp help plugin list
---

## Plugin Management

### List Installed Plugins

```bash
wp plugin list
Lists the plugins installed on the WordPress site, including their status and version.

List Only Active Plugins
wp plugin list --status=active

Displays only currently active plugins.

Check Plugin Status
wp plugin status

Displays information about installed plugins and their current status.

Activate a Plugin
wp plugin activate plugin-name

Activates a specific plugin.

Deactivate a Plugin
wp plugin deactivate plugin-name

Deactivates a specific plugin.

Deactivate All Plugins
wp plugin deactivate --all

Deactivates all installed plugins.

Update a Plugin
wp plugin update plugin-name

Updates a specific plugin.

Update All Plugins
wp plugin update --all

Updates all installed plugins.

Install a Plugin
wp plugin install plugin-name

Downloads and installs a plugin.

Install and activate it immediately:

wp plugin install plugin-name --activate
Delete a Plugin
wp plugin delete plugin-name

Deletes a specific plugin.

Production note: Be careful with plugin activation, updates, and deletion on production sites. Verify the target environment and understand the potential impact before making changes.
