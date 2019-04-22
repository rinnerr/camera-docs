1.  vsFTP
    *   [Tham khao](https://www.linode.com/docs/uptime/logs/use-logrotate-to-manage-log-files/)
    *   Cấu hình
        *   vsftpd
            *   Tạo home directory `/data/ftp` cho các FTP users

        ```bash
            mkdir /data/ftp									#create root directory for all ftp user
            mkdir /etc/vsftpd/vconf							#create vsftpd virtual users directory config
            useradd -s /sbin/nologin -d /data/ftp vsftpd	#create a local virtual user
            chown -R vsftpd:vsftpd /etc/vsftpd/vconf
            chown -R vsftpd:vsftpd /data/ftp				#change owner root directory for all ftp user
        ```

        *   Configuration for vsftpd's pam `/etc/pam.d/vsftpd`

            ```
            # Auth in Web API
            auth       required  pam_url.so config=/etc/pam_url.conf
            # Running script if user have no root's directory
            # the script file name shoule be pam_script_auth
            auth       required pam_script.so onerr=success dir=/etc
            # Account in Web API
            account    required   pam_url.so config=/etc/pam_url.conf
            ```

        *   vsftpd config file `/etc/vsftpd/vsftpd.conf`

            ```
            # Allow anonymous FTP? (Beware - allowed by default if you comment this out).
            anonymous_enable=NO
            # Uncomment this to allow local users to log in.
            # When SELinux is enforcing check for SE bool ftp_home_dir
            local_enable=YES
            # Uncomment this to enable any form of FTP write command.
            write_enable=YES
            # Default umask for local users is 077. You may wish to change this to 022,
            # if your users expect that (022 is used by most other ftpd's)
            local_umask=022
            # Activate directory messages - messages given to remote users when they
            # go into a certain directory.
            dirmessage_enable=YES
            # Activate logging of uploads/downloads.
            xferlog_enable=YES
            # Make sure PORT transfer connections originate from port 20 (ftp-data).
            connect_from_port_20=YES
            # verbose ftp log, should disable xferlog_std_format=NO
            log_ftp_protocol=YES
            # You may change the default value for timing out an idle session.
            idle_session_timeout=600
            # You may change the default value for timing out a data connection.
            data_connection_timeout=120
            # When "listen" directive is enabled, vsftpd runs in standalone mode and
            # listens on IPv4 sockets. This directive cannot be used in conjunction
            # with the listen_ipv6 directive.
            listen=YES
            listen_ipv6=NO
            vsftpd_log_file=/var/log/vsftpd.log
            nopriv_user=vsftpd
            pam_service_name=vsftpd
            userlist_enable=YES
            tcp_wrappers=YES
            allow_writeable_chroot=YES
            guest_enable=YES
            guest_username=vsftpd
            local_root=/data/ftp/$USER
            user_sub_token=$USER
            virtual_use_local_privs=YES
            # enable ls/dir commandline
            dirlist_enable=NO
            user_config_dir=/etc/vsftpd/vconf
            ```

        *   Configuration pam\_url `/etc/pam_url.conf`

            ```
            pam_url:
            {
                settings:
                {
                    url         = "http://127.0.0.1:5000/account/check"; # URI to fetch
                    returncode  = "OK";                        # The remote script/cgi should return a 200 http code and this string as its only results
                    userfield   = "username";                  # userfield name to send
                    passwdfield = "password";                  # passwdfield name to send
                    extradata   = "&do=login";                 # extra data to send
                    prompt      = "Token: ";                   # password prompt
                };
                ssl:
                {
                    verify_peer = true;                               # Verify peer?
                    verify_host = true;                               # Make sure peer CN matches?
                    client_cert = "/etc/pki/tls/certs/totpcgi.crt";   # Client-side certificate
                    client_key  = "/etc/pki/tls/private/totpcgi.pem"; # Client-side key
                    ca_cert     = "/etc/pki/tls/certs/ca-bundle.crt"; # ca cert - defaults to ca-bundle.crt
                };
            };
            ```

        *   Configuration pam\_script `/etc/pam-script.d/pam_script_auth`

            ```bash
            #!/bin/sh
            if [ ! -d "/data/ftp/$PAM_USER" ]; then
            /usr/bin/env mkdir /data/ftp/$PAM_USER
            /usr/bin/env chown vsftpd:vsftpd /data/ftp/$PAM_USER
            fi
            ```

2.  Viết API sử dụng Flask framework using for vsftp's AAA
    *   python 3.6.x
        *   Tạo python virtual enviroment `python3 -m venv venv`
            *   Active python với virtual enviroment `source venv/bin/active`
        *   Cài đặt các packages liên quan
            *   flask
            *   flask-migrate
            *   flask-sqlalchemy
            *   gunicorn
            *   prometheus-flask-exporter
            *   example: `pip install flask flask-sqlalchemy flask-migrate gunicorn prometheus-flask-exporter`
        *   [download FTP-API code from git.yeahspace.net](http://git.yeahspace.net/hadn4/system/raw/master/project-vsftp/ftp-api.tar.gz)
    *   Tạo folder log cho flask
        *   `sudo mkdir -p /var/log/flask/gunicorn` #Tạo folder cho gunicorn and log
        *   `sudo mkdir /var/run/flask` #Tạo folder cho flask run
        *   `sudo chown -R hadn: /var/run/flask`
        *   `sudo chown -R hadn: /var/log/flask`
    *   Dùng sqlite làm database
        *   database schema

            ```bash
            CREATE TABLE  accounts (
                id int(11) AUTO_INCREMENT PRIMARY KEY,
                username varchar(100) NOT NULL,
                password varchar(100) NOT NULL,
                active int(11) NOT NULL,
                timestamp DATETIME NOT NULL
            );
            ```

    *   Cấu trúc folders của API

        ```bash
        ├── api
        │   ├── __init__.py
        │   ├── models
        │   │   └── accounts.py
        │   ├── templates
        │   └── views
        │       └── accounts.py
        ├── app.db
        ├── config.py
        ├── gunicorn
        │   └── config.py
        ├── migrations
        │   ├── alembic.ini
        │   ├── env.py
        │   ├── README
        │   ├── script.py.mako
        │   └── versions
        │       └── 8a603ecacfb2_.py
        ├── requirements.txt
        ├── run.py
        └── systemd
            └── env
        ```

    *   Flask routes

        ```bash
        Endpoint        Methods  Rule
        --------------  -------  -----------------------
        account.check   POST     /account/check
        account.create  POST     /account/create
        account.delete  POST     /account/delete
        account.index   GET      /account/
        static          GET      /static/<path:filename>
        ```

    *   URL của API
        *   URL tạo account **/account/create**
            *   Methods **POST**
            *   Parameters
                *   username
                *   password
        *   URL xóa account **/account/delete**
            *   Methods **POST**
            *   Parameters
                *   username
                *   password
        *   URL kiểm tra account **/account/check**
            *   Methods **POST**
            *   Parameters
                *   username
                *   password
    *   Tạo systemc file `/etc/systemd/system/flask.service`

        ```bash
        [Unit]
        Description = VsFTP for FLASK
        After = network.target
        [Service]
        Type=simple
        # using systemd environment file
        # should be edit the file for mapping to real system
        EnvironmentFile=-/home/hadn/ftp-api/systemd/env
        LimitNOFILE=16384
        PermissionsStartOnly = true
        User = hadn
        Group = hadn
        # should be create python virtual environment
        # start gunicorn at flask source code
        # check cpu cores - cat /proc/cpuinfo | grep 'core id' | wc -l
        # flask service start with user hadn and hadn's home folder - /home/hadn
        ExecStart = /home/hadn/ftp-api/venv/bin/gunicorn  -c ${gunicorn_config_file} run:app -b 127.0.0.1:5000 -w 4 --pid ${PIDFile}
        ExecReload = /bin/kill -s HUP $MAINPID
        ExecStop = /bin/kill -s TERM $MAINPID
        ExecStopPost = /bin/rm -rf $PIDFile
        PrivateTmp = true
        [Install]
        WantedBy=multi-user.target
        ```

    *   Nginx configuration file

        ```bash
        server {
                listen       80;
                server_name  prod-ftp.iotc.vn;
                #charset koi8-r;
                location / {
                    proxy_pass http://127.0.0.1:5000/;
                    proxy_http_version 1.1;
                    proxy_set_header Upgrade $http_upgrade;
                    proxy_set_header Connection "upgrade";
                }
        }
        ```
