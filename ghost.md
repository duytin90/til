##Ghost til's

####Learned to install Ghost - 02/23/2016
[Ghost supported Node versions](http://support.ghost.org/supported-node-versions/)

#####On Windows
[Download latest Node](https://nodejs.org/en/download/)

[Follow tutorial to install node](http://support.ghost.org/installing-ghost-windows/) - *very straightforward*

By default, Ghost runs in Development mode. In order to run in production mode, have to set NODE_ENV=production. In cmd, run

    set NODE_ENV=product
    npm start

In powershell, run

    $env:NODE_ENV="production"
    npm start

#####With Docker
Using the [official ghost image](https://hub.docker.com/_/ghost/) for Docker, run the following command to create docker image

        docker run --name docker_container_name -e NODE_ENV=production -v path_to_volume_on_host:/var/lib/ghost -p 6000:2368 -d ghost
    
After running this, most likely,the container will exit with error code 235, permission error. Looking around on comments on the ghost image page, the issue seems to be with Path in production mode. Navigate to `path_to_volume_on_host`, edit `config.js` file, copy the `paths` section from `development` section to `production` section. Restart docker container by running

        docker start container_name

and voila, it runs. Add nginx entry to redirect traffic to the right port.
