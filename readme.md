# Easy WordPress With Docker

This assumes you have Docker already installed on your system.  If you don't have then go to https://www.docker.com/products/docker-desktop and follow the installation instructions.

The easiest method is to use the existing Docker image developed by WordPress.

1. Open your code editor of choice and create a file called ```docker-compose.yml```
2. Paste the code below and save



```
version: '3.1'

services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - wordpress:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - db:/var/lib/mysql

volumes:
  wordpress:
  db:
```


3. Open a terminal and change to the root of your project. Type the command ```docker-compose up -d```
4. Wait for docker to pull the necessary files and point your browser to http://localhost:8080

You should be presented with the install screen for WordPress.



Let's breakdown what the docker compose file so we know what's going on. The first think to notice is that it is a .yaml file. Yaml is a recursive acronym for "YAML Ain't Markup Language". The first thing to note with .yaml files is that **indentation is important**.




### The first level of the .yaml file

Let's look at the first level. The first line tells us the version of docker compose that is being used. This tells Docker how to interpret the file. For mor inoformation about versions got to https://docs.docker.com/compose/compose-file/

The next section is called ```services```.  This is where the containers are described. In this case there is a container called "wordpress" which contains the set up information to set a web server and install the WordPress files.

The next section is ```volumes``` . Volumes tell Docker where to store the data and files. In this case a volume called "wordpress" contains the WordPress files. The volume called "db" is where the database is stored.


### First level

Everything is lined up to left

```
version: '3.1'
services:
volumes:
 
```



## Second Level

Indented by 4 spaces. Sometimes 2 spaces. Whatever you do, make is consistent.

```
version: '3.1'

services:

  wordpress:

  db:
      
volumes:
  wordpress:
  db:
```

The second level lists the services .i.e the containers that Docker will run. It will run a WordPress and database container.

The volumes are labeled as "wordpress" for the WordPress files and "db" for database files.



## Third Level

Second level of indents

### Wordpress
```
  wordpress:
    image: wordpress
    restart: always
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - wordpress:/var/www/html
```

**image**: This downloads the latest version of WordPress by default. You can download specific versions of WordPress by changing this setting. For example `wordpress:5.5` will give you an older version. A fulll listing of options can be found at: https://hub.docker.com/_/wordpress


**restart**: This tells Docker to restart the image if it crashes.


**ports**:  This maps port numbers. the number on the left tells us which port is visible to the outside. The number on the left is the internal prot number. Moist web servers run on port 80. `- 8080:80` is telling Docker to make container available on http://localhost:8000. Changing the config to `- 3333:80` would mke the container visible on http://localhost:3333


**environment**: This holds the variables WordPress requires to work. Ih this case the host is `db` (the same name as the container. It is telling the WordPress containee to connect to the database container with the user name `exampleuser` usint the password `examplepass` to a database called `exampledb`



**volumes**: This maps the volume called `wordpress` to the files in the `/var/www/html` directory on the `wordpress` container.

## Summary

As you can see, Docker makes it easy to set up

Each level of indentation belongs to the one above.


## Links

- WordPress Docker Image: https://hub.docker.com/_/wordpress
- Blank WordPress Starter Theme: https://underscores.me/
