---
title: Soju IRC bouncer with Libera
date: 2025-07-16 20:44:53
tags:
- irc
---

Connecting to [Libera](https://libera.chat/) with [Halloy](https://halloy.chat/) via [Soju](https://codeberg.org/emersion/soju).

<!-- more -->

To connect to Libera with Soju, do the following:

1. Connect to Libera with your client of choice (I use Halloy) normally, register with NickServ.
1. On your server, install Soju (`apt install soju`)
1. Edit Soju's config (`/etc/soju/config`) to be this:
```ini
db sqlite3 /var/lib/soju/main.db
message-store fs /var/lib/soju/logs/
hostname 164.92.109.104
listen irc+insecure://0.0.0.0:6667
```
1. Run the user setup command in the Soju docs (`sojudb create-user <soju username> -admin`)
    - This username is not the username that you connect to Libera with, though it can be the same
1. Run/restart Soju (`systemctl restart soju`)
1. In Halloy's configuration, update your Libera server config to the following:
```toml
[servers.liberachat]
nickname = "<libera username>"
server = "164.92.109.104"
port = 6667
use_tls = false
username = "<soju username>/irc.libera.chat"
password = "<soju password>"
chathistory = true
```
1. Connect with Halloy. Soju should try to connect to Libera, but Libera will likely kill the connection citing requiring registration from your IP (if you're using a server by a well-used host like DigitalOcean).
1. Open a message to BouncerServ (`/msg BouncerServ`)
1. Enter these commands to set up SASL:
```plain
sasl set-plain <libera username> <libera password>
```
1. It should work.

I didn't know about messaging BouncerServ, so I manually edited the Soju sqlite DB with the following:

```sql
update network set sasl_mechanism = "PLAIN", sasl_plain_username = "<libera username>", sasl_plain_password = "<libera password>";
```
