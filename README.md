# RPP-M (Rec-Play-Pause Micro)
 Template repository for cross-platform cpp development based on CMake build system Resources

> **NOTE :** This repository is tested only on MacOS and debian based Linux Distributions. Will be updated for Windows in the future.

### Who is this repository for?
* If project based on CMakeLists build system.
* If project contains only few source files like `main.cpp`, `mylib.cpp` etc.
* If you need a C++ development laboratuary to test simple codes does not requires any 3Rd dependencies.

### Requirements:
* Cmake
* Make (optional)
* Git
* GNU Compiler Collection (gcc, g++ etc.)

### Extras : 
* `License.txt`, you can license your software by editing this file.

---

## Project Structure
### Zero Files/Dirs :
* **`main.cpp` file :** Your C++ file.
* **`rppmConfig.h.in` file :** Configuration file of the project.
* **`bake.sh` file :**  Baker file for tests.

### Created Files/Dirs :
* **`out/app` directory :** Installation directory of the CMake
* **`out/build` directory :** Build output of the CMake 
* **`out/package` directory :** Package output of the CMake 

## Preparing Development Environment
    ```
    # clone the repo
    git clone git@github.com:eheperson/rpp-m.git

    # change access rights of bake.sh file
    chmod +x bake.sh
    ```

---

## Development

* Use the `bake.sh` file for developement. 
* Purpose of the `bake.sh` file is handling all *build*, *install* and *packing* procedures in isolated directory named as `out/`.
* `CMAKE_INSTALL_PREFIX` option is modified to install targets in `/out` directory.

### Steps

1. Write your code.
2. Edit the `CMakeLists.txt` file for proper linking, compiling and installing.
3. Run the `bake.sh` file.

---

## Production

* After your development is done, you can follow the steps below to build and pack your application.

### 1. Configure : 

```
    cmake -S . -B ./out/build
```

### 2. Build : 

```
    # option 1 : building by cmake
    cd ./out/build && cmake --build . -v 

    # option 2 : building by make itself 
    # ( there is no any major difference between option1 and option 2)
    cd ./out/build && make 

    # alternatively you can use -j parameters with make command to speed up build process
    cd ./out/build && make -j7
```

### 3. Install : 

The project will configured to install common unix install directories like : `usr/slare/local`, `/usr/local` etc.

> you may require run install commands by `sudo`, because installing process will install your project to the one of the root directories.

```
    # option 1 :
    cd ./out/build && cmake --install .

    # option 2 : 
    cd ./out/build && make install
```

### 4. Packing : 

```
    cd ./out/build && cpack -C Release
```
## Adding more .cpp files :

In the `CMakeLists.txt` there are an example of adding a new executable file.
As a first step, create an executable like `temp.cpp`.

```
# Step 1 : adding executable (CMakeLists.txt line:57)
add_executable(temp temp.cpp)

# Step 2 : adding binary tree to the search path (CMakeLists.txt line:58)
# if you will not use rppmConfig.h in your executable, this step is unnecessary.
target_include_directories(temp PUBLIC "${PROJECT_BINARY_DIR}") 

# Step 3 : install the executable (CMakeLists.txt line:67)
install(TARGETS temp DESTINATION bin)
```

> Optionally, you can add new executable as a dependency of main.cpp like : 
> ```
> add_executable(${CMAKE_PROJECT_NAME} main.cpp temp.cpp)   
> ```

## Extra Info

* If your executables dependen to each other just add them like below and yo do not have to change anything else : 
    ```
        # Step 0 : adding dependent executables (CMakeLists.txt line:29)
        add_executable(${CMAKE_PROJECT_NAME} main.cpp dep1.cpp dep2.cpp)  
    ```
