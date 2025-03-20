# Laravel WebSocket & Vue.js One-to-One Chat App

A real-time one-to-one chat application built using Laravel WebSockets and Vue.js.

## Features

- **Real-time messaging** using Laravel WebSockets
- **Vue.js frontend** with dynamic chat updates
- **User authentication**
- **Private messaging** (one-to-one chat)
- **Total Unread Messages Counter**

## Technologies Used

- **Backend**: Laravel, Laravel WebSockets, Laravel Echo
- **Frontend**: Vue.js, Axios, Bootstrap
- **Database**: MySQL
- **WebSocket Server**: pusher-js

## Installation
Do the following after you've cloned this project:
```bash
# navigate to the project folder
cd laravel-websockets

# make an environment file (.env)
cp .env.example .env

# install composer dependencies
composer update

# install npm dependencies
npm install

# for your application, generate a key
php artisan key:generate

# To generate the schema, run the migration files.
php artisan migrate

# Start with a few people and messages in your database.
php artisan db:seed

# Create a free app at https://pusher.com. After that, paste the keys into your.env file.
PUSHER_APP_ID=your_pusher_app_id
PUSHER_APP_KEY=your_pusher_app_key
PUSHER_APP_SECRET=your_pusher_app_secret
PUSHER_APP_CLUSTER=your_pusher_cluster

# Update BROADCAST_DRIVER to pusher in your.env
BROADCAST_DRIVER=pusher

# Run webpack and keep an eye out for changes.
npm run watch

Take any user email from the seeded database users and login by typing the password in the password field.
```
    
