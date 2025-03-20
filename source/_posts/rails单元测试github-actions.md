---
title: rails单元测试github actions
date: 2021-07-08 00:14:24
tags: ['github actions', '持续集成']
---

学 rails 时用到了单元测试，那就用 github actions 来跑测试吧。

下面是workflow配置：
<!-- more -->
```
name: Ruby

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  test:

    runs-on: ubuntu-latest

    services:
      mysql:
        image: mysql:5.7
        env:
          MYSQL_ROOT_PASSWORD: root
          MYSQL_DATABASE: app_db_test
        ports:
          - 3306
        options: >-
          --health-cmd="mysqladmin ping"
          --health-interval=10s
          --health-timeout=5s
          --health-retries=3

    steps:
      - uses: actions/checkout@v1

      - name: Set up Ruby 2.6.6
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 2.6.6

      - name: Gem cache
        uses: actions/cache@v1
        with:
          path: vendor/bundle
          key: ${{ runner.os }}-gems-${{ hashFiles('**/Gemfile.lock') }}
          restore-keys: |
            ${{ runner.os }}-gems-

      - name: Verify MySQL connection from host
        run: |
          sudo apt-get install -y mysql-client libmysqlclient-dev
          sudo /etc/init.d/mysql start
          mysql -h 127.0.0.1 --port ${{ job.services.mysql.ports[3306] }} -u root -proot -e "CREATE DATABASE IF NOT EXISTS app_db_test;"

      - name: Bundle install, setup DB and run tests
        env:
          RAILS_ENV: test
          DB_PASSWORD: root
          DB_PORT: ${{ job.services.mysql.ports[3306] }}
        run: |
          cp config/database.yml.ci config/database.yml
          gem install bundler
          bundle config path vendor/bundle
          bundle install --jobs 4 --retry 3
          bundle exec rails db:setup
          bundle exec rails test
```

database.yml.ci 配置：

```
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: root

test:
  <<: *default
  database: app_db_test

```
