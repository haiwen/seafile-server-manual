# Deploy Seafile with Apache

## With Apache 2.4

### Preparation

Load Modules

#### Edit httpd.conf

First edit your `httpd.conf`. Add the following lines to **the end of the file**:

```
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so
LoadModule proxy_http_module modules/mod_proxy_http.so
LoadModule rewrite_module modules/mod_rewrite.so
Include conf/extra/httpd-vhosts.conf
```

And please ensure this line does NOT exist:

```
DocumentRoot "${SRVROOT}/htdocs"
```

### Deploy Seahub/FileServer With Apache 2.4

Seahub is the web interface of Seafile server. FileServer is used to handle raw file uploading/downloading through browsers. By default, it listens on port 8082 for HTTP request.

Here we deploy Seahub using fastcgi, and deploy FileServer with reverse proxy. We assume you are running Seahub using domain '''www.myseafile.com'''.

#### Edit your httpd-vhosts.conf

Modify Apache config file: (conf/extra/httpd-vhosts.conf)

Assume you have uncompresssed seafile server into `C:/seafile`.

If you need to use the IP address to access the seafile service directly.Please unconfigure the `<VirtualHost _default_:80>` section.

Then add the following lines:

```
<VirtualHost *:80>
    ServerName www.myseafile.com
    DocumentRoot "${SRVROOT}/htdocs"
    Alias /media  "C:/seafile/seafile-server-6.0.7/seahub/media"

    RewriteEngine On

    <Location /media>
        Require all granted
    </Location>

    #
    # seafile fileserver
    #
    ProxyPass /seafhttp http://127.0.0.1:8082
    ProxyPassReverse /seafhttp http://127.0.0.1:8082
    RewriteRule ^/seafhttp - [QSA,L]

    #
    # seahub
    #
    SetEnvIf Request_URI . proxy-fcgi-pathinfo=unescape
    SetEnvIf Authorization "(.*)" HTTP_AUTHORIZATION=$1
    ProxyPass / fcgi://127.0.0.1:8000/
</VirtualHost>
```

#### Modify seafile/seafile.conf

Modify the `seahub` section of `seafile/seafile.conf`:

```
[seahub]
port = 8000
fastcgi = true
```

### Modify ccnet.conf and seahub_setting.py

#### Modify ccnet.conf

You need to modify the value of `SERVICE_URL` in [ccnet.conf](../config/ccnet-conf.md)
to let Seafile know the domain you choose. You can also modify SERVICE_URL via web UI in "System Admin->Settings". (**Warning**: if you set the value both via Web UI and ccnet.conf, the setting via Web UI will take precedence.)

```python
SERVICE_URL = http://www.myseafile.com
```

Note: If you later change the domain assigned to seahub, you also need to change the value of  `SERVICE_URL`.

#### Modify seahub_settings.py

You need to add a line in `seahub_settings.py` to set the value of `FILE_SERVER_ROOT`. You can also modify `FILE_SERVER_ROOT` via web UI in "System Admin->Settings". (**Warning**: if you set the value both via Web UI and seahub_settings.py, the setting via Web UI will take precedence.)

```python
FILE_SERVER_ROOT = 'http://www.myseafile.com/seafhttp'
```

### Please restart Seafile Server and httpd.


## With Apache 2.2

### Preparation

#### Install mod_fastcgi

Download [mod_fastcgi-*.dll] (http://fastcgi.com/dist/) first, and put it into the `modules/` directory of your Apache installation.

**Note**: You must download the right version of `mod_fastcgi` DLL according for your Apache. For example:

- If you are using Apache 2.2, you should download http://fastcgi.com/dist/mod_fastcgi-2.4.6-AP22.dll. The **AP22** part of the dll indicate it's for Apache **2.2**.
- If you are using Apache 2.0, you should download http://fastcgi.com/dist/old/mod_fastcgi-2.4.2-AP20.dll

## Deploy Seahub/FileServer With Apache

Seahub is the web interface of Seafile server. FileServer is used to handle raw file uploading/downloading through browsers. By default, it listens on port 8082 for HTTP request.

Here we deploy Seahub using fastcgi, and deploy FileServer with reverse proxy. We assume you are running Seahub using domain '''www.myseafile.com'''.

### Edit httpd.conf

First edit your `httpd.conf`. Add the following lines to **the end of the file**:

```
LoadModule fastcgi_module modules/mod_fastcgi-2.4.6-AP22.dll
LoadModule rewrite_module modules/mod_rewrite.so
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_http_module modules/mod_proxy_http.so
Include conf/extra/httpd-vhosts.conf
```

Then add this line (substitute `YourDocumentRoot` with the value of your apache `DocumentRoot`)

```
FastCGIExternalServer "YourDocumentRoot/seahub.fcgi" -host 127.0.0.1:8000
```

Note, `seahub.fcgi` is just a placeholder, you don't need to actually have this file in your system.

### Edit your httpd-vhosts.conf

Assume you have uncompresssed seafile server into `C:/SeafileProgram/seafile-pro-server-2.1.4`.

```
<VirtualHost *:80>
  ServerName www.myseafile.com
  Alias /media  "C:/SeafileProgram/seafile-pro-server-2.1.4/seahub/media"

  RewriteEngine On

  #
  # seafile fileserver
  #
  ProxyPass /seafhttp http://127.0.0.1:8082
  ProxyPassReverse /seafhttp http://127.0.0.1:8082
  RewriteRule ^/seafhttp - [QSA,L]

  #
  # seahub
  #
  RewriteRule ^/(media.*)$ /$1 [QSA,L,PT]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^(.*)$ /seahub.fcgi$1 [QSA,L,E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
</VirtualHost>

<Directory "C:/SeafileProgram/seafile-pro-server-2.1.4/seahub/media">
    Options Indexes FollowSymLinks
    AllowOverride None
    Order allow,deny
    Allow from all
</Directory>
```

## Modify Configurations

### Modify ccnet.conf

You can also modify SERVICE_URL via web UI in "System Admin->Settings". (**Warning**: if you set the value both via Web UI and ccnet.conf, the setting via Web UI will take precedence.)

```
SERVICE_URL = http://www.myseafile.com
```

Note: If you later change the domain assigned to seahub, you also need to change the value of  `SERVICE_URL`.

### Modify seafile-data/seafile.conf

Modify the `seahub` section of `seafile-data/seafile.conf`:

```
[seahub]
port=8000
fastcgi=true
```

### Modify seahub_settings.py

You need to add a line in `seahub_settings.py` to set the value of `FILE_SERVER_ROOT`. You can also modify `FILE_SERVER_ROOT` via web UI in "System Admin->Settings". (**Warning**: if you set the value both via Web UI and seahub_settings.py, the setting via Web UI will take precedence.)

```
FILE_SERVER_ROOT = 'http://www.myseafile.com/seafhttp'
```

## Notes when Upgrading Seafile Server

When upgrading seafile server, besides the normal steps you should take, there is one extra step to do: '''Update the path of the static files in your apache configuration'''. For example, assume your are upgrading seafile server 2.1.4 to 2.1.5, then:

```
<VirtualHost *:80>
  ...
  Alias /media  "C:/SeafileProgram/seafile-pro-server-2.1.5/seahub/media"
  ...
</VirtualHost>

<Directory "C:/SeafileProgram/seafile-pro-server-2.1.5/seahub/media">
  ...
</Directory>
```
