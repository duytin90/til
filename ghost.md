##Ghost til's

####Learned to install Ghost - 02/23/2016
[Ghost supported Node versions](http://support.ghost.org/supported-node-versions/)

#####On Windows
[Download latest Node](https://nodejs.org/en/download/)

[Follow tutorial to install node](http://support.ghost.org/installing-ghost-windows/) - *very straightforward*

By default, Ghost runs in Development mode. In order to run in production mode, have to set NODE_EVN=production. In cmd, run

    set NODE_ENV=product
    npm start

In powershell, run

    $env:NODE_ENV="production"
    npm start
