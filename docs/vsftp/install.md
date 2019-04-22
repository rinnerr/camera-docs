### Hướng dẫn cách cài đặt vsFTP

1.  https://www.linode.com/docs/uptime/logs/use-logrotate-to-manage-log-files/
2.  OS sử dụng CentOS 7
    *   Cài đặt
        *   repo epel **yum install -y epel-release**
        *   vsftp và các modules liên quan **yum install -y pam\_url vsftpd pam\_script**
            *   vsftp
                *   Virtual User là gì
                    *   Không phải users của OS system level
                    *   Users chỉ có ý nghĩa với FTP
            *   [pam\_url](https://github.com/mricon/pam_url)
                *   Dùng để chứng thực thông qua web services
            *   [pam\_script](https://github.com/jeroennijhof/pam_script)
                *   Dùng để tạo FTP user home folder
                *   pam\_script folder **/etc/pam-script.d/**
                *   pam\_script file execute **/etc/pam-script.d/pam\_script\_auth**
    *   Cấu hinh
        *   Disable selinux
