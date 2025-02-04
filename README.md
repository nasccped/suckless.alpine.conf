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

Dependencies
------------

Before getting all dependencies, you'll need to run the following
command:

```shell
doas setup-xorg-base
```

I don't know exactly what this do, but is required in dwm
installation _(according to wiki.alpinelinux.org)_.

---

Now, we'll get all dependencies to turn our machine into an usable
xorg desktop. All dependencies can be found at 'SAC-reqs.txt'
_(Suckless Alpine Conf - requirements)_.
