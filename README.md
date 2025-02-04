Suckless Alpine Conf
====================

Welcome to my personal **Suckless bundle** configurations for
**Alpine Linux**

First things first
------------------

If your Alpine PC is completely default, make sure to turn on the
_'community'_ mirror and the _'rolling release'_ mode: (You can use
nano or vi as file editor)

```shell
# open the mirror urls file:
doas vi /etc/apk/repositories
```

And then, uncomment the community mirror by removing the `#` at the
beginning of the line + change the version tag to `edge`, literally.
Basically this:

```txt
#/media/cdrom/apks
http://alpinelinux.SOME.MIRROR.URL/VERSION_EXAMPLE/main
#http://alpinelinux.SOME.MIRROR.URL/VERSION_EXAMPLE/community
```

Will turn into this:

```txt
#/media/cdrom/apks
http://alpinelinux.SOME.MIRROR.URL/edge/main
http://alpinelinux.SOME.MIRROR.URL/edge/community
```

Finally, you can use the following command to update your package
manager:

```shell
doas apk update
doas apk upgrade --available
```
