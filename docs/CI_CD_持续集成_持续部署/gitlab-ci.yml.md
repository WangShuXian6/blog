>.gitlab-ci.yml

```yaml
include: "/.variables.yml"

stages:
  #- test
  - build
  - deploy

cache:
  #key: ${CI_COMMIT_REF_NAME}
  paths:
    - node_modules/

#test_dev:
#  image: node:alpine
#  stage: test
#  only:
#    - dev
#  tags:
#    - shell
#  script:
#    - npm run test


build_devlop:
  #image: node:alpine
  stage: build
  only:
    - develop
  tags:
    - shell
  script:
    - wget https://nodejs.org/dist/v10.15.3/node-v10.15.3-linux-x64.tar.xz
    - xz -d  node-v10.15.3-linux-x64.tar.xz
    - tar -xvf node-v10.15.3-linux-x64.tar
    - ./node-v10.15.3-linux-x64/bin/npm set registry https://registry.npm.taobao.org # 设置淘宝镜像地址
    - ./node-v10.15.3-linux-x64/bin/npm install --progress=false
    - ./node-v10.15.3-linux-x64/bin/npm run build:dev
  artifacts:
    expire_in: 1 week
    paths:
    - dist

build_master:
  #image: node:alpine
  stage: build
  only:
    - master
  tags:
    - shell
  script:
    - wget https://nodejs.org/dist/v10.15.3/node-v10.15.3-linux-x64.tar.xz
    - xz -d  node-v10.15.3-linux-x64.tar.xz
    - tar -xvf node-v10.15.3-linux-x64.tar
    - ./node-v10.15.3-linux-x64/bin/npm set registry https://registry.npm.taobao.org # 设置淘宝镜像地址
    - ./node-v10.15.3-linux-x64/bin/npm install --progress=false
    - ./node-v10.15.3-linux-x64/bin/npm run build
  artifacts:
    expire_in: 1 week
    paths:
    - dist

deploy_develop:
  #image: alpine
  stage: deploy
  only:
    - develop
  tags:
    - shell
  environment:
    name: ''
    url: ''
  script:
    - "'"
  when: manual

deploy_master:
  #image: alpine
  stage: deploy
  only:
    - master
  tags:
    - shell
  environment:
    name: ''
    url: ''
  script:
    - "'"
  when: manual

```