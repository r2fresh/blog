# Mac에서 SSH로 시작하는 git 사용 시 에러 해결

회사에서 내부에서 gitblit를 사용 시, git clone의 주소 앞이 https, git이 아닌 ssh일 경우 리눅스 계열에서는 문제없이 git이 작동하지만 Mac에서는 작동을 하지 않고 아래와 같은 에러가 발생한다.

``` bash
r2:(master) $ git push -u gerrit master
Unable to negotiate with ***.***.***.*** port 29418: no matching cipher found. Their offer: aes128-cbc,3des-cbc,blowfish-cbc,aes192-cbc,aes256-cbc
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

이 경우에는 ssh_config의 설정에서 주석으로 처리되어 있는 부분을 풀고 저장하면 문제없이 사용이 가능하다.

``` bash
$ > sudo vim /etc/ssh/ssh_config

#       $OpenBSD: ssh_config,v 1.33 2017/05/07 23:12:57 djm Exp $
  
# This is the ssh client system-wide configuration file.  See
# ssh_config(5) for more information.  This file provides defaults for
# users, and the values can be changed in per-user configuration files
# or on the command line.

# Configuration data is parsed as follows:
#  1. command line options
#  2. user-specific file
#  3. system-wide file
# Any configuration value is only changed the first time it is set.
# Thus, host-specific definitions should be at the beginning of the
# configuration file, and defaults at the end.

# Site-wide defaults for some commonly used options.  For a comprehensive
# list of available options, their meanings and defaults, please see the
# ssh_config(5) man page.

# Host *
#   ForwardAgent no
#   ForwardX11 no
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   IdentityFile ~/.ssh/id_ecdsa
#   IdentityFile ~/.ssh/id_ed25519
#   Port 22
#   Protocol 2
    Ciphers aes128-ctr,aes192-ctr,aes256-ctr,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
#   RekeyLimit 1G 1h

Host *
        SendEnv LANG LC_*
~
```

## 참고

* http://itsooda.com/2018/04/15/env/2018-04-15-fallingg-macos-ssh-error/

