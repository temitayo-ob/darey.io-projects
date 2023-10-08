project LEMP has been executed

### Errors encountered 
encountered an `error 502` ba d gateway when testing PHP with nginx server, URL test came back as `error 502` when `http://ip/info.php` file was inputed.

### Solution

problem was in the php configuration file where the php8.0-fpm sock was specified and the php being run on the sytem was php7.4. i changed to php7.4-fpm, and the error was corrected'


