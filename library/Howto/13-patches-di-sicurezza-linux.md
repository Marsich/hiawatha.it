# Patches di sicurezza Linux

### AppArmor

Se decidessimo di usare [AppArmor](http://wiki.apparmor.net/index.php/Main_Page) potremmo cominciare con questa configurazione:

```
#include
/usr/sbin/hiawatha {
    #include

    capability dac_override,
    capability net_bind_service,
    capability sys_chroot,
    capability setgid,
    capability setuid,

    network inet tcp,

    /usr/sbin/hiawatha mr,
    /usr/sbin/cgi-wrapper mr,
    /etc/passwd r,
    /etc/group r,
    /etc/hiawatha/** r,
    /etc/nsswitch.conf r,
    /var/log/hiawatha/* rw,
    /var/run/hiawatha.pid w,
    /var/lib/hiawatha/* rw,
    /var/www/** rw,
    /home/*/public_html/** r,
}
```
* * *

### grsecurity

Se volessimo usare il sistema RBAC di [grsecurity](https://grsecurity.net/) con Hiawatha, potremmo conciare col seguente file di configurazione:

```
subject /usr/sbin/hiawatha o
    /                       r
    /etc/hiawatha           r
    /var/run/hiawatha.pid   cw
    /var/log/hiawatha       rwca
    /var/lib/hiawatha       rwmcd
    /var/www                rxwmcad
    /home
    /home/*/public_html     rxwmcad
    /usr/sbin/cgi-wrapper   x
    /lib                    rx
    /usr/lib                rx
    /proc
    /proc/kcore             h
    /proc/sys               h
    /proc/*/fd              rw
    /dev
    /dev/random             r
    /dev/urandom            r
    /dev/null               rw
    /dev/pts                rw
    /dev/std*               rw

    +CAP_SETUID
    +CAP_SETGID
    +CAP_SYS_CHROOT
    +CAP_NET_BIND_SERVICE
```

Pagina originale: [https://www.hiawatha-webserver.org/howto/linux_security_patches](https://www.hiawatha-webserver.org/howto/linux_security_patches)