# Setup a Firewall with UFW on Ubuntu

[DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server)

Previously, setting up a firewall was done through complicated or arcane utilities. Many of these utilities (e.g., `iptables`) have a lot of functionality built into them, but do require extra effort from the user to learn and understand them.

Another option is **UFW, or Uncomplicated Firewall**. UFW is a front-end to iptables that aims to provide a more user-friendly interface than other firewall management utilities.

## Setting Up UFW Defaults

UFW’s default is to deny all incoming connections and allow all outgoing connections.

```bash
$ sudo ufw default deny incoming

Default incoming policy changed to 'deny'
(be sure to update your rules accordingly)

```

```bash
$ sudo ufw default allow outgoing

Default outgoing policy changed to 'allow'
(be sure to update your rules accordingly)
```

## Allowing Connections to the Firewall

If you turned on your firewall now, for example, it would deny all incoming connections. If you’re connected over SSH to your server, this would be a problem because you would be locked out of your server. Prevent this from happening by enabling SSH connections to your server:

```bash
$ sudo ufw allow ssh

Rule added
Rule added (v6)
```

### Securing Web Servers

To secure a web server with File Transfer Protocol (FTP) access, you’ll need to allow connections for port 80/tcp.

Allowing connections for port 80 is useful for web servers such as Apache and Nginx that listen to HTTP connection requests.

```bash
sudo ufw allow 80/tcp
```

### Specifying IP Addresses

You can allow connections from a specific IP address such as in the following.

```bash
sudo ufw allow from your_server_ip
```

## Enabling UFW

Once you’ve defined all the rules you want to apply to your firewall, you can enable UFW so it starts enforcing them. If you’re connecting via SSH, make sure to set your SSH port, commonly port 22, to allow connections to be received. Otherwise, you could lock yourself out of your server:

```bash
$ sudo ufw enable

Firewall is active and enabled on system startup
```

To confirm your changes went through, check the status to review the list of rules:

```bash
$ sudo ufw status

Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
22/tcp                     ALLOW       Anywhere
2222/tcp                   ALLOW       Anywhere
20/tcp                     ALLOW       Anywhere
80/tcp                     DENY        Anywhere
…
```
