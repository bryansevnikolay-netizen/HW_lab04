## Laboratory work IV

## Report

### Задание
Настроить систему непрерывной интеграции для библиотек и приложений, 
с которыми вы работали в прошлый раз. 
Настройте сборочные процедуры на различных платформах (Windows и Linux) в Git Actions.
```
name: CI for Lab04    // имя workflow

on:    условия запуска
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]
jobs:    // задачи
  build-linux:  // задача в Linux
    runs-on: ubuntu-latest    // запуск на виртуальной машине ubuntu
    steps:
    - uses: actions/checkout@v4    // скачивание кода из репозитория
    - name: Configure CMake    // настройка CMake для сборки проекта
      run: cmake -B build -S .
    - name: Build    // компиляция проекта
      run: cmake --build build -j2
    - name: Run solver    // запусе проекта solver
      run: ./build/solver_application/solver
    - name: Run hello_world    // запуск проекта hello_world
      run: ./build/hello_world_application/hello_world
     
  build-windows:    // задача в Windows
    runs-on: windows-latest
    steps:
    - uses: actions/checkout@v4    // скачивание кода из репозитория
    - name: Configure CMake     // настройка CMake для сборки проекта
      run: cmake -B build -S .
    - name: Build    // компиляция проекта
      run: cmake --build build --config Release
    - name: Run solver    // запуск проекта solver
      run: ./build/solver_application/Release/solver.exe
    - name: Run hello_world    // запуск проекта hello_world
      run: ./build/hello_world_application/Release/hello_world.exe
```
