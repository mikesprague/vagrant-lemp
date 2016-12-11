# Vagrant LEMP

Vagrant box for PHP development with Linux, Nginx, MariaDB, PHP 7, Composer, PHPUnit, and phpMyAdmin

## Prerequisites

### Required

It is assumed you have Virtual Box and Vagrant installed. If not, then grab
the latest version of each at the links below:

* [Virtual Box and Virtual Box Extension Pack](https://www.virtualbox.org/wiki/Downloads)
* [Vagrant](https://www.vagrantup.com/downloads.html)

### Recommended

Once Vagrant is installed, or if it already is, it's highly recommended
that you install the following Vagrant plugin:

* [vagrant-hostupdater](https://github.com/cogitatio/vagrant-hostsupdater)

  ```bash
  vagrant plugin install vagrant-hostsupdater
  ```

---

## What's Included

* Ubuntu Server v16.04 LTS (Xenial Xerus) 64bit
* Nginx
* PHP 7
* Composer
* PHPUnit
* MariaDB
* phpMyAdmin

---

## Installation

The first time you clone the repo and bring the box up, it may take several
minutes. If it doesn't explicitly fail/quit, then it is still working.

```bash
git clone https://github.com/mikesprague/vagrant-lemp.git
cd vagrant-lemp && vagrant up
```

Once the Vagrant box finishes and is ready, you can verify PHP is working at
[http://lemp.dev/test.php](http://lemp.dev/test.php).

* phpMyAdmin: [http://lemp.dev/phpmyadmin](http://lemp.dev/phpmyadmin)
  * user: phpmyadmin
  * pass: root

---

## Sequel Pro MySQL Workbench Access

To login to the server using Sequel Pro or Workbench just use 
* host lemp.dev, 
* user ubuntu, 
* NO password
* default port (22)

For the MySQL details use 
* host 127.0.0.1, 
* user phpmyadmin and 
* password root.

NB Sequel Pro users:use the SSH tab

---

## License

The MIT License (MIT)

Copyright (c) 2016 Mike Sprague

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
