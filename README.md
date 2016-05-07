### running the image

`docker run -dp 3389:3389 --restart=always swined/rdp`

### adding users manually

run the image, find it's id with `docker ps`, then run `docker exec -it <containerId> adduser <username> --ingroup sudo` and follow the prompts. the user will have admin access, check out `man adduser` for more details on configuring user's permissions.

### building custom images with predefined users

run `echo -n <password> | openssl passwd -crypt -stdin` and copy the hashed password, that is returned. create a folder, and put there a 'Dockerfile' with the following content:

```
FROM swined/rdp
RUN useradd -mp <hashedPassword> -s /bin/bash -G sudo <username>
```

run `docker build -t <imageName> .` and then run the image as usual, replacing 'swined/rdp' with your custom image name

### building custom images with preinstalled software

create a Dockerfile as described earlier, and put a line with `RUN apt-get update && apt-get install -y --force-yes <listOfUbuntuPackageNames> && apt-get clean` instead of 'RUN useradd ...', or combine both 'useradd' and 'apt-get' if you need both custom users and custom software
