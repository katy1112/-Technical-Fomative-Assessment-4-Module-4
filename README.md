# TFA 4 — Who's Allowed In? Sessions and Authentication

A CodeIgniter 4-based POS web application with session-based authentication and protected customer and user management pages.

This project builds on the previous TFA 3 application by adding login authentication, password hashing, session management, route protection using a CodeIgniter Filter, and logout functionality.

## Project Overview

The system requires users to log in before they can manage customer and user accounts.

The authentication workflow is:

```text
Login Form
    ↓
Username and Password
    ↓
Find User in Database
    ↓
password_verify()
    ↓
Create Session
    ↓
Access Protected Pages


When a user logs out, the session is destroyed and protected pages can no longer be accessed until the user logs in again.

Technologies Used
PHP
CodeIgniter 4
MySQL
MySQLi
HTML5
CSS3
XAMPP
Apache
PHP Sessions
Main Features
Authentication

The application provides:

Login form
Username verification
Hashed password storage
Password verification using password_verify()
Session creation after successful login
Session-based access control
Logout functionality
Session destruction during logout
Protected Pages

The following Customer and User pages require authentication:

Customer Accounts
View customers
Add customer
Create customer
Edit customer
Update customer
User Accounts
View users
Add user
Create user
Edit user
Update user

Users who are not logged in are automatically redirected to the login page.

Authentication

Passwords are not stored as plain text.

Passwords are hashed using:

password_hash()

During login, the entered password is checked against the stored hash using:

password_verify()

The login session stores:

session()->set([
    'user_id' => $user['id'],
    'username' => $user['username'],
    'isLoggedIn' => true
]);

The isLoggedIn session value is used by the authentication filter to determine whether the user is allowed to access protected pages.

Authentication Filter

The project uses a custom CodeIgniter Filter:

app/Filters/AuthFilter.php

The filter checks whether the user has an active login session.

If the user is not logged in:

isLoggedIn = false
        ↓
Redirect to /login

If the user is logged in:

isLoggedIn = true
        ↓
Allow access

The filter is registered in:

app/Config/Filters.php

using the alias:

'auth' => \App\Filters\AuthFilter::class,
Protected Routes

The authentication filter is applied to the following routes:

/customers
/customers/new
/customers/create
/customers/edit/{id}
/customers/update/{id}

/users
/users/new
/users/create
/users/edit/{id}
/users/update/{id}

The following routes remain accessible without authentication:

/
/login
/logout
/about
Logout

The logout action is handled by:

Auth::logout()

The session is destroyed using:

session()->destroy();

After logout, the user is redirected to:

/login

Trying to access a protected page after logging out will redirect the user back to the login page.

Demo Account

A demo account is included for testing:

Username: katy
Password: katy123

The password is stored in the database as a hash and is not stored as plain text.

The demo credentials are for academic/testing purposes only.

Database

Database name:

pos_system_tfa3

The project includes the database export:

pos_system_tfa3.sql
Tables

The database contains:

customers
users
customers table
id
full_name
email
phone
created_at
users table
id
username
full_name
email
created_at
avatar
password

The password column stores the hashed password.

The avatar column stores only the filename of the uploaded avatar.

Project Structure
tfa3/
│
├── app/
│   ├── Config/
│   │   ├── Filters.php
│   │   └── Routes.php
│   │
│   ├── Controllers/
│   │   ├── Auth.php
│   │   ├── Customers.php
│   │   ├── Users.php
│   │   └── Pages.php
│   │
│   ├── Filters/
│   │   └── AuthFilter.php
│   │
│   ├── Models/
│   │   ├── CustomerModel.php
│   │   └── UserModel.php
│   │
│   └── Views/
│       ├── auth/
│       │   └── login.php
│       │
│       ├── customers/
│       │   ├── index.php
│       │   ├── new.php
│       │   └── edit.php
│       │
│       ├── users/
│       │   ├── index.php
│       │   ├── new.php
│       │   └── edit.php
│       │
│       └── pages/
│
├── public/
│   ├── css/
│   │   └── style.css
│   │
│   └── uploads/
│       └── avatars/
│
├── writable/
│
├── .env
├── pos_system_tfa3.sql
└── README.md
Routes
Authentication Routes
Method	Route	Purpose
GET	/login	Display login page
POST	/login	Process login
GET	/logout	Destroy session and logout
Customer Routes
Method	Route	Purpose
GET	/customers	Display customer list
GET	/customers/new	Display add customer form
POST	/customers/create	Create customer
GET	/customers/edit/{id}	Display edit customer form
POST	/customers/update/{id}	Update customer
User Routes
Method	Route	Purpose
GET	/users	Display user list
GET	/users/new	Display add user form
POST	/users/create	Create user
GET	/users/edit/{id}	Display edit user form
POST	/users/update/{id}	Update user
Installation and Setup
Requirements

Make sure the following are installed:

XAMPP
PHP 8.2 or higher
Apache
MySQL
CodeIgniter 4
MySQLi PHP extension
GD PHP extension
Step 1 — Clone or Copy the Project

Place the project inside the XAMPP htdocs directory:

C:\xampp\htdocs\tfa3
Step 2 — Start XAMPP

Start:

Apache
MySQL
Step 3 — Create the Database

Open:

http://localhost/phpmyadmin/

Create a database named:

pos_system_tfa3

Import:

pos_system_tfa3.sql

into the database.

Step 4 — Configure .env

Configure the database connection:

database.default.hostname = localhost
database.default.database = pos_system_tfa3
database.default.username = root
database.default.password =
database.default.DBDriver = MySQLi
database.default.port = 3306

app.baseURL = 'http://localhost/tfa3/'
Step 5 — Run the Application

Open:

http://localhost/tfa3/

To access the login page directly:

http://localhost/tfa3/login
Testing

The following authentication functions were tested.

Login
 Login page displays correctly
 Username is checked against the database
 Password is verified using password_verify()
 Incorrect credentials are rejected
 Successful login creates a session
 Successful login redirects to the Customers page
Session
 Login session stores the user's ID
 Login session stores the username
 isLoggedIn tracks authentication state
 Protected pages check authentication through the filter
Access Control
 Logged-out users cannot access /customers
 Logged-out users cannot access /customers/new
 Logged-out users cannot access customer edit pages
 Logged-out users cannot access /users
 Logged-out users cannot access /users/new
 Logged-out users cannot access user edit pages
 Logged-in users can access protected pages normally
Logout
 Logout destroys the session
 Logout redirects to /login
 Protected pages cannot be accessed after logout
Security Practices

The project applies the following security practices:

Passwords are hashed using password_hash()
Passwords are verified using password_verify()
Authentication state is stored in a server-side session
Protected routes use a CodeIgniter authentication filter
Logged-out users are redirected to the login page
CSRF protection is included in forms using csrf_field()
User output is escaped using esc()
Database models use allowedFields
Avatar uploads are validated by file type and size
Uploaded avatars use generated filenames
Academic Project

This project was developed for:

IT0049 — Web System Technologies

Technical Formative Assessment 4

Who's Allowed In? Sessions and Authentication

Program Outcome:

Design, implement and evaluate computer-based systems or applications to meet desired needs and requirements.

Course Learning Outcome:

Apply advanced web development principles in creating, debugging, validating, and securing database-backed web applications.

Author

Katrina Mangat

FEU Diliman
Web and Mobile Design
