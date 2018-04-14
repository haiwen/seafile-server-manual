# Server Configuration and Customization

**Note**: Since Seafile Server 5.0.0, all config files are moved to the central **conf** folder. [Read More](../deploy/new_directory_layout_5_0_0.md).

This manual explains how to change various config options for Seafile server.

There are three config files in the community edition:

- [ccnet.conf](ccnet-conf.md): contains the network settings
- [seafile.conf](seafile-conf.md): contains settings for seafile daemon and FileServer.
- [seahub_settings.py](seahub_settings_py.md): contains settings for Seahub

There is one additional config file in the pro edition:

- `seafevents.conf`: contains settings for ccnet/ccnet.search and documents preview




## Storage Quota Setting (seafile.conf)

You may set a default quota (e.g. 2GB) for all users. To do this, just add the following lines to `seafile.conf` file

```
[quota]
# default user quota in GB, integer only
default = 2
```

This setting applies to all users. If you want to set quota for a specific user, you may log in to seahub website as administrator, then set it in "System Admin" page.

## Default history length limit (seafile.conf)

If you don't want to keep all file revision history, you may set a default history length limit for all libraries.

```
[history]
keep_days = days of history to keep
```

## Seafile fileserver configuration (seafile.conf)

The configuration of seafile fileserver is in the `[fileserver]` section of the file `seafile.conf`

```
[fileserver]
# binding host for fileserver
host = 0.0.0.0
# tcp port for fileserver
port = 8082
```

Change upload/download settings.

```
[fileserver]
# Set maximum upload file size to 200M.
max_upload_size=200

# Set maximum download directory size to 200M.
max_download_dir_size=200
```

**Note**: You need to restart seafile and seahub so that your changes take effect.
```
./seahub.sh restart
./seafile.sh restart
```

## Seahub Configurations (seahub_settings.py)

#### Sending Email Notifications on Seahub

A few features work better if it can send email notifications, such as notifying users about new messages.
If you want to setup email notifications, please add the following lines to seahub_settings.py (and set your email server). 
See [Django email documentation](https://docs.djangoproject.com/en/1.10/topics/email/) for the full description of these variables.

```
EMAIL_USE_TLS = False
EMAIL_HOST = 'smtp.example.com'        # smpt server
EMAIL_HOST_USER = 'username@example.com'    # smtp authentication username
EMAIL_HOST_PASSWORD = 'password'    # smtp authentication password
EMAIL_PORT = '25'
DEFAULT_FROM_EMAIL = EMAIL_HOST_USER # value of email's From: field
SERVER_EMAIL = EMAIL_HOST_USER # error-reporting emails' From: field
```

If you are using Gmail as email server, use following lines:

```
EMAIL_USE_TLS = True
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_HOST_USER = 'username@gmail.com'
EMAIL_HOST_PASSWORD = 'password'
EMAIL_PORT = 587
DEFAULT_FROM_EMAIL = EMAIL_HOST_USER
SERVER_EMAIL = EMAIL_HOST_USER
```

**Note**: If your Email service still can not work, you may checkout the log file `logs/seahub.log` to see what may cause the problem. For complete email notification list, please refer to [Email notification list](customize_email_notifications.md).

**Note2**: If you want to use the Email service without authentication leaf `EMAIL_HOST_USER` and `EMAIL_HOST_PASSWORD` **blank** (`''`). (But notice that the emails then will be sent without a `From:` address.)

#### Cache

Seahub caches items(avatars, profiles, etc) on file system by default(/tmp/seahub_cache/). You can replace with Memcached (you have to install python-memcache first).

```
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
	'LOCATION': '127.0.0.1:11211',
    }
}
```

#### Seahub Settings

You may change seahub website's settings by adding variables in `seahub_settings.py`.

```

# Choices can be found here:
# http://en.wikipedia.org/wiki/List_of_tz_zones_by_name
# although not all choices may be available on all operating systems.
# If running in a Windows environment this must be set to the same as your
# system time zone.
TIME_ZONE = 'UTC'

# Set this to seahub website's URL. This URL is contained in email notifications.
SITE_BASE = 'http://www.example.com/'

# Set this to your website's name. This is contained in email notifications.
SITE_NAME = 'example.com'

# Set seahub website's title
SITE_TITLE = 'Seafile'

# If you don't want to run seahub website on your site's root path, set this option to your preferred path.
# e.g. setting it to '/seahub/' would run seahub on http://example.com/seahub/.
SITE_ROOT = '/'

# Whether to use pdf.js to view pdf files online. Default is `True`,  you can turn it off.
# NOTE: since version 1.4.
USE_PDFJS = True

# Enalbe or disalbe registration on web. Default is `False`.
# NOTE: since version 1.4.
ENABLE_SIGNUP = False

# Activate or deactivate user when registration complete. Default is `True`.
# If set to `False`, new users need to be activated by admin in admin panel.
# NOTE: since version 1.8
ACTIVATE_AFTER_REGISTRATION = False

# Whether to send email when a system admin adding a new member. Default is `True`.
# NOTE: since version 1.4.
SEND_EMAIL_ON_ADDING_SYSTEM_MEMBER = True

 # Whether to send email when a system admin resetting a user's password. Default is `True`.
# NOTE: since version 1.4.
SEND_EMAIL_ON_RESETTING_USER_PASSWD = True

# Hide `Organization` tab.
# If you want your private seafile behave exactly like https://cloud.seafile.com/, you can set this flag.
CLOUD_MODE = True

# Online preview maximum file size, defaults to 30M.
FILE_PREVIEW_MAX_SIZE = 30 * 1024 * 1024

# Age of cookie, in seconds (default: 2 weeks).
SESSION_COOKIE_AGE = 60 * 60 * 24 * 7 * 2

# Whether to save the session data on every request.
SESSION_SAVE_EVERY_REQUEST = False

# Whether a user's session cookie expires when the Web browser is closed.
SESSION_EXPIRE_AT_BROWSER_CLOSE = False

# Using server side crypto by default, otherwise, let user choose crypto method.
FORCE_SERVER_CRYPTO = True

```

**Note**:

* You need to restart seahub so that your changes take effect.
* If your changes don't take effect, You may need to delete 'seahub_setting.pyc'. (A cache file)

```
./seahub.sh restart
```
