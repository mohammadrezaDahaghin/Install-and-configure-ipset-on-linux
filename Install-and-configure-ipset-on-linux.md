# Install and configure ipset on linux

## install ipset and ipset-persistent:

```bash
sudo apt install ipset ipset-persistent
```

Create a list called demolist that stores a set of IPs inside:

```bash
sudo ipset create demolist hash:ip
```

View the details of the list we have created:

```bash
mrdex@srv1:~$ sudo ipset list demolist
Name: demolist
Type: hash:ip
Revision: 5
Header: family inet hashsize 1024 maxelem 65536 bucketsize 12 initval 0x57e41446
Size in memory: 200
References: 0
Number of entries: 0
Members:
```

Add the desired IP to the list of members:

```bash
sudo ipset add demolist 10.10.10.3
```

To add a rule to iptable:

```bash
sudo iptables -A INPUT -m set --match-set demolist src -j ACCEPT
```

open the sysctl.conf file and set the “net.ipv4.ip_forward” parameter to one by uncommenting it:

```bash
sudo vim /etc/sysctl.conf
```

Now enable the changes to above file using the command:

```bash
mrdex@srv1:~$ sudo sysctl -p
net.ipv4.ip_forward = 1
```

Now, to check if everything is working fine, ping any public IP from the client:

![Screenshot 2024-04-04 135937.png](Install%20and%20configure%20ipset%20on%20linux%203bf3ef99e2b146ebaabab0211a624116/Screenshot_2024-04-04_135937.png)