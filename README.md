## Installation for laravel project with vite

    # creating larvel project lastest in using webpack configuration
    composer create-project laravel/laravel basicwebpack "9.1"
    cd basicwebpack
    # Installation vue3 and vue-router
    npm install vue@next vue-loader-next vue-router@next    

# Configuring Vue in the project
    # create a resources/js/App.vue basic vue template file

    # create div with id 'vue-app' to map vue application in the desired blade file (welcome.blade.php) and add following to the header of each page

        <link href="{{ asset('css/app.css') }}" rel="stylesheet">
        <script type="module" src="{{asset('js/app.js')}}"></script>

    # configure vue instance in app.js,  mount it and call it in the welcome blade file 

    import { createApp } from 'vue';
    import App from './App.vue';

    const app = createApp(App).mount('#vue-app');

    # configure webpack.mix.js - add vue() to mix.js    
   
    # js compilation in terminal
    npm install
    npm run dev 

    php artisan serve


# changing laravel-mix to vite  ( refer https://github.com/laravel/laravel/pull/5895/files )
    ensure you are using Laravel Framework 9.19.0 To check use
        php artisan --version
            or
        composer require laravel/framework:^9.19.0

    # Installation of Vite in the project

    npm install --save-dev vite laravel-vite-plugin
    npm install --save-dev @vitejs/plugin-vue


    # create vite.config.js as given below

    import { defineConfig } from 'vite';
    import laravel from 'laravel-vite-plugin';
    import vue from '@vitejs/plugin-vue';
    //import react from '@vitejs/plugin-react';
  

    export default defineConfig({
        plugins: [
            laravel([
                'resources/css/app.css',
                'resources/js/app.js',
            ]),
            // react(),
            vue({
            //     template: {
                            // transformAssetUrls: {
                                // The Vue plugin will re-write asset URLs, when referenced
                                // in Single File Components, to point to the Laravel web
                                // server. Setting this to `null` allows the Laravel plugin
                                // to instead re-write asset URLs to point to the Vite
                                // server instead.
                                //   
                            // base: null,
                                // The Vue plugin will parse absolute URLs and treat them
                                // as absolute paths to files on disk. Setting this to
                                // `false` will leave absolute URLs un-touched so they can
                                // reference assets in the public directly as expected.
                            // includeAbsolute: false,
            //         },
            //     },
            }),
        ],
            resolve: {
                alias: {
                '@': '/resources/js'
                }
            }
    });

# Update Aliases
If you are migrating aliases from your webpack.mix.js file to your vite.config.js file, you should ensure that the paths start with /. For example, resources/js would become /resources/js:

    export default defineConfig({
        plugins: [
            laravel([
                'resources/css/app.css',
                'resources/js/app.js',
            ]),
        ],
        resolve: {
            alias: {
            '@': '/resources/js'
        }            
});

# update package.json as below

  "scripts": {
    -     "dev": "npm run development",
    -     "development": "mix",
    -     "watch": "mix watch",
    -     "watch-poll": "mix watch -- --watch-options-poll=1000",
    -     "hot": "mix watch --hot",
    -     "prod": "npm run production",
    -     "production": "mix --production"
    +     "dev": "vite",
    +     "build": "vite build"
  }

# Update environment variables
You will need to update the environment variables that are explicitly exposed in your .env files and in hosting environments such as Forge to use the VITE_ prefix instead of MIX_:

- MIX_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
- MIX_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"
+ VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
+ VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"

# Vite compatiable imports
    #in resources/js/app.js
        - require('./bootstrap');
        + import './bootstrap';

    # in resources/js/bootstrap.js
       - window._ = require('lodash');
        +import _ from 'lodash';
        + window._ = _;

        - window.axios = require('axios');
        + import axios from 'axios';
        + window.axios = axios;

        window.axios.defaults.headers.common['X-Requested-With'] = 'XMLHttpRequest';

        @@ -18,7 +20,8 @@ window.axios.defaults.headers.common['X-Requested-With'] = 'XMLHttpRequest';

        // window.Pusher = require('pusher-js');
        // import Pusher from 'pusher-js';
        // window.Pusher = Pusher;

        
        // window.Echo = new Echo({
        // broadcaster: 'pusher',

# do following changes to welcome blade file or include it in every header of each module

    <!--   <link href="{{ asset('css/app.css') }}" rel="stylesheet">
    <script type="module" src="{{asset('js/app.js')}}"></script> -->
    @vite(['resources/css/app.css', 'resources/js/app.js'])

# Remove Laravel Mix
    # The Laravel Mix package can now be uninstalled:
    npm remove laravel-mix

    #And you may remove your Mix configuration file:
    rm webpack.mix.js

# Run following commands
    npm run dev
    php artisan serve

# run development server in terminal
    php artisan serve







## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.

## Learning Laravel

Laravel has the most extensive and thorough [documentation](https://laravel.com/docs) and video tutorial library of all modern web application frameworks, making it a breeze to get started with the framework.

If you don't feel like reading, [Laracasts](https://laracasts.com) can help. Laracasts contains over 2000 video tutorials on a range of topics including Laravel, modern PHP, unit testing, and JavaScript. Boost your skills by digging into our comprehensive video library.

## Laravel Sponsors

We would like to extend our thanks to the following sponsors for funding Laravel development. If you are interested in becoming a sponsor, please visit the Laravel [Patreon page](https://patreon.com/taylorotwell).

### Premium Partners

- **[Vehikl](https://vehikl.com/)**
- **[Tighten Co.](https://tighten.co)**
- **[Kirschbaum Development Group](https://kirschbaumdevelopment.com)**
- **[64 Robots](https://64robots.com)**
- **[Cubet Techno Labs](https://cubettech.com)**
- **[Cyber-Duck](https://cyber-duck.co.uk)**
- **[Many](https://www.many.co.uk)**
- **[Webdock, Fast VPS Hosting](https://www.webdock.io/en)**
- **[DevSquad](https://devsquad.com)**
- **[Curotec](https://www.curotec.com/services/technologies/laravel/)**
- **[OP.GG](https://op.gg)**
- **[WebReinvent](https://webreinvent.com/?utm_source=laravel&utm_medium=github&utm_campaign=patreon-sponsors)**
- **[Lendio](https://lendio.com)**

## Contributing

Thank you for considering contributing to the Laravel framework! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
