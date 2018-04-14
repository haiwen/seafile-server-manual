# Download and Setup Seafile Windows Server

## Download/Uncompress
### Install Python 2.7.11 32bit

- Download and install [python 2.7.11 32bit](https://www.python.org/ftp/python/2.7.13/python-2.7.13.msi)
- Add the installation path of python2.7 to the system PATH environment variable. If you installed python 2.7 to ``C:\Python27`` add ``C:\Python27`` to the PATH environment variable.


**Warning**: Be sure to use Python 2.7.13 32bit. 64bit and other versions don't work.

### Download/Uncompress Seafile Server

- Get the latest version of Seafile Server program
- Create a new folder to store seafile program, such as ``C:\SeafileProgram\``. Please remember the location of the folder, we'll use it later.
- Uncompress ``seafile-server_5.1.3_win32.tar.gz`` to ``C:\SeafileProgram\``

Now you have a folder like this:
```sh
C:\SeafileProgram
         |__ seafile-server-5.1.3
```
## Start/Initialization

### Start Seafile Server

Go to the folder ``C:\SeafileProgram\seafile-server-5.1.3\``, and double click run.bat to start Seafile Server. You should notice a seafile icon appear in the system tray.

### Choose a disk to store Seafile Server data

If there is more than one disk available, you will now be prompted a with dialog to choose the disk on which to store the data of seafile server.

- Please choose a disk with enough free space
- Once you have clicked the OK button, Seafile will create a folder named seafile-server on the disk you have chosen. This is the data folder for Seafile Server. For example, if you choose disk D, your data folder will be ``D:\seafile-server``

### Add an admin account

Right click the tray icon of Seafile Server and choose __Add an admin account__. Input your admin username and password in the dialog prompt.

If the operation is successful, the tray icon will show a bubble saying __Successfully added the admin account__

### Configure Seafile Server

After initialization, there are some options that need to be configured:

- Right click the tray icon, choose __Open seafile-server folder__. Your seafile-server data folder will open.
- Open the file `conf/ccnet.conf` and modify the following line. (You can also modify SERVICE_URL via web UI in "System Admin->Settings". **Warning**: if you set the value both via Web UI and ccnet.conf, the setting via Web UI will take precedence.)
```
SERVICE_URL = XXX
```

- Change the value of `SERVICE_URL` to `http://<your ip address>:8000`. Say the ip address of your windows server is 192.168.1.100, then change it to `SERVICE_URL = http://192.168.1.100:8000`

After the edit, right click tray icon and choose __Restart seafile__

### Visit Seahub

Open your browser and visit `http://127.0.0.1:8000`. Login with the admin account. If you can login, the initialization is successful.

Seafile Server configuration is complete. Please see the Seafile Client Manual for how to use the client.

### You may also want to read about:

- [Deploy Seafile with MySQL](deploy_with_mysql.md)
- [Deploy Seafile with Apache](deploy_with_apache.md)
- [Deploy Seafile with Nginx](deploy_with_nginx.md)
- [LDAP Integration](using_ldap.md)
- [Install Seafile Server as a Windows Service](install_seafile_server_as_a_windows_service.md)
- [Ports used by Seafile Windows Server](ports_used_by_seafile_windows_server.md)
- [Upgrading Seafile Windows Server](upgrading_seafile_windows_server.md)
- [Options & Customization](../config/README.md)
