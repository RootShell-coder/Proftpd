# Proftpd

- The `mod_exec` module in ProFTPD allows the execution of external programs or scripts in response to various FTP commands such as STOR, RETR, QUIT, and others. This module is useful for integrating the FTP server with other systems or for performing arbitrary actions in response to user actions, such as uploading or downloading files.

- The `mod_lang` module in ProFTPD allows you to configure support for different languages for messages displayed to the user when interacting with the FTP server. This module is used for localization and translation of standard error messages, warnings, and other text that is output to the client during FTP sessions.

---

## Environments

PROFTPD_USER: "`<user>`:`<password>`:`<uid>`:`<gid>`"

PROFTPD_MASQUERADE_ADDRESS: "`proftpd`" usually only the service name is enough

PROFTPD_PASSIVE_PORTS: "`30000-30100`" FTP passive ports

TZ: "`Europe/London`" Timezone

LANG: "`en_GB.UTF8`" Locale

LC_ALL: "`en_GB.UTF-8`"

## Processing

The `mod_exec` module actively uses the `processing` file. When the STOR event is triggered, it writes a `.mod_exec.test` file in the user's home directory with the following content: `127.0.0.1 user1 /home/user1/new file.txt STOR`. Configured in the `exec.conf` file.

### exec.conf

```conf
<IfModule mod_exec.c>
  ExecEngine on
  ExecLog /var/log/proftpd/exec.log
  ExecOnCommand STOR /usr/local/bin/processing %a %u %f %m %b
</IfModule>

```

---

Users are locked into their home directories with `Limit`, but this is not a `chroot` directory.
