# expressive-syslog

## Work needed

### dpkg

- No syslog option
- [Workaround via logger available](https://unix.stackexchange.com/questions/192877/logging-apt-dpkg-activity-to-syslog)
- [Issue (2015) with patch (2016) exists](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=781324)
- No severity levels implemented

### freshclam

Those all have level 'notice':

```
WARNING: Your ClamAV installation is OUTDATED!
WARNING: Local version: 0.99 Recommended version: 0.99.2
[...]
ERROR: NotifyClamd: Can't find or parse configuration file /etc/clamav/clamd.conf
[...]
Can't connect to port 80 of host db.local.clamav.net (IP: XXX.XXX.XXX.XXX)
```

### gitlab-runner

- [Issue (2016) exists](https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/issues/1018)
- Single wrong severity

journalalert filter

```yaml
 - identifier: gitlab-runner
   message: '*Build failed: exit status *'
```

### libvirtd

- Unsure what upstream situation is (only observed on Debian Jessie). In principle the messages could be imported in some situations. For us they only appeared on reboot.

journalalert filter

```yaml
 - command: libvirtd
   message: 'stream aborted at client request'
 - command: libvirtd
   message: 'internal error: End of file from monitor'
 - command: libvirtd
   message: 'error from service: TerminateMachine: No machine *'
```

### mariadb/mysql

- [Issue (2015) exists](https://jira.mariadb.org/browse/MDEV-9054)
- Only (re)start affected (loggered stdout)

journalalert filter

```yaml
 - unit: mysql
   message: '*[[]Note[]]*'
 - unit: mysql
   message: ''
 - unit: mysql
   message: 'Version:*'
```

### roundcube


### su

journalalert filter

```yaml
 - identifier: su
   message: 'No passwd entry for user *'
```

### sshd

- Syslog is default
- Severity levels are silly
- [Issue (2016) exists](https://bugzilla.mindrot.org/show_bug.cgi?id=2585)

journalalert filter

```yaml
 - identifier: sshd
   message: 'PAM service(sshd) ignoring max retries*'
 - identifier: sshd
   message: 'fatal: Read from socket failed: Connection reset by peer *'
 - identifier: sshd
   message: 'fatal: Unable to negotiate a key exchange method *'
 - identifier: sshd
   message: 'fatal: no matching cipher found: *'
 - identifier: sshd
   message: 'error: Received disconnect from *'
 - identifier: sshd
   message: 'warning: can''t get client address: Connection reset by peer'
 - identifier: sshd
   message: 'fatal: Write failed: Broken pipe *'
 - identifier: sshd
   message: 'fatal: Write failed: Connection reset by peer *'
 - identifier: sshd
   message: 'fatal: no hostkey alg *'
```

## Working

- apache2 (more to come in 2.6)
- postgresql
- postfix
- systemd
