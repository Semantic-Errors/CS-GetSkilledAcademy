# Section 2 - Installation

## Overall tools

There are many ways to write and compile our codes such as:

- Code in any text editor, save, compile and run from the command line using a C++ compiler
    - **text edditors:** [vscode](https://code.visualstudio.com/), [sublimetext](https://www.sublimetext.com/), ...
    - **compiler:** [gcc](https://gcc.gnu.org/), [clang](https://clang.llvm.org/), [MSVC](https://visualstudio.microsoft.com/vs/features/cplusplus/), ...
- **Online compilers**
    - [ideone](https://ideone.com/)
- **Using IDEs**
    - [CodeBlocks](https://www.codeblocks.org/)
    - [Eclipse](https://www.eclipse.org/)

Details on how to use and configure these tools were discussed in the sessions.\
In the following section, I will provide an overview of how I manage my C++ environment.

## Own Enviroment Setup

I Personally go with the first approach, coding in a text editor and using a terminal multiplexer: `nvim` and `tmux`.\
For the compilation process, I use `gcc` as my main compiler which usually comes with most Linux systems.


### C++ Compilers

C++ has many standards (e.g., `C++11`, `C++14`, `C++17`, `C++20`, ...), each supported by different compiler versions. It's crucial to know your compiler version to ensure compatibility with the desired C++ standard.


[C++ Compiler Support](https://en.cppreference.com/w/cpp/compiler_support) provides a list of compilers versions and their support for C++ language standards and features.


To check the version of __GCC__, use the following command:

```bash
$ g++ --version
g++ (GCC) 14.1.1 20240522
Copyright (C) 2024 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
``` 

Sample of compiling and running a C++ code using C++20:

```bash
$ g++ -std=c++20 main.cpp
$ ./a.out
```


**But why specifying the standard version during the compilation?**
- Simply having the latest compiler version doesn't ensure it uses the latest standard by default.
- You sometimes have to specify the standard version by using with the `-std` flag.
    - `-std=c++20` for C++20 standard
    - `-std=c++17` for C++17 standard
    - ...

we can take one more step further and insure my code is in the specified standard by compiling the cpp code [(standard.cpp)](https://github.com/AbdelrahmanBayoumi/C-Cpp-Notes/blob/main/cpp/Section%202%20-%20Installation/standard.cpp) provided in the repo.

try compiling the code with the following commands, first output may vary depending on your compiler version:
```bash
$ g++ standard.cpp
$ ./a.out
C++17
$ g++ -std=c++20 standard.cpp
$ ./a.out
C++20
```
as you have seen my compiler supports the `C++20` standard but by default it uses the `C++17` standard so I needed to specify the version of the standard I want to use.

### Docker and the GCC Docker Container

Sometimes, I face issues with compiler updates (e.g., can't easily install a specific version) due to changing the OS or developing on a new machine.


To avoid this, I use **Docker**. 
- a tool for packaging and excuting software that allows you to create and run isolated Docker Containers.


**Docker Containers** are lightweight, portable, standalone packages that include everything needed to run an application, encompassing code, runtime, system tools, libraries, and settings. They ensure consistent execution across different platforms and simplify complex software setups, making it easier to adopt new technologies swiftly.

Thankfully to **Docker Hub** for offering an official **GCC Docker Container**, ensuring a consistent environment and access to the latest GCC compilers. \
The [GCC Docker Container](https://hub.docker.com/_/gcc/) is regularly updated to provide the latest gcc environment, making it easy to maintain and update your development setup efficiently.


### Docker Project Setup

1. Install [Docker](https://docs.docker.com/engine/install/) in your machine, if you don't have it already.
2. Download the Dockerfile provided in this repo or from the [GCC Docker Container](https://hub.docker.com/_/gcc/) to your local machine inside you project folders.
3. To compile the code inside the Docker container, run the following command for Unix-based systems:
    ```bash
    $ docker run -it --rm -v "$(pwd)":/home/usr/project gcc
    ```
    *arguments:*
    - `-it`:
        - Runs the Docker container interactively, providing an interactive command line interface and allocating a pseudo-TTY.
    - `--rm`: 
        - Removes the container and its associated resources automatically after it exits. This ensures that resources are reclaimed and avoids accumulating unused containers
        - by default if you aren't using this option 
            - Any resources (such as disk space or network ports) allocated to the container will not be automatically released 
            - every time you use `docker run` again it's going to reallocate a new resource. 
    - `-v "$(pwd)":/home/usr/project`: 
        - to mount the current directory to the container's `/home/usr/project` directory
        - NOTES:
            - we put `"$(pwd)"` between the double quotes to avoid any issues with spaces in the path
    
    for Windows users, you can use the following command:
    ```bash
    $ docker run -it --rm -v "%CD%":/home/usr/project gcc
    ```
    - `%CD%` 
        - refers to the current directory in Windows
4. after it finishes, it will pop you into the container where you will find your code inside the `/home/usr/project` directory as specified in the arguments before.
5. Edit your code from the main machine from any editor you want, and compile it using the `g++` compiler - **inside the container** - as mentioned in the previous sections.
6. To exit the container, type `exit` and press `Enter` or `Ctrl + D`.
    - NOTE: don't exit the container unless you have finished your work or you will start over from step 3 again.

