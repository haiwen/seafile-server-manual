# Ports used by Seafile Windows Server

Seafile server has two components, so two TCP ports are used.

## The two configuration files

All ports related configuration are recorded in ``ccnet.conf`` and ``seafile.conf``.
### How to open ``ccnet.conf``

- Right click the seafile server trayicon, choose __Open seafile-server folder__
- Open the folder ``ccnet`` under ``seafile-server`` folder. The file ``ccnet.conf`` is there.

### How to open ``seafile.conf``

- Right click the seafile server trayicon, choose __Open seafile-server folder__
- Open the folder ``seafile-data`` under ``seafile-server`` folder. The file ``seafile.conf`` is there.

In the following section we list the TCP ports used by each of seafile components, as well as how to change them (For example, some port may have already been used by some other application).

**Note**: If you change any of the ports, you have to restart seafile server.

## seafile fileserver

seafile fileserver handles raw file upload/download for Seahub

- default: 8082
- How to change: The Seafile desktop client will try to connect this port for file syncing. Don't change this port.

## seahub

seahub is the web interface of seafile server.

**Note**: If you change the port of seahub, you need to change the ``SERVICE_URL`` in ``ccnet.conf``.

- default: 8000
- How to change: Edit the file ``seafile.conf``. Change the value of port under the seahub section. (This is added in Seafile Windows Server 1.7.0.1)

```
[seahub]
port=8000
```
- Edit the file ``ccnet.conf``, modify the value of ``SERVICE_URL`` accordingly. For example, if you have changed the port to 8001, then modify the value of ``SERVICE_URL`` accordingly:

```
[General]
SERVICE_URL = <Your IP OR DOMAIN>:8001
```
