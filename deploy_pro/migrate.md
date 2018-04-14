# Migrate data between different backends

Seafile supports data migration between filesystem, s3, ceph, swift and Alibaba oss (migrating from swift is not supported yet, this support will be added in the future). If you enabled storage backend encryption feature, migration is not supported at the moment.

Data migration takes 3 steps:
1. Create a new temporary seafile.conf
2. Run migrate.sh
3. Replace the original seafile.conf

## Create a new temporary seafile.conf
We need to add new backend configurations to this file (including `[block_backend]`, `[commit_object_backend]`, `[fs_object_backend]` options) and save it under a readable path.
Let's assume that we are migrating data to S3 and create temporary seafile.conf under `/opt`
```
cat > seafile.conf << EOF
[commit_object_backend]
name = s3
bucket = seacomm
key_id = ******
key = ******

[fs_object_backend]
name = s3
bucket = seafs
key_id = ******
key = ******

[block_backend]
name = s3
bucket = seablk
key_id = ******
key = ******
EOF

mv seafile.conf /opt
```
Repalce the configurations with your own choice.

## Run migrate.sh
We assume you have installed seafile pro server under `~/haiwen`, enter `~/haiwen/seafile-server-latest` and run migrate.sh with parent path of temporary seafile.conf as parameter, here is `/opt`.
```
cd ~/haiwen/seafile-server-latest
./migrate.sh /opt
```

## Replace the original seafile.conf
After running the script, we need replace the original seafile.conf with new one:
```
mv /opt/seafile.conf ~/haiwen/conf
```
now we only have configurations about backend, more config options, e.g. memcache and quota, please refer to [this tutorial](https://manual.seafile.com/config/seafile-conf.html).

After replacing seafile.conf, you can restart seafile server and access the data on the new backend.

