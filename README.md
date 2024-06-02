## Laboratory work III
 
Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**
 
```sh
$ open https://cmake.org/
```
 
## Tasks
 
- [ ] 1. Создать публичный репозиторий с названием **lab03** на сервисе **GitHub**
- [ ] 2. Ознакомиться со ссылками учебного материала
- [ ] 3. Выполнить инструкцию учебного материала
- [ ] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**
 
## Tutorial
 
```sh
$ export GITHUB_USERNAME=<имя_пользователя>
```
 
```sh
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```
 
```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03
$ cd projects/lab03
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
```
 
```sh
$ g++ -std=c++11 -I./include -c sources/print.cpp
$ ls print.o
$ nm print.o | grep print
$ ar rvs print.a print.o
$ file print.a
$ g++ -std=c++11 -I./include -c examples/example1.cpp
$ ls example1.o
$ g++ example1.o print.a -o example1
$ ./example1 && echo
```
 
```sh
$ g++ -std=c++11 -I./include -c examples/example2.cpp
$ nm example2.o
$ g++ example2.o print.a -o example2
$ ./example2
$ cat log.txt && echo
```
 
```sh
$ rm -rf example1.o example2.o print.o
$ rm -rf print.a
$ rm -rf example1 example2
$ rm -rf log.txt
```
 
```sh
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(print)
EOF
```
 
```sh
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```
 
```sh
$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF
```
 
```sh
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```
 
```sh
$ cmake -H. -B_build
$ cmake --build _build
```
 
```sh
$ cat >> CMakeLists.txt <<EOF
 
add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
```
 
```sh
$ cat >> CMakeLists.txt <<EOF
 
target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```
 
```sh
$ cmake --build _build
$ cmake --build _build --target print
$ cmake --build _build --target example1
$ cmake --build _build --target example2
```
 
```sh
$ ls -la _build/libprint.a
$ _build/example1 && echo
hello
$ _build/example2
$ cat log.txt && echo
hello
$ rm -rf log.txt
```
 
```sh
$ git clone https://github.com/tp-labs/lab03 tmp
$ mv -f tmp/CMakeLists.txt .
$ rm -rf tmp
```
 
```sh
$ cat CMakeLists.txt
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
$ cmake --build _build --target install
$ tree _install
```
 
```sh
$ git add CMakeLists.txt
$ git commit -m"added CMakeLists.txt"
$ git push origin master
```
 
## Report
 
```sh
$ popd
$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```
 
## Homework
 
Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.
 
```sh
$ mkdir Lab03
$ cd Lab03
$ git init
$ git remote add origin https://github.com/kssseniya/Lab03.git
$ git pull origin master
$ sudo apt istall cmake
```
Перехожу в директорию `$ cd formatter_lib`
 
Создаю файл `$ touch CMakeLists.txt`
 
Редактирую файл `$ nano CMakeLists.txt`
 
Запишем в CMakeLists.txt:
```sh
cmake_minimum_required(VERSION 3.4)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
project(formatter)
add_library(formatter STATIC formatter.cpp)
target_include_directories(formatter PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
```
```sh
$ cmake -H. -B_build
```
Вывод:
```sh
-- The C compiler identification is GNU 12.2.0
-- The CXX compiler identification is GNU 12.2.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to:
/home/user/kssseniya/workspace/projects/Lab03/formatter_lib/_build
```
```sh
$ cmake --build _build
```
Вывод:
```sh
[ 50%] Building CXX object CMakeFiles/formatter.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter.a
[100%] Built target formatter
```
 
Закоммитим и запушим изменения:
```sh
$ cd ..
$ git add CMakeList.txt
$ git commit -m "created CMakeLists.txt"
$ git add _build
$ git commit -m "created build"
$ git push origin master
```
 
### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.
 
Перехожу в директорию `$ cd formatter_ex_lib`
 
Создаю файл `$ touch CMakeLists.txt`
 
Редактирую файл `$ nano CMakeLists.txt`
 
Запишем в CMakeLists.txt:
```sh
cmake_minimum_required(VERSION 3.4)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
project(formatter_ex_lib)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib formatter_lib_dir)
add_library(formatter_ex_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
target_include_directories(formatter_ex_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib)
target_link_libraries(formatter_ex_lib formatter_lib)
```
```sh
$ cmake -H. -B_build
```
```sh
$ cmake --build _build
```
Закоммитим и запушим изменения:
```sh
$ cd ..
$ git add CMakeList.txt
$ git commit -m "created CMakeLists.txt"
$ git add _build
$ git commit -m "created build"
$ git push origin master
```
### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

Перехожу в директорию `$ cd hello_world_application`
 
Создаю файл `$ touch CMakeLists.txt`
 
Редактирую файл `$ nano CMakeLists.txt`
 
Запишем в CMakeLists.txt:
```sh
cmake_minimum_required(VERSION 3.4)
project(Hello_World)
add_library(formatter_lib STATIC ../formatter_lib/formatter.cpp)
target_include_directories(formatter_lib PUBLIC ../formatter_lib)
add_library(formatter_ex_lib STATIC ../formatter_ex_lib/formatter_ex.cpp)
target_include_directories(formatter_ex_lib PUBLIC ../formatter_ex_lib)
add_executable(hello_world hello_world.cpp)
target_link_libraries(formatter_ex_lib formatter_lib)
target_link_libraries(hello_world formatter_ex_lib)
```
```sh
$ cmake -H. -B_build
```
```sh
$ cmake --build _build
```
Закоммитим и запушим изменения:
```sh
$ cd ..
$ git add CMakeList.txt
$ git commit -m "created CMakeLists.txt"
$ git add _build
$ git commit -m "created build"
$ git push origin master
```

Перехожу в директорию `$ cd solver_application`
 
Создаю файл `$ touch CMakeLists.txt`
 
Редактирую файл `$ nano CMakeLists.txt`

Запишем в CMakeLists.txt:
```sh
cmake_minimum_required(VERSION 3.2)
project(Solver)
add_library(formatter_lib STATIC ../formatter_lib/formatter.cpp)
target_include_directories(formatter_lib PUBLIC ../formatter_lib)
add_library(solver STATIC ../solver_lib/solver.cpp)
target_include_directories(solver PUBLIC ../solver_lib)
add_library(formatter_ex_lib STATIC ../formatter_ex_lib/formatter_ex.cpp)
target_include_directories(formatter_ex_lib PUBLIC ../formatter_ex_lib)
add_executable(solver_app equation.cpp)
target_link_libraries(formatter_ex_lib formatter_lib)
target_link_libraries(solver_app formatter_ex_lib)
target_link_libraries(solver_app solver)
```
```sh
$ cmake -B build
```
Вывод:
```sh
-- Configuring done
-- Generating done
-- Build files have been written to: /home/user/kssseniya/workspace/projects/Lab03/solver_application/build
```
```sh
$ cmake --build build
```
Вывод:
```sh
[ 25%] Built target formatter_lib
[ 37%] Building CXX object CMakeFiles/solver.dir/home/user/kssseniya/workspace/projects/Lab03/solver_lib/solver.cpp.o
[ 50%] Linking CXX static library libsolver.a
[ 50%] Built target solver
[ 62%] Building CXX object CMakeFiles/formatter_ex_lib.dir/home/user/kssseniya/workspace/projects/Lab03/formatter_ex_lib/formatter_ex.cpp.o
[ 75%] Linking CXX static library libformatter_ex_lib.a
[ 75%] Built target formatter_ex_lib
[ 87%] Building CXX object CMakeFiles/solver_app.dir/equation.cpp.o
[100%] Linking CXX executable solver_app
[100%] Built target solver_app
```
Закоммитим и запушим изменения:
```sh
$ cd ..
$ git add CMakeList.txt
$ git commit -m "created CMakeLists.txt"
$ git add _build
$ git commit -m "created build"
$ git push origin master
```
 
**Удачной стажировки!**
 
## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)
 
```
Copyright (c) 2015-2021 The ISC Authors
```
