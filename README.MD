29.-30. OAuth
Is a standard developed back in 2007
API access without username\password
OAuth2 was reworking, developed in 2012
php artisan make:auth
composer require laravel/passport=~4.0
php artisan migrate
php artisan passport:install
edit app/User.php

---

28. Basic Authentication
php artisan make:middleware BasicAuth
edit BasicAuth.php; Kernel.php
php artisan serve
test with get request - error 401 UNAUTHORIZED
fill BasicAuth data of request via Insomnia - USERNAME: api-poll.users.email; PASSWORD: secret;
test with get request - 200 OK

----

27.Token authentication
Auth.png
Token Authentication - we pass a token either as a header or in the URL parameters
php artisan make:middleware TokenAuth
we need to check if the request has a certain header - edit TokenAuth.php; edit Kernel.php
php artisan serve
test with get request - error 401 UNAUTHORIZED
fill headers data of request via Insomnia - NAME: X-API-TOKEN; VALUE: test-value;
test with get request - 200 OK

---

26.2 
attach a single string to the header - edit Kernel.php; edit HttpHeadeers.php
test via GET request - RESPONSE will include Header, specified in Kernel.php as httpHeaders:
---

26.1 Middleware to modify the response (the most common use of middleware on the response side)
add custom HTTP header
php artisan make:middleware HttpHeaders
to call middleware after the response, we need replace content of handle method  - edit HttpHeaders.php
edit Kernel.php
test via GET request - RESPONSE will include Header with the name... :)

---

 25.2
to log both request and response - edit Logging.php
Insomnia - make a request
find log in the end of storage/logs/laravel.log with REQUEST local.DEBUG: GET and RESPONSE local.DEBUG: 200 

25.1 Creating a logging middleware layer
php artisan make:middleware Logging
To build in login feature - edit app/http/Middleware/Logging.php
Add the middleware layer to application - edit app/Http/Kernel.php 
Insomnia - make a couples of requests
find logs in the end of storage/logs/laravel.log with REQUEST local.DEBUG: GET

---

24 Middleware Theory
App - the center of an onion /center.png
Each layer can modify the request and the response
middleware ideas
    Logging
    Rate Limit
    Authentication
    Change date format
Middleware is movable between frameworks

---

23 file upload
api.php Route::post('files/create', 'FilesController@create');
FilesController.php public function create()...
Insomnia - Multipart post request http://127.0.0.1:8000/api/files/create
    name: photo; select file <2Mb; SEND

---

22 download endpoint
php artisan make:controller FilesController
routes/api.php - Route::get('files/get', 'FilesController@show');
add public function show() to app/http/Controllers/FilesController
copy somefile to storage/app/

---

21 Paginate the API

---

Managing subresources

Loading Subresources
Add relations

---

making  endpoint for Questions, using QuestionsController
    php artisan make:controller QuestionsController --resource --model=Question
api.php:
    Route::apiResource('questions', 'QuestionsCont roller');
QuestionController.php:
    

---

making resources for data transforming
php artisan make:resource Poll      //creates /app/Http/Resources/Poll.php

---

adding a record
    create post route in routes/api.php
    create public function store() in PollsController.php
    making POST request with Insomnia program to http://127.0.0.1:8000/api/polls: 500Error...
    edit Poll.php: add protected $fillable = ['title]
    making POST request with Insomnia program to http://127.0.0.1:8000/api/polls: 201Created :)
    making POST request with Postman: POST to http://127.0.0.1:8000/api/polls with KEY=title; VALUE=testing2: status 201Created :)
  
    
---

to display a single record 
    edit routes/api.php
    create public function show() in PollsController.php


---

php artisan migrate:fresh --seed

php artisan make:model Poll

php artisan make:model Question

php artisan make:model Answer

php artisan db:seed

getting the records for the endpoint: simple API endpoint to get all the polls, to begin with make controller:
    php artisan make:controller PollsController
    edit routes/api.php
    edit http/Controllers/PollsController.php 

php artisan serve

test working GET request http://127.0.0.1:8000/api/polls (for example via Postman ) :)