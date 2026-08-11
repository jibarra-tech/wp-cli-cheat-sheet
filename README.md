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
