## Laboratory work III

## Report

### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.
```
cd ~/worksapce/tasks/lab03/formatter_lib
touch CMakeLists.txt
nano CMakeLists.txt

cmake_minimum_required(VERSION 3.4)
project(formatter_lib)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(formatter_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)

cmake -B build
cmake --build build
```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.
```
cd ../formatter_ex_lib
touch CMakeLists.txt
nano CMakeLists.txt

cmake_minimum_required(VERSION 3.4)
project(formatter_ex_lib)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib formatter_lib_dir)
add_library(formatter_ex_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
target_include_directories(formatter_ex_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib)
target_link_libraries(formatter_ex_lib formatter_lib)

cmake -B build
cmake --build build
```

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

hello_world:
```
cd ../hello_world_application
touch CMakeLists.txt
nano CMakeLists.txt

cmake_minimum_required(VERSION 3.4)
project(hello_world)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib formatter_ex_lib_dir)
add_executable(hello_world ${CMAKE_CURRENT_SOURCE_DIR}/hello_world.cpp)
target_include_directories(hello_world PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib)
target_link_libraries(hello_world PRIVATE formatter_ex_lib)

cmake -B build
cmake --build build
build/hello_world
// вывод строки "hello world"
```

solver:
1) собираем библиотеку solver_lib
```
cd ../solver_lib
touch CMakeLists.txt
nano CMakeLists.txt

cmake_minimum_required(VERSION 3.4)
project(solver_lib)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(solver_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/solver.cpp)
```
2) создаем исполняемый файл, подключая созданные библиотеки
```
cd ../solver_application
touch CMakeLists.txt
nano CMakeLists.txt

cmake_minimum_required(VERSION 3.4)
project(solver)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib formatter_ex_lib_dir)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib solver_lib_dir)
add_executable(solver ${CMAKE_CURRENT_SOURCE_DIR}/equation.cpp)
target_include_directories(solver PRIVATE
${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib
${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib)
target_link_libraries(solver PRIVATE formatter_ex_lib solver_lib)

cmake -B build
cmake --build build
build/solver
// вывод корней квадратного уравнения
```
