# Setup Memcached Cluster and MariaDB Galera Cluster

For high availability, it is recommended to set up a memcached cluster and MariaDB Galera cluster for Seafile cluster. This documentation will provide information on how to do this with 3 servers. You can either use 3 dedicated servers or use the 3 Seafile server nodes.

## Setup Memcached Cluster

Seafile servers share session information within memcached. So when you set up a Seafile cluster, there needs to be a memcached server (cluster) running.

The simplest way is to use a single-node memcached server. But when this server fails, some functions in the web UI of Seafile cannot work. So for HA, it's usually desirable to have more than one memcached servers.

Unlike other cluster architecture, key distribution in memcached cluster is controlled by the memcached clients. So there is no special configuration on the memcached server for building a cluster. But there are a few things to take care when building a memcached cluster:

- Make sure all the seafile server nodes connects to all the memcached nodes. The memcached servers should be listed in the same order in Seafile's config files.
- After one memcached server gets shut down and restarted, sometimes the Seafile servers' views on the memcached cluster will become inconsistent. This is due to limitation of the memcached cluster architecture. You may notice some errors in the web UI functionalities. You have to restart the Seafile server processes to make their views consistent again. Typical error messages you can find in seafile.log are:
    * `SERVER HAS FAILED AND IS DISABLED UNTIL TIMED RETRY`
    * `SERVER IS MARKED DEAD`

Seafile servers, work as memcached clients, are designed to automatically migrate keys to living memcached nodes when a memcached node fails. So a memcached node failure usually doesn't affect the service availability.

### Setup MariaDB Cluster

MariaDB cluster helps you to remove single point of failure from the cluster architecture. Every update in the database cluster is synchronously replicated to all instances.

You can choose between two different setups:

- For a small cluster with 3 nodes, you can run MariaDB cluster directly on the Seafile server nodes. Each Seafile server access its local instance of MariaDB.
- For larger clusters, it's preferable to have 3 dedicated MariaDB nodes to form a cluster. You have to set up a HAProxy in front of the MariaDB cluster. Seafile will access database via HAProxy.

We refer to the documentation from MariaDB team:

- [Setting up MariaDB cluster on CentOS 7](https://mariadb.com/resources/blog/setting-mariadb-enterprise-cluster-part-2-how-set-mariadb-cluster)
- [Setting up HAProxy for MariaDB Galera Cluster](https://mariadb.com/resources/blog/setup-mariadb-enterprise-cluster-part-3-setup-ha-proxy-load-balancer-read-and-write-pools). Note that Seafile doesn't use read/write isolation techniques. So you don't need to setup read and write pools.
