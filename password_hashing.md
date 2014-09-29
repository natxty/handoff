# Password Hashing #

Older members (2010 and earlier) tend to have _unique salts_ that are used for password encryption. These are stored in the `users` table with their password hash.

Later in 2010, the developers abandoned this system and used _one single salt_ for all users. This is located in the `Config/core.php` file. This is how users' passwords are currently processed/handled.

The current login() function checks for, and handles, both these scenarios.