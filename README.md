[![Build Status](https://travis-ci.org/ilvivl/lab05.svg?branch=master)](https://travis-ci.com/ilvivl/lab05)
## Laboratory work V

<a href="https://yandex.ru/efir/?stream_id=vQw_LH0UfN6I"><img src="https://raw.githubusercontent.com/tp-labs/lab05/master/preview.png" width="640"/></a>

Данная лабораторная работа посвещена изучению фреймворков для тестирования на примере **GTest**

```sh
$ open https://github.com/google/googletest
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab05** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=ilvivl #define value GITHUB_USERNAME
$ alias gsed=sed # for *-nix system
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
~/Documents/acro/ilvivl/workspace ~/Documents/acro/ilvivl/workspace
$ source scripts/activate
```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab04 projects/lab05
Cloning into 'projects/lab05'...
remote: Enumerating objects: 101, done.
remote: Counting objects: 100% (101/101), done.
remote: Compressing objects: 100% (52/52), done.
remote: Total 101 (delta 44), reused 101 (delta 44), pack-reused 0
Receiving objects: 100% (101/101), 1.02 MiB | 1.60 MiB/s, done.
Resolving deltas: 100% (44/44), done.
$ cd projects/lab05
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab05
```

```sh
$ mkdir third-party
$ git submodule add https://github.com/google/googletest third-party/gtest #add a test framework as a submodule of the repository
Cloning into '/home/ilya/Documents/acro/ilvivl/workspace/projects/lab05/third-party/gtest'...
remote: Enumerating objects: 20, done.
remote: Counting objects: 100% (20/20), done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 20303 (delta 3), reused 10 (delta 3), pack-reused 20283
Receiving objects: 100% (20303/20303), 7.54 MiB | 1.96 MiB/s, done.
Resolving deltas: 100% (15006/15006), done.
$ cd third-party/gtest && git checkout release-1.8.1 && cd ../.. #stay on stable version 1.8.1
HEAD is now at 2fe3bd99 Merge pull request #1433 from dsacre/fix-clang-warnings
$ git add third-party/gtest
$ git commit -m"added gtest framework"
[master da9671a] added gtest framework
 2 files changed, 4 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 third-party/gtest
```

```sh
$ gsed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\ #sed - stream editor for filtering and transforming text,  insert after the line "option(BUILD_EXAMPLES "Build examples" OFF)" line "option(BUILD_TESTS "Build tests" OFF)" into CMakeLists
option(BUILD_TESTS "Build tests" OFF)
' CMakeLists.txt
$ cat >> CMakeLists.txt <<EOF #add target of test

if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
  add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
  target_link_libraries(check \${PROJECT_NAME} gtest_main)
  add_test(NAME check COMMAND check)
endif()
EOF
```

```sh
$ mkdir tests
$ cat > tests/test1.cpp <<EOF #test
#include <print.hpp>

#include <gtest/gtest.h>

TEST(Print, InFileStream)
{
  std::string filepath = "file.txt";
  std::string text = "hello";
  std::ofstream out{filepath};

  print(text, out);
  out.close();

  std::string result;
  std::ifstream in{filepath};
  in >> result;

  EXPECT_EQ(result, text);
}
EOF
```

```sh
$ cmake -H. -B_build -DBUILD_TESTS=ON #building
-- Build files have been written to: /home/ilya/Documents/acro/ilvivl/workspace/projects/lab05/_build
$ cmake --build _build # build subdirectory
[ 83%] Built target gmock
Scanning dependencies of target gmock_main
[ 91%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library libgmock_main.a
[100%] Built target gmock_main
$ cmake --build _build --target test #test checking
Running tests...
Test project /home/ilya/Documents/acro/ilvivl/workspace/projects/lab05/_build
    Start 1: check
1/1 Test #1: check ............................   Passed    0.02 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.03 sec
```

```sh
$ _build/check # run tests entirely
[==========] Running 1 test from 1 test case.
[----------] Global test environment set-up.
[----------] 1 test from Print
[ RUN      ] Print.InFileStream
[       OK ] Print.InFileStream (1 ms)
[----------] 1 test from Print (1 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test case ran. (1 ms total)
[  PASSED  ] 1 test.
$ cmake --build _build --target test -- ARGS=--verbose #detail building
Running tests...
UpdateCTestConfiguration  from :/home/ilya/Documents/acro/ilvivl/workspace/projects/lab05/_build/DartConfiguration.tcl
UpdateCTestConfiguration  from :/home/ilya/Documents/acro/ilvivl/workspace/projects/lab05/_build/DartConfiguration.tcl
Test project /home/ilya/Documents/acro/ilvivl/workspace/projects/lab05/_build
Constructing a list of tests
Done constructing a list of tests
Updating test list for fixtures
Added 0 tests to meet fixture requirements
Checking test dependency graph...
Checking test dependency graph end
test 1
    Start 1: check

1: Test command: /home/ilya/Documents/acro/ilvivl/workspace/projects/lab05/_build/check
1: Test timeout computed to be: 10000000
1: Running main() from /home/ilya/Documents/acro/ilvivl/workspace/projects/lab05/third-party/gtest/googletest/src/gtest_main.cc
1: [==========] Running 1 test from 1 test case.
1: [----------] Global test environment set-up.
1: [----------] 1 test from Print
1: [ RUN      ] Print.InFileStream
1: [       OK ] Print.InFileStream (0 ms)
1: [----------] 1 test from Print (0 ms total)
1: 
1: [----------] Global test environment tear-down
1: [==========] 1 test from 1 test case ran. (0 ms total)
1: [  PASSED  ] 1 test.
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.00 sec

```

```sh
$ gsed -i 's/lab04/lab05/g' README.md #edit files in place (makes backup if SUFFIX supplied)
$ gsed -i 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml #add code for test building in travis.yml
$ gsed -i '/cmake --build _build --target install/a\ #add code for test launching
- cmake --build _build --target test -- ARGS=--verbose
' .travis.yml
```

```sh
$ travis lint #check travis
Hooray, .travis.yml looks valid :)
```

```sh
$ git add .travis.yml
$ git add tests
$ git add -p #look, what didn'ty add
diff --git a/CMakeLists.txt b/CMakeLists.txt
index 96a361e..89739e7 100644
--- a/CMakeLists.txt
+++ b/CMakeLists.txt
@@ -4,6 +4,7 @@ set(CMAKE_CXX_STANDARD 11)
 set(CMAKE_CXX_STANDARD_REQUIRED ON)
 
 option(BUILD_EXAMPLES "Build examples" OFF)
+option(BUILD_TESTS "Build tests" OFF)
 
 project(print)
 
Stage this hunk [y,n,q,a,d,j,J,g,/,e,?]? y
@@ -34,3 +35,11 @@ install(TARGETS print
 
 install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
 install(EXPORT print-config DESTINATION cmake)
+if(BUILD_TESTS)
+  enable_testing()
+  add_subdirectory(third-party/gtest)
+  file(GLOB ${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
+  add_executable(check ${${PROJECT_NAME}_TEST_SOURCES})
+  target_link_libraries(check ${PROJECT_NAME} gtest_main)
+  add_test(NAME check COMMAND check)
+endif()
Stage this hunk [y,n,q,a,d,K,g,/,e,?]? y

diff --git a/README.md b/README.md
index 095c4d4..a25491a 100644
--- a/README.md
+++ b/README.md
@@ -1,4 +1,4 @@
-[![Build Status](https://travis-ci.com/ilvivl/lab04.svg?branch=master)](https://travis-ci.com/ilvivl/lab04)
+[![Build Status](https://travis-ci.com/ilvivl/lab05.svg?branch=master)](https://travis-ci.com/ilvivl/lab05)
 ## Laboratory work III
 
 <a href="https://yandex.ru/efir/?stream_id=vjKAlxJ0UQrs"><img src="https://raw.githubusercontent.com/tp-labs/lab03/master/preview.png" width="640"/></a>
Stage this hunk [y,n,q,a,d,e,?]? y
$ git commit -m"added tests"
[master 3745a37] added tests
 4 files changed, 31 insertions(+), 2 deletions(-)
 create mode 100644 tests/test1.cpp
$ git push origin master
Username for 'https://github.com': ilvivl
Password for 'https://ilvivl@github.com': 
Enumerating objects: 116, done.
Counting objects: 100% (116/116), done.
Delta compression using up to 4 threads
Compressing objects: 100% (65/65), done.
Writing objects: 100% (116/116), 1.02 MiB | 41.89 MiB/s, done.
Total 116 (delta 49), reused 98 (delta 44)
remote: Resolving deltas: 100% (49/49), done.
To https://github.com/ilvivl/lab05
 * [new branch]      master -> master
```

```sh
$ travis login --auto
Successfully logged in as ilvivl!
$ travis enable #enable repo
Detected repository as ilvivl/lab05, is this correct? |yes| yes
ilvivl/lab05: enabled :)
```

```sh
$ mkdir artifacts
$ sleep 20s && gnome-screenshot --file artifacts/screenshot.png #make an save a screenshot
# for macOS: $ screencapture -T 20 artifacts/screenshot.png
# open https://github.com/${GITHUB_USERNAME}/lab05
```

## Report

```sh
$ popd
$ export LAB_NUMBER=05
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Homework

### Задание
1. Создайте `CMakeList.txt` для библиотеки *banking*.
2. Создайте модульные тесты на классы `Transaction` и `Account`.
    * Используйте mock-объекты.
    * Покрытие кода должно составлять 100%.
3. Настройте сборочную процедуру на **TravisCI**.
4. Настройте [Coveralls.io](https://coveralls.io/).

## Links

- [C++ CI: Travis, CMake, GTest, Coveralls & Appveyor](http://david-grs.github.io/cpp-clang-travis-cmake-gtest-coveralls-appveyor/)
- [Boost.Tests](http://www.boost.org/doc/libs/1_63_0/libs/test/doc/html/)
- [Catch](https://github.com/catchorg/Catch2)

```
Copyright (c) 2015-2020 The ISC Authors
```
