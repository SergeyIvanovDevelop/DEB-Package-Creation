<p align="center">
  <a href="https://github.com/SergeyIvanovDevelop/DEB-Package-Creation">
    <img alt="Simple-OS" src="./resources/logo.png" width="280" height="240" />
  </a>
</p>
<h1 align="center">
  Creation deb-package
</h1>

## Creation deb-package &middot; [![GitHub license](https://img.shields.io/badge/license-CC%20BY--NC--SA%203.0-blue)](./LICENSE) [![C](https://img.shields.io/badge/language-C-yellow)](https://www.iso.org/standard/74528.html) [![Makefile](https://img.shields.io/badge/build-Makefile-yellowgreen)](https://www.gnu.org/software/make/manual/make.html) [![deb-package](https://img.shields.io/badge/package-deb-lightgrey)](https://www.debian.org/distrib/packages) [![LinkedIn](https://img.shields.io/badge/linkedin-Sergey%20Ivanov-blue)](https://www.linkedin.com/in/sergey-ivanov-33413823a/) [![Telegram](https://img.shields.io/badge/telegram-%40SergeyIvanov__dev-blueviolet)](https://t.me/SergeyIvanov_dev) ##

This repository contains the code of the simplest program in the `C` programming language and step by step instructions on how to package this software into a `deb` package.

## :computer: Getting Started  ##

**Stage 1**

1. Go to home directory and clone repository from github: `cd ~ && git clone https://SergeyIvanovDevelop@github.com/SergeyIvanovDevelop/DEB-Package-Creation && cd DEB-Package-Creation`

**Stage 2**<br>

2. Initial data: there is a directory `~/DEB-Package-Creation/Example_deb_create`, and it contains 2 more empty directories `Create_deb` and `Run_deb`.


**Stage 3**<br>

3. Install everything you need to build the `C` program and build the `deb` packages: `sudo apt-get install build-essential dh-make`

**Stage 4**<br>

4. Create file `hello_world_deb.c` and `Makefile` to it in directory `Create_deb`

```
hello_world_deb.c :
---------------------------
int main() {
    printf(“Hello, World\n”);
    return 0;
}
---------------------------	
```

```
Makefile :
---------------------------
all:
	gcc hello_world_deb.c -o hello_world_deb

clean:
	rm hello_world_deb || true
---------------------------
```

**Stage 5**<br>

5. We build the executable file (from the `Create_deb` directory): `make -f Makefile`<br>

The `hello_world_deb` file should appear in the `Create_deb` directory

**Stage 6**<br>

6. From the `Create_deb` directory, execute the command to create the `Create_deb/debian` directory (a service directory for building a `deb` package): `dh_make -p mydebpackagename_0.0.1 --createorig`

, где `mydebpackagename_0.0.1` имя и версия `deb`-пакета соответственно

In the terminal for executing this command, 2 questions will be asked.

1st question: 
"Type of package: (single, indep, library, python)
[s/i/l/p]?".

Must select "s"<br>

2nd question:
"Maintainer Name     : Sergey
Email-Address       : sergey@unknown
Date                : Thu, 16 Jul 2020 10:55:34 +0300
Package Name        : mydebpackagename
Version             : 0.0.1
License             : blank
Package Type        : single
Are the details correct? [Y/n/q]". 

Must select "Y"

If the command is successful, the runtime terminal will display "Done. Please edit the files in the debian/ subdirectory now."

**Stage 7**<br>

7. Change to the `Create_deb/debian` directory (from the `Create_dir` directory): `cd debian`

**Stage 8**<br>

8. Open the `changelog` file for editing: `nano changelog`

**Stage 9**<br>

9. Change the word `unstable` to the word `trusty`. Save and exit the file `(Ctrl+O, Ctrl+X)`

**Stage 10**<br>

10. Open the `rules` file for editing: `nano rules`

**Stage 11**<br>

11. In the `%` target, before the `dh $@` command, add the following line: `mkdir ~/DEB-Package-Creation/Example_deb_create/Run_deb/mydebpackagename_dir || true`

We will place the executable file of our program in this folder.

_Note: "|| true" is necessary so that when trying to re-create the `~/DEB-Package-Creation/Example_deb_create/Run_deb/mydebpackagename_dir` directory, the `deb` package creation script does not throw an error (and this script tries to execute this command several times (duplicating this command for some internal purposes - you can see on the Internet how to avoid this by overriding various default targets - but this problem is easier and more elegantly solved)_

**Stage 12**<br>

12. Create a file `mydebpackagename.install` and open it for editing:

```
mydebpackagename.install:
------------------------------------
hello_world_deb.c ~/DEB-Package-Creation/Example_deb_create/Run_deb/mydebpackagename_dir
hello_world_deb ~/DEB-Package-Creation/Example_deb_create/Run_deb/mydebpackagename_dir
Makefile ~/DEB-Package-Creation/Example_deb_create/Run_deb/mydebpackagename_dir
------------------------------------
```

**Stage 13**<br>

13. Now you need to change to the `Create_deb` directory from the `Create_deb/debian` directory: `cd ..`

**Stage 14**<br>

14. From the `Create_deb` directory, you need to run the command to build the `deb` package: `dpkg-buildpackage`

**Stage 15**<br>

15. After that, you need to go down one directory from the `Create_deb` directory (to the `Example_deb_create` directory): `cd ..`

**Stage 16**<br>

16. Run the `ls` command and make sure that the file `mydebpackagename_0.0.1-1_amd64.deb` has actually been created: `ls`

**Stage 17**<br>

17. Run the command to install the `deb` package: `sudo dpkg -i mydebpackagename_0.0.1–1_amd64.deb`

**Stage 18**<br>

18. From the `Example_deb_create` directory change to the `Example_deb_create/Run_deb/mydebpackagename_dir` directory: `cd Run_deb/mydebpackagename_dir`

**Stage 19**<br>

19. Make sure there are 3 files there (`hello_world_deb.c`, `hello_world_deb`, `Makefile`): `ls`

**Stage 20**<br>

20. Run the `hello_world_deb` executable and make sure it is running (the line `Hello, World` should appear in the console): `./hello_world_deb`

**Stage 21**<br>

21. If the line is displayed - **congratulations**, you have correctly compiled and installed a simple `deb` package with your own software!

### :bookmark_tabs: Licence ###
Creation deb-package is [CC BY-NC-SA 3.0 licensed](./LICENSE).
