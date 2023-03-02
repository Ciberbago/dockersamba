This is my first docker image. Following the tutorial of MadhuKraft on youtube.
Credits to him. I just followed the tutorial and made it available to the public as a docker hub image.

How to use:

First, you need a smb.conf file, create it wherever you prefer, just make sure to put the route in the docker-compose file.
Change variables to your preference and if you only want one user, you can delete the second part. Make sure to match the "name1" or "name2" to the one on docker compose.

Example:

```
[name1]
path=/mnt/name1_share
public=no
valid users=name1
write list=name1
read only=no

[name2]
path=/mnt/name2_share
public=no
valid users=name2
write list=name2
read only=no
```

docker:

```
docker run -d -p 445:445 -p 139:139 -v /route/to/config/smb.conf:/etc/samba/smb.conf -v /route/to/share1:/mnt/name1_share -e user_count=1 -e user1=name1 -e password1=1234 --restart unless-stopped
```

docker-compose:

```
services:
  samba:
    container_name: samba
    image: ciberbago/samba
    ports:
      - 445:445
      - 139:139
    volumes:
      - /route/to/config/file/smb.conf:/etc/samba/smb.conf
      - /route/to/your/share:/mnt/name1_share
      - /route/to/your/share2:/mnt/name2_share #optional
    environment:
      # Change "user_count" to match the desired number of users. If you want more than 2 
      # make sure to add "user3=name3" and "password3=pass"
      - user_count=2 
      - user1=name1
      - password1=1234
      - user2=name2 #optional
      - password2=1234 #optional
    restart: unless-stopped
```