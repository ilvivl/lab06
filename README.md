[![Build Status](https://travis-ci.com/ilvivl/lab04.svg?branch=master)](https://travis-ci.com/ilvivl/lab04)
## Laboratory work IV

<a href="https://yandex.ru/efir/?stream_id=vCgeA9EiySzw"><img src="https://raw.githubusercontent.com/tp-labs/lab04/master/preview.png" width="640"/></a>

Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса **Travis CI**

```sh
$ open https://travis-ci.org
```

## Tasks

- [x] 1. Авторизоваться на сервисе **Travis CI** с использованием **GitHub** аккаунта
- [x] 2. Создать публичный репозиторий с названием **lab04** на сервисе **GitHub**
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Включить интеграцию сервиса **Travis CI** с созданным репозиторием
- [x] 5. Получить токен для **Travis CLI** с правами **repo** и **user**
- [x] 6. Получить фрагмент вставки значка сервиса **Travis CI** в формате **Markdown**
- [x] 7. Выполнить инструкцию учебного материала
- [x] 8. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=ilvivl #define value GITHUB_USERNAME
$ export GITHUB_TOKEN=********** #define value GITHUB_TOKEN
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ pushd . #save the current working directory in memory
~/Documents/acro/ilvivl/workspace ~/Documents/acro/ilvivl/workspace
$ source scripts/activate #read and execute file activate
```

```sh
$ \curl -sSL https://get.rvm.io | bash -s -- --ignore-dotfiles #curl  is  a tool to transfer data from or to a server, update packages
Installation of RVM in /home/ilya/.rvm/ is almost complete:

  * To start using RVM you need to run `source /home/ilya/.rvm/scripts/rvm`
    in all your open shell windows, in rare cases you need to reopen all shell windows.
Thanks for installing RVM 🙏
$ echo "source $HOME/.rvm/scripts/rvm" >> scripts/activate
$ . scripts/activate
$ rvm autolibs disable # rvm - The Ruby Version Manager, disable automatic dependency installation
$ rvm install ruby-2.4.2
Searching for binary rubies, this might take some time.
Found remote file https://rvm_io.global.ssl.fastly.net/binaries/ubuntu/19.10/x86_64/ruby-2.4.2.tar.bz2
ruby-2.4.2 - #configure
ruby-2.4.2 - #download
$ rvm use 2.4.2 --default #set default ruby language manager
Using /home/ilya/.rvm/gems/ruby-2.4.2
$ gem install travis
Done installing documentation for multipart-post, faraday, faraday_middleware, highline, concurrent-ruby, i18n, thread_safe, tzinfo, activesupport, multi_json, public_suffix, addressable, net-http-persistent, net-http-pipeline, gh, launchy, ffi, ethon, typhoeus, websocket, pusher-client, travis after 15 seconds
22 gems installed

```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab03 projects/lab04
Cloning into 'projects/lab04'...
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 88 (delta 7), reused 4 (delta 2), pack-reused 73
Unpacking objects: 100% (88/88), done.
$ cd projects/lab04
$ git remote remove origin #remove remote origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04 #add remote origin
```

```sh
$ cat > .travis.yml <<EOF #install instruments for the cpp compilation
language: cpp
EOF
```

```sh
$ cat >> .travis.yml <<EOF #in the build phase

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
```

```sh
$ cat >> .travis.yml <<EOF #packages

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
```

```sh
$ travis login --github-token ${GITHUB_TOKEN} #work with Travis
Successfully logged in as ilvivl!
```

```sh
$ travis lint #check the yml file for the validity
Hooray, .travis.yml looks valid :)
```

```sh
$ ex -sc '1i|<фрагмент_вставки_значка>' -cx README.md
```

```sh
$ git add .travis.yml
$ git add README.md
$ git commit -m"added CI"
[master bb33f2f] added CI
 2 files changed, 15 insertions(+)
 create mode 100644 .travis.yml
$ git push origin master
Username for 'https://github.com': ilvivl
Password for 'https://ilvivl@github.com': 
Enumerating objects: 92, done.
Counting objects: 100% (92/92), done.
Delta compression using up to 4 threads
Compressing objects: 100% (89/89), done.
Writing objects: 100% (92/92), 1.02 MiB | 14.31 MiB/s, done.
Total 92 (delta 43), reused 0 (delta 0)
remote: Resolving deltas: 100% (43/43), done.
To https://github.com/ilvivl/lab04
 * [new branch]      master -> master
```

```sh
$ travis lint #check the yml file for the validity
Hooray, .travis.yml looks valid :)
$ travis accounts #Travis accounts
ilvivl (Ilya_Vinogradov): subscribed, 11 repositories
$ travis sync #synchronize Github repos and Travis
synchronizing: . done
$ travis repos #watch repos
...
ilvivl/lab03 (active: no, admin: yes, push: yes, pull: yes)
Description: ???

ilvivl/lab04 (active: yes, admin: yes, push: yes, pull: yes)
Description: ???
...
$ travis enable # watch, what are enable and do
Detected repository as ilvivl/lab04, is this correct? |yes| yes
ilvivl/lab04: enabled :)
$ travis whatsup #projects statu#show building historys 
ilvivl/lab04 passed: #2
$ travis branches #branch infromation
master:  #2    passed     fix
$ travis history #building history
#2 passed:       master fix
#1 failed:       master added CI
$ travis show #show last buildingpo
Job #2.1:  fix
State:         passed
Type:          push
Branch:        master
Compare URL:   https://github.com/ilvivl/lab04/compare/bb33f2f924fe...e7924eb223eb
Duration:      35 sec
Started:       2020-05-18 00:10:26
Finished:      2020-05-18 00:11:01
Allow Failure: false
Config:        os: linux
```

## Report

```sh
$ popd
~/Documents/acro/ilvivl/workspace
$ export LAB_NUMBER=04
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
Cloning into 'tasks/lab04'...
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (14/14), done.
remote: Total 81 (delta 4), reused 2 (delta 1), pack-reused 66
Unpacking objects: 100% (81/81), done.
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Homework

Вы продолжаете проходить стажировку в "Formatter Inc." (см [подробности](https://github.com/tp-labs/lab03#Homework)).

В прошлый раз ваше задание заключалось в настройке автоматизированной системы **CMake**.

Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в [прошлый раз](https://github.com/tp-labs/lab03#Homework). Настройте сборочные процедуры на различных платформах:
* используйте [TravisCI](https://travis-ci.com/) для сборки на операционной системе **Linux** с использованием компиляторов **gcc** и **clang**;
* используйте [AppVeyor](https://www.appveyor.com/) для сборки на операционной системе **Windows**.

## Links

- [Travis Client](https://github.com/travis-ci/travis.rb)
- [AppVeyour](https://www.appveyor.com/)
- [GitLab CI](https://about.gitlab.com/gitlab-ci/)

```
Copyright (c) 2015-2020 The ISC Authors
```
