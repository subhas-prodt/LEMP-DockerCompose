# LEMP-DockerCompose

### Key Points

-> Replace the servername in nginx/site-available/default file with the docker ip. 
To find docker ip use the command ifconfig and look for the line inet addr: in the row docker* .

Default file:

server {
    listen  80;

    # this path MUST be exactly as docker-compose.fpm.volumes,
    # even if it doesn't exists in this dock.
    root /usr/share/nginx/html;
    index index.php index.html index.html;
    
    server_name 172.17.0.1;

    location / {
        try_files $uri /index.php$is_args$args;
    }

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass phpfpm:9000; 
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}

-> Create a php file under public folder

-> By default the volume tag in docker-compose creates a directory not a file, therefore create all the required files prior to running the docker-compose up -d.
