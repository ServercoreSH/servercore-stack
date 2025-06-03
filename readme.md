# Servercore Stack

**A hardened and modular LEMP stack in pure Bash — designed for performance, auditability, and security.**

Servercore is a fork of SlickStack, rewritten and reorganized to serve as a secure, minimal, and fully auditable foundation for WordPress and general PHP/MySQL web infrastructure. It is designed for system administrators, agencies, and professionals who prioritize simplicity, control, and long-term stability.

---

## Installation

Servercore is written entirely in Bash, with no external dependencies. It runs on Ubuntu LTS cloud servers (KVM-based), and requires only basic system tools. Unlike heavier tools like Ansible or Docker, Servercore avoids complexity by sticking to native shell scripting.

The following steps assume you’ve already deployed a KVM-based Ubuntu LTS server with at least 1GB of RAM, and that you’re logged in as `root` via SSH.

```bash
cd /tmp/ && wget -O sc https://raw.githubusercontent.com/ServercoreSH/servercore-stack/main/sc-install && bash sc
```

From this point forward, you can manage your Servercore server by simply using the `sudo bash` command on any one of the bundled scripts located within the `/var/www/` directory, as needed. However, in most cases there shouldn't be any need for much hands-on management as the server will intelligently run various cron jobs which connect to this GitHub repo.

You can safely re-install Servercore anytime via `sudo bash /var/www/sc-install` without causing any conflicts or data loss since the installation process is completely idempotent.

**Note:** Servercore requires Cloudflare to be activated on your domain before SSL (HTTPS) will be recognized as fully secure by your browser, because of its self-signed OpenSSL certificate. If you wish to use Let's Encrypt instead, be sure to change your settings in `sc-config` before running the installation.

## Modules

*Last updated: Jun 03, 2025*

| Module | Version | What does Servercore optimize? |
| :------------- | :----------: | :----------: |
| **Ubuntu LTS** | 24.04 | `crontab` + `gai.conf` + `sshd_config` + `sudoers` + `sysctl.conf` |
| **Nginx** | 1.18.x | `nginx.conf` + `cloudflare.conf` + server blocks |
| **OpenSSL** | 3.0.x | `servercore.crt` + `servercore.key` + `dhparam.pem` |
| **Certbot** | 2.9.x | `cert.perm` + `chain.pem` + `fullchain.pem` + `privkey.pem` |
| **MySQL** | 8.0.x | `my.cnf` |
| **PHP-FPM** | 8.3.x | `php.ini` + `php-fpm.conf` + `www.conf` |
| **Memcached** | 1.6.x | `memcached.conf` + `object-cache.php` |
| **WordPress** | 6.7.x | some WP Core junk files removed by `sc-clean-files` |
| **WP-CLI** | 2.12.x | some `wp` commands disabled |
| **Adminer** | 4.8.1 | default config |
| **Iptables** | 1.8.x | `rules.v4` + `rules.v6` |
| **Fail2ban** | 1.0.x | `jail.local` + custom filters |

## Requirements

Servercore requires an Ubuntu LTS 22.04+ server (KVM-based VPS recommended), with 1–2 GB of RAM, root SSH access, and Cloudflare DNS enabled before installation.

*Note: Servercore is intended for single-site deployments, not multi-tenant shared hosting. WordPress Multisite is supported, but the system is built around simplicity and isolation for maximum performance and security.*

Servercore works best on KVM cloud servers with at least 2 GB of RAM, such as DigitalOcean, Vultr, or Linode. The LEMP stack is primarily designed for high-traffic, single-site WordPress deployments. Servercore supports WordPress, WooCommerce, bbPress, and BuddyPress out of the box with optimized settings that scale, including support for WordPress Multisite. This means you can upgrade your cloud server at any time, then run `sc-install` again, and most Servercore settings will automatically adjust based on available resources.

By default, MySQL will connect locally via TCP to `127.0.0.1:3306` databases called `production`, `staging`, and `development` (depending on whether you have enabled staging/dev sites or not), although remote databases also work very well. Server "clustering" or "load balancing" has not been tested, and is not the goal here; complex enterprise-style configurations for WordPress are rarely needed (and can be expensive and difficult to manage), thus Servercore aims to provide a simple solution for the 99% of WordPress sites that don't need such complexity.

It should also be noted that Servercore is HTTPS-only, and that HSTS is enabled by default, meaning that HTTP sites are not supported. Because OpenSSL generates self-signed certificates, Servercore servers require Cloudflare to be active in front of your server in order for SSL certificates to be properly CA-signed and loaded by your browser, at least until the first `sc-install` has been completed (after that, you can switch to Certbot / Let's Encrypt).

## Design Philosophy

Modern hosting trends have moved toward abstraction, automation, and centralized SaaS platforms. While this is efficient at scale, many developers and agencies still require transparent, controllable, and self-hosted solutions.

Servercore exists for those who:
	•	Prefer to run their own secure infrastructure
	•	Want to understand and control what’s running
	•	Need a stack they can audit, extend, or harden without vendor lock-in

It is not a website builder. It is not a control panel.
It is a clean and minimal server framework designed to support real-world production sites with modern security and reliability standards.

## License

This project is open source under the MIT License.
