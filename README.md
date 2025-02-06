![wallpaper](./repository-img/wallpaper-nginx-php-laravel.jpg)
# Overview
An environment with PHP-FPM and Nginx, prepared to serve local Laravel apps.
This will make your development process more realistic and closer to a production environment.

# Usage
### Download the repository and build the image
1. Download this repository and open a terminal in its directory;
2. We need to build the image: 
```bash
$ sudo docker build -t my-username/my-image --build-arg user=<your-system-username> --build-arg node=22 ./environment-files/ 
```
> * After `-t`, you must specify the name you want for the image;
> * In `<your-system-username>`, you must specify your operational system's username;
> * In `node`, you must specify the version of Node that you want.
> 
> In my case, the command is as follows:
> ```bash
> $ sudo docker build -t matheusbrdev/php-fpm:8.3 --build-arg user=matheusbrdev --build-arg node=22 ./environment-files/ 
> ```

### Creating a project 
1. Open your terminal in the path where you want to create the project;
2. Create a Laravel project using the image you built:
```bash
$ sudo docker container run --rm -v "$(pwd)/":/var/www/html matheusbrdev/php-fpm:8.3 composer create-project "laravel
/laravel:^11" example-app
```
> This command will run a temporary container to create a Laravel project called `example-app`

3. Copy the `docker-compose.yml` file and the `environment-files` folder,
then paste them into the folder you created;
4. Change the image name in `docker-compose.yml` to the name you chose in the build step;
5. Add in your `.env` this variable:
```env
APP_PORT=
```
> `APP_PORT` must be the port you want to use to serve the app on your machine;
6. Now, run the following command:
```bash
$ sudo docker compose up -d
```
7. Great! Your app is now being served!

### Usage on an existing project
1. Copy the `docker-compose.yml` file and the `environment-files` folder,
then paste them into your Laravel project directory;
2. In your `.env` file, add this variable:
```env
APP_PORT=
```
> `APP_PORT` must be the port you want to use to serve the app on your machine;
3. Change the image name in `docker-compose.yml` to the name you chose in the build step;
4. Now, run the following command:
```bash
$ sudo docker compose up --build -d
```
5. Install the framework(only if the vendor folder doesn't exist):
```bash
$ sudo docker compose exec app composer install
```
6. Generate the key for your project (only if the key doesn't exist in the `.env` file):
```bash
$ sudo docker compose exec app php artisan key:generate
```
7. Great! Your app is now being served!