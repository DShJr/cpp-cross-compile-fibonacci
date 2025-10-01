# 🛠️ Cross-Compilation Practice: Fibonacci (C++/CMake/MinGW)

## EN | English Version

# 🛠️ Cross-Compilation Practice: Fibonacci (C++/CMake/MinGW)

**Description:** This project demonstrates the **cross-compilation** process for a simple C++ application that calculates Fibonacci numbers. The build targets **Windows** (x64) from a **Linux (WSL)** environment.

**Goal:** To create a **self-contained `.exe` file** for Windows by using static linking, thus eliminating the dependency on external DLLs (like `libstdc++-6.dll`).

### 📁 Project Structure

```
FIBONACCI_CROSS/
├── main.cpp            # C++ Source Code
├── CMakeLists.txt      # Main CMake instructions
├── toolchain.cmake     # Toolchain file for the cross-compiler (MinGW-w64)
└── build_win/          # Output folder for the Windows executable
```

### 💻 Source Code (`main.cpp`)

The program calculates and prints the first five Fibonacci numbers. `std::cin.get()` is included to pause the console window on execution.

```cpp
#include <iostream>

int main() {
    double f1 = 0.0;
    double f2 = 1.0;
    double f3 = f1 + f2;
    double f4 = f2 + f3;
    double f5 = f3 + f4;
    
    std::cout << "Fibonacci number 1: " << f1 << std::endl;
    std::cout << "Fibonacci number 2: " << f2 << std::endl;
    std::cout << "Fibonacci number 3: " << f3 << std::endl;
    std::cout << "Fibonacci number 4: " << f4 << std::endl;
    std::cout << "Fibonacci number 5: " << f5 << std::endl;

    // Pause to prevent the window from closing immediately
    std::cout << "Press Enter to close the window..." << std::endl;
    std::cin.get();

    return 0;
}
```

### ⚙️ CMake Configuration

**`CMakeLists.txt` Code**

This file sets the minimum CMake version, names the project, and uses `CMAKE_BUILD_TYPE` to generate either a `_debug` or `_release` executable.

```cmake
cmake_minimum_required(VERSION 3.10)
project(FibonacciCross)
    
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    add_executable(fibonacci_app_debug main.cpp)
else()
    add_executable(fibonacci_app_release main.cpp)
endif()
```

**`toolchain.cmake` Code**

The critical file that sets up MinGW-w64 as the compiler and explicitly includes the `-static` flag for static linking.

```cmake
# Specify the target system is Windows
set(CMAKE_SYSTEM_NAME Windows)
set(CMAKE_SYSTEM_PROCESSOR x86_64)

# Specify the paths to the cross-compiler
set(CMAKE_C_COMPILER /usr/bin/x86_64-w64-mingw32-gcc)
set(CMAKE_CXX_COMPILER /usr/bin/x86_64-w64-mingw32-g++)

# Set search paths to prevent CMake from using Linux system libraries
set(CMAKE_FIND_ROOT_PATH /usr/x86_64-w64-mingw32)
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)

# Add the -static flag to embed standard libraries and resolve the DLL error
set(CMAKE_EXE_LINKER_FLAGS "-static")
```

### 🚀 Build Process

The build is executed from the `build_win/` folder, specifying the `toolchain.cmake` file.

```bash
# 1. Create and navigate to the build folder
mkdir build_win
cd build_win

# 2. Generate the build (using toolchain.cmake) and compile
cmake -DCMAKE_TOOLCHAIN_FILE=../toolchain.cmake .. && cmake --build .
```

### ✅ Result

The file **`fibonacci_app_release.exe`** is successfully created in the `build_win/` directory and runs on the target Windows platform.

**Output when running on Windows:**

```
Fibonacci number 1: 0
Fibonacci number 2: 1
Fibonacci number 3: 1
Fibonacci number 4: 2
Fibonacci number 5: 3

Press Enter to close the window...
```

*(Note: An issue with Russian characters (Cyrillic) in the Windows console was encountered, but will be fixed after resolving MinGW character encoding settings.)*


-----
## RU | Русская версия

**Описание:** Данный проект демонстрирует процесс **кросс-компиляции** простого C++ приложения, которое вычисляет числа Фибоначчи. Сборка выполняется для целевой платформы **Windows** (x64) из среды **Linux (WSL)**.

**Цель:** Создать **автономный `.exe` файл** для Windows, используя статическую компоновку, чтобы избежать зависимости от внешних библиотек (DLL), таких как `libstdc++-6.dll`.

### 📁 Структура Проекта

```
FIBONACCI_CROSS/
├── main.cpp            # Исходный код C++
├── CMakeLists.txt      # Основные инструкции для CMake
├── toolchain.cmake     # Файл настроек для кросс-компилятора (MinGW-w64)
└── build_win/          # Папка для сборки .exe файла
```

### 💻 Исходный Код (`main.cpp`)

Программа вычисляет и выводит первые пять чисел Фибоначчи. Для паузы добавлен `std::cin.get()`.

```cpp
#include <iostream>

int main() {
    double f1 = 0.0;
    double f2 = 1.0;
    double f3 = f1 + f2;
    double f4 = f2 + f3;
    double f5 = f3 + f4;
    
    std::cout << "Fibonacci number 1: " << f1 << std::endl;
    std::cout << "Fibonacci number 2: " << f2 << std::endl;
    std::cout << "Fibonacci number 3: " << f3 << std::endl;
    std::cout << "Fibonacci number 4: " << f4 << std::endl;
    std::cout << "Fibonacci number 5: " << f5 << std::endl;

    // Добавляем паузу, чтобы окно не закрывалось сразу
    std::cout << "Нажмите Enter, чтобы закрыть окно..." << std::endl;
    std::cin.get();

    return 0;
}
```

### ⚙️ Настройка CMake

**Код `CMakeLists.txt`**

Этот файл определяет минимальную версию CMake и название проекта, а также использует переменную `CMAKE_BUILD_TYPE` для создания `_debug` или `_release` версии исполняемого файла.

```cmake
cmake_minimum_required(VERSION 3.10)
project(FibonacciCross)
    
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    add_executable(fibonacci_app_debug main.cpp)
else()
    add_executable(fibonacci_app_release main.cpp)
endif()
```

**Код `toolchain.cmake`**

Ключевой файл, который устанавливает MinGW-w64 как компилятор и явно добавляет флаг `-static` для статической компоновки.

```cmake
# Указываем, что мы делаем кросс-компилятор
set(CMAKE_SYSTEM_NAME Windows)
set(CMAKE_SYSTEM_PROCESSOR x86_64)

# Указываем пути к кросс-компилятору
set(CMAKE_C_COMPILER /usr/bin/x86_64-w64-mingw32-gcc)
set(CMAKE_CXX_COMPILER /usr/bin/x86_64-w64-mingw32-g++)

# Устанавливаем пути поиска, чтобы CMake не использовал библиотеки Linux
set(CMAKE_FIND_ROOT_PATH /usr/x86_64-w64-mingw32)
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)

# Добавим флаг -static, чтобы встроить стандартные библиотеки и устранить ошибку DLL
set(CMAKE_EXE_LINKER_FLAGS "-static")
```

### 🚀 Сборка

Сборка выполняется из папки `build_win/` с обязательным указанием `toolchain.cmake`.

```bash
# 1. Создаем и переходим в папку для сборки
mkdir build_win
cd build_win

# 2. Генерация сбоорки (используем toolchain.cmake) и компиляция
cmake -DCMAKE_TOOLCHAIN_FILE=../toolchain.cmake .. && cmake --build .
```

### ✅ Результат

В директории `build_win/` создан файл **`fibonacci_app_release.exe`**, который успешно запускается на целевой платформе Windows.

**Вывод при запуске в Windows:**

```
Fibonacci number 1: 0
Fibonacci number 2: 1
Fibonacci number 3: 1
Fibonacci number 4: 2
Fibonacci number 5: 3

Нажмите Enter, чтобы закрыть окно...
```

*(Примечание: Проблема с кириллицей в консоли Windows возникла, но будет исправлена после изучения настроек кодировки MinGW.)*
