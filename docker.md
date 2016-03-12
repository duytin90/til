##Docker til's

####Docker Machine on OSX - 03/12/2016
To run docker on a Mac, follow the instructions here
[Docker Machine OSX](https://docs.docker.com/engine/installation/mac/)

Use `docker-machine help` for reference.

####MySQL container - 03/12/2016
Since docker on OSX runs inside a Linux VM, it creates strange permission error when running mysql container.
I ran into permission issue on running the mysql container, quick googling around got me [here](https://github.com/docker-library/mysql/issues/99). 
Apparently, this is a common error. The fix provided includes creating a new Dockerfile and building a new docker image, a modified mysql 
image inheritting from the standard mysql image. This modified image gives the ability to run a different shell script to start the container.
The new shell script gives the mysql user the right permission to write to OSX volume. The new dockerfile is 

    FROM mysql
    MAINTAINER Tin Nguyen "tindn14@gmail.com"

    ENV MYSQL_ROOT_PASSWORD root

    # this need to stay the same for script to work
    ENV MYSQL_USER mysql

    COPY ./localdb-run.sh /
    RUN chmod 755 /localdb-run.sh

    ENTRYPOINT ["/localdb-run.sh"] 

The new entrypoint shell script file is 

    #!/bin/bash
    set -e

    # Script to workaround docker-machine/boot2docker OSX host volume issues: https://github.com/docker-library/mysql/issues/99
    
    echo '* Working around permission errors locally by making sure that "mysql" uses the same uid and gid as the host volume'
    TARGET_UID=$(stat -c "%u" /var/lib/mysql)
    echo '-- Setting mysql user to use uid '$TARGET_UID
    usermod -o -u $TARGET_UID mysql || true
    TARGET_GID=$(stat -c "%g" /var/lib/mysql)
    echo '-- Setting mysql group to use gid '$TARGET_GID
    groupmod -o -g $TARGET_GID mysql || true
    echo
    echo '* Starting MySQL'
    chown -R mysql:root /var/run/mysqld/
    /entrypoint.sh mysqld --user=mysql --console
And just like that, the mysql container auto-magically works. Happy coding!
