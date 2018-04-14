# ccnet.conf

**Note**: Since Seafile Server 5.0.0, all config files are moved to the central **conf** folder. [Read More](../deploy/new_directory_layout_5_0_0.md).

Ccnet is the internal RPC framework used by Seafile server and also manages the user database. A few useful options are in ccnet.conf.

```
[General]

# Used internally. Don't delete.
ID=eb812fd276432eff33bcdde7506f896eb4769da0

# Used internally. Don't delete.
NAME=example

# This is outside URL for Seahub(Seafile Web). 
# The domain part (i.e., www.example.com) will be used in generating share links and download/upload file via web.
# Note: Outside URL means "if you use Nginx, it should be the Nginx's address"
SERVICE_URL=http://www.example.com:8000


[Network]
# Not used anymore
PORT=10001

[Client]
# Not used anymore
PORT=13419

```

**Note**: You should restart seafile so that your changes take effect.

```
cd seafile-server
./seafile.sh restart
```

## Changing MySQL Connection Pool Size

When you configure ccnet to use MySQL, the default connection pool size is 100, which should be enough for most use cases. You can change this value by adding following options to ccnet.conf:

```
[Database]
......
# Use larger connection pool
MAX_CONNECTIONS = 200
```
