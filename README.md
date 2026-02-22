# Домашнее задание к занятию «GitLab»

### Задание 1

**Что нужно сделать:**

1. Разверните GitLab локально, используя Vagrantfile и инструкцию, описанные в [этом репозитории](https://github.com/netology-code/sdvps-materials/tree/main/gitlab).   
2. Создайте новый проект и пустой репозиторий в нём.
3. Зарегистрируйте gitlab-runner для этого проекта и запустите его в режиме Docker. Раннер можно регистрировать и запускать на той же виртуальной машине, на которой запущен GitLab.

В качестве ответа в репозиторий шаблона с решением добавьте скриншоты с настройками раннера в проекте.

### Решение

<img width="1514" height="448" alt="image" src="https://github.com/user-attachments/assets/1cdcaade-f44e-4074-aa49-0c95d42138b8" />


---

### Задание 2

**Что нужно сделать:**

1. Запушьте [репозиторий](https://github.com/netology-code/sdvps-materials/tree/main/gitlab) на GitLab, изменив origin. Это изучалось на занятии по Git.
2. Создайте .gitlab-ci.yml, описав в нём все необходимые, на ваш взгляд, этапы.

В качестве ответа в шаблон с решением добавьте: 
   
 * файл gitlab-ci.yml для своего проекта или вставьте код в соответствующее поле в шаблоне; 
 * скриншоты с успешно собранными сборками.

 ### Решениe
 <img width="1096" height="279" alt="image" src="https://github.com/user-attachments/assets/ccf43f40-55b3-4de3-9db8-b6a613ccd147" />
 <img width="652" height="436" alt="image" src="https://github.com/user-attachments/assets/6d752910-d6b5-4aac-af57-6f04dc6dd168" />
 <img width="1225" height="654" alt="image" src="https://github.com/user-attachments/assets/617bb2cf-63b4-4afe-8133-6ca136cbef39" />
 <img width="1219" height="650" alt="image" src="https://github.com/user-attachments/assets/895c22da-bd07-4731-b6c5-d770d215bff4" />


### .gitlab-ci.yml

```
stages:
  - test
  - build

variables:
  DOCKER_DRIVER: overlay2

# Запуск unit-тестов Go
test:
  stage: test
  image: golang:1.17
  tags:
    - docker
  script:
    - go test ./...

# Сборка Docker-образа
build:
  stage: build
  image: docker:20.10.16
  services:
    - name: docker:20.10.16-dind
      command: ["--host=tcp://0.0.0.0:2375", "--tls=false"]
  variables:
    DOCKER_HOST: tcp://docker:2375
    DOCKER_TLS_CERTDIR: ""
  tags:
    - docker
  script:
    - docker info
    - docker build -t gitlab-ci-example .
```
