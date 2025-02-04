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

Permission Error Issue
----------------------

I don't known if it's an OS problem or a virtual machine problem, but
when trying to run a startx on the sheel, I get some
**permission denied** issues, even as wheel/super user. I was
googling _(AI'ing too)_ about this problem and I found the following
solution:

1. **Add your user to especific groups:**
   Your user (even if it's super) need to be seted on tty and video
   groups:

   ```shell
   doas adduser <YOUR_USERNAME> tty
   doas adduser <YOUR_USERNAME> video
   ```

   Then, you can see if the command was successfuly runned by using
   `grops <YOUR_USERNAME>` command. It will print the groups that
   your user in.

2. **Set SUID on Xorg:**
   The Xorg binary needs to have the SUID (Set User ID) bit set to
   run with elevated privileges:

   ```shell
   doas chmod u+s /usr/bin/Xorg
   ```

3. **Using seatd for Seat Management (Optional but recommended):**

   - Install seatd for manage user seat permissions with no root:

     ```shell
     doas apk add seatd
     ```

   - Add your user to seat group + start the service:

     ```shell
     doas adduser <YOUR_USERNAME> seat
     doas rc-service seatd start
     doas rc-update add seatd
     ```
4. **Reboot:**
   Now, you can logout/reboot and when back in, your user will have
   the permissions :^D
