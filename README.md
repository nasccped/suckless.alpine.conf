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

Clone
-----

Clone the repository by using the following command (git already
installed as dependency):

```shell
git clone https://github.com/nasccped/suckless.alpine.conf
# removing .git folder
rm -rf suckless.alpine.conf/.git
```

Permission Error Issue
----------------------

I don't know if it's an OS problem or a virtual machine problem, but
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

Installing
----------

After getting all dependencies on date + solving permissions errors,
you can now install dwm + dmenu.

For dwm install:

```shell
# go to the dwm dir
cd dwm

# build binaries + install
doas make clean install

# go back a dir
cd ..
```

For dmenu install:

```shell
# go to the dmenu dir
cd dmenu

# build binaries + install
doas make clean install

# go back a dir
cd ..
```

TADAH! the dwm + dmenu program is installed on your machine!

Setting the environment
-----------------------

And now, All you need to do is:

```shell
# go to the home dir
cd

# if .xinitrc already exists, I recommend to delet it by using
# `rm .xinitrc`

# insert the exec line at .xinitrc
echo "exec dwm" > .xinitrc
```

Have fun!
---------

And now, all that I have to do is run the `startx` command to start
the Xorg server and have fun with your dwm setup!

Extra
-----

### Keyboard Mapping

You'll probably configure the keyboard mapping for using Xorg. The
keyboard mapping for tty shell and Xorg are differents, so, when
starting dwm with startx, you basically start a Xorg sessing with
a default mapping. To change it:

- Install keyboard mapping for Xorg:
  ```shell
  doas apk add setxkbmap xkeyboard-config
  ```

- Add the correct mapping to your .xinitrc:

  ```txt
  # I personally use brazilian mapping. You can search for your map
  # at wiki.archlinux.org
  setxkbmap -layout br -variant abnt2
  exec dwm
  ```

### Resolution

To set the dwm to a specific resolution, You'll need to:

1. Install the `xrandr` program:

   ```shell
   doas apk add xrandr
   ```

2. Change the `.xinitrc` script:

   ```txt
   xrandr --output Virtual-1 --mode 1920x1080 --rate 60
   exec dwm
   ```

   The example above, I'm seting the output called 'Virtual-1' to
   1920x1080 pixels resolution with 60 fps rate. The name of outputs,
   pixel reso and the rate can differ from PC to PC. If you're unsure
   on how to use, open a new Xorg server with 'startx', open the
   terminal on it and run the `xrandr` command. It will print every
   available outputs and modes for your computer. Chose the one that
   best suits your machine and just change the `--output`, `--mode`
   and `--rate` arguments!

### Default shell

If your Alpine linux is from scratch, it probably came with **ash**
or **sh** shell. Trust me, it isn't a problem, but, on major linux
web guides and video tutorials, everything is working on **bash**,
not **sh**, so, here's how you can switch your default shell for a
prefered one:

1. Install the shell + chsh command by shadow pack:

  ```shell
  # get chsh
  doas apk add shadow
  # I prefer bash, but this command works for zsh or fish too
  doas apk add bash
  ```

2. Change default shell using chsh command:

  ```shell
  chsh -s /bin/bash
  ```

3. Reboot your pc:

  ```shell
  doas reboot
  ```

4. Log in again and see the changes:

  ```shell
  echo $SHELL # should outputs '/bin/bash' / the shell of your choice
  # you can use the following command to
  # echo $0
  # it's expected to print '-bash' / the shell of your choice
  ```

---

But, if it doesn't work, you can try a more manual process:

1. Open the `/etc/passwd` file with a text editor:

  ```shell
  vi /etc/passwd
  ```

  You will see something like this:

  ```txt
  root:x:0:0:root:/root:/bin/<SOME_RANDOM_SHELL>
  bin:x:1:1:bin:/bin:/sbin/nologin
  daemon:x:2:2:daemon:/sbin:/sbin/nologin
  lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
  sync:x:5:0:sync:/sbin:/bin/sync
  shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
  halt:x:7:0:halt:/sbin:/sbin/halt
  mail:x:8:12:mail:/var/mail:/sbin/nologin
  news:x:9:13:news:/usr/lib/news:/sbin/nologin
  uucp:x:10:14:uucp:/var/spool/uucppublic:/sbin/nologin
  cron:x:16:16:cron:/var/spool/cron:/sbin/nologin
  ftp:x:21:21::/var/lib/ftp:/sbin/nologin
  sshd:x:22:22:sshd:/dev/null:/sbin/nologin
  games:x:35:35:games:/usr/games:/sbin/nologin
  ntp:x:123:123:NTP:/var/empty:/sbin/nologin
  guest:x:405:100:guest:/dev/null:/sbin/nologin
  nobody:x:65534:65534:nobody:/:/sbin/nologin
  klogd:x:100:101:klogd:/dev/null:/sbin/nologin
  <YOUR_USERNAME>:x:1000:1000:<YOUR_USERNAME>:/home/<YOUR_USERNAME>:/bin/<SOME_RANDOM_SHELL>
  messagebus:x:101:102:messagebus:/dev/null:/sbin/nologin
  ```

2. Change the `<SOME_RANDOM_SHELL>` + reboot:

  This step is self-explanatory. Just change the places that contains
  the `<SOME_RANDOM_SHELL>` as in example to the shell of your
  choice ('bash' in my case), and then, reboot your machine:

  - `/etc/passwd` after changes (for bash):

  ```txt
  root:x:0:0:root:/root:/bin/bash
  bin:x:1:1:bin:/bin:/sbin/nologin
  daemon:x:2:2:daemon:/sbin:/sbin/nologin
  lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
  sync:x:5:0:sync:/sbin:/bin/sync
  shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
  halt:x:7:0:halt:/sbin:/sbin/halt
  mail:x:8:12:mail:/var/mail:/sbin/nologin
  news:x:9:13:news:/usr/lib/news:/sbin/nologin
  uucp:x:10:14:uucp:/var/spool/uucppublic:/sbin/nologin
  cron:x:16:16:cron:/var/spool/cron:/sbin/nologin
  ftp:x:21:21::/var/lib/ftp:/sbin/nologin
  sshd:x:22:22:sshd:/dev/null:/sbin/nologin
  games:x:35:35:games:/usr/games:/sbin/nologin
  ntp:x:123:123:NTP:/var/empty:/sbin/nologin
  guest:x:405:100:guest:/dev/null:/sbin/nologin
  nobody:x:65534:65534:nobody:/:/sbin/nologin
  klogd:x:100:101:klogd:/dev/null:/sbin/nologin
  <YOUR_USERNAME>:x:1000:1000:<YOUR_USERNAME>:/home/<YOUR_USERNAME>:/bin/bash
  messagebus:x:101:102:messagebus:/dev/null:/sbin/nologin
  ```

  - now, reboot:

  ```shell
  doas reboot
  ```

I hope this works :^)
