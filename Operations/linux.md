# Linux Cheatsheet

## Setup a Firewall with `ufw`(Uncomplicated Firewall) on an Ubuntu and Debian Cloud Server

### TL;DR

- Check status:

```bash
sudo ufw status
```

- Restart (needed when configurations changed):

```bash
sudo ufw status
```

- Allow connections:

```bash
sudo ufw allow ssh
```

```bash
sudo ufw allow 9000/tcp
```

- List of User Groups

```bash
cat /etc/group
```

- Add user to existing group

```bash
# -a (append), otherwise, the user will be removed from any groups, not in the list;
# -G takes a (comma-separated) list of additional groups to assign the user to;
usermode -a -G groupName userName
```

## Reference

[How To Setup a Firewall with UFW on an Ubuntu and Debian Cloud Server](https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server#what-is-ufw)