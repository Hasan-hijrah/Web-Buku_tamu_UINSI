# CI4 Authentication Starter

## Description

This is a starter project for CodeIgniter 4. It includes a basic authentication system with login, registration, email verification, and password reset.

## Installation

1. Clone the repository
2. Run `composer install`
3. Copy `.env.example` to `.env` and fill in the database details, email settings, and base URL
4. Run `php spark migrate` to create the database tables
5. Run `php spark serve` to start the development server

## Server Requirements

PHP version 7.4 or higher is required, with the following extensions installed:

- [intl](http://php.net/manual/en/intl.requirements.php)
- [mbstring](http://php.net/manual/en/mbstring.installation.php)

Additionally, make sure that the following extensions are enabled in your PHP:

- json (enabled by default - don't turn it off)
- [mysqlnd](http://php.net/manual/en/mysqlnd.install.php) if you plan to use MySQL
- [libcurl](http://php.net/manual/en/curl.requirements.php) if you plan to use the HTTP\CURLRequest library
