# Домашнее задание к занятию «Введение в Terraform»

# Артюшин Андрей

### Чек-лист готовности к домашнему заданию

1. Скачайте и установите **Terraform** версии >=1.12.0 . Приложите скриншот вывода команды ```terraform --version```.
```Bash
andrey@andrey-VirtualBox:~$
andrey@andrey-VirtualBox:~$ terraform --version
Terraform v1.12.0
on linux_amd64

Your version of Terraform is out of date! The latest version
is 1.12.2. You can update by downloading from https://www.terraform.io/downloads.html
andrey@andrey-VirtualBox:~$
```
2. Скачайте на свой ПК этот git-репозиторий. Исходный код для выполнения задания расположен в директории **01/src**.
3. Убедитесь, что в вашей ОС установлен docker.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. Репозиторий с ссылкой на зеркало для установки и настройки Terraform: [ссылка](https://github.com/netology-code/devops-materials).
2. Установка docker: [ссылка](https://docs.docker.com/engine/install/ubuntu/). 
------
### Внимание!! Обязательно предоставляем на проверку получившийся код в виде ссылки на ваш github-репозиторий!
------

### Задание 1

1. Перейдите в каталог [**src**](https://github.com/netology-code/ter-homeworks/tree/main/01/src). Скачайте все необходимые зависимости, использованные в проекте.

Ответ:
```Bash
andrey@andrey-VirtualBox:~/ter-homeworks/01/src$
andrey@andrey-VirtualBox:~/ter-homeworks/01/src$
andrey@andrey-VirtualBox:~/ter-homeworks/01/src$ terraform init

Initializing the backend...

Initializing provider plugins...
- Reusing previous version of kreuzwerker/docker from the dependency lock file
- Reusing previous version of hashicorp/random from the dependency lock file
- Using previously-installed kreuzwerker/docker v3.0.2
- Using previously-installed hashicorp/random v3.5.1

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
andrey@andrey-VirtualBox:~/ter-homeworks/01/src$
```

2. Изучите файл **.gitignore**. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд)

Ответ:
Сохранить личную, секретную информацию согласно .gitignore допустимо в файле **personal.auto.tfvars**

3. Выполните код проекта. Найдите в state-файле секретное содержимое созданного ресурса **random_password**, пришлите в качестве ответа конкретный ключ и его значение.

Ответ:
В данном содержимом state файла нет секретного содержимого, так как мы ни каких токенов и паролей не передаем для развертывания.

**terraform.tfstate**
```Bash
{
  "version": 4,
  "terraform_version": "1.12.0",
  "serial": 93,
  "lineage": "1cf12d0a-38e6-05b3-9d5b-cb2235a8d53a",
  "outputs": {},
  "resources": [
    {
      "mode": "managed",
      "type": "random_password",
      "name": "random_string",
      "provider": "provider[\"registry.terraform.io/hashicorp/random\"]",
      "instances": [
        {
          "schema_version": 3,
          "attributes": {
            "bcrypt_hash": "$2a$10$N6NEh5miZWJBbYYslidHauFw6fLq1ntaKLoXaJWOAngbzVdoVoMqS",
            "id": "none",
            "keepers": null,
            "length": 16,
            "lower": true,
            "min_lower": 1,
            "min_numeric": 1,
            "min_special": 0,
            "min_upper": 1,
            "number": true,
            "numeric": true,
            "override_special": null,
            "result": "fqzD4tXMtX3wmTZD",
            "special": false,
            "upper": true
          },
          "sensitive_attributes": []
        }
      ]
    }
  ],
  "check_results": null
}
```

4. Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла **main.tf**.
Выполните команду ```terraform validate```. Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их.

Ответ:
Недопустимое имя ресурса - имя должно начинаться с буквы или символа подчеркивания и может содержать только буквы, цифры, символы подчеркивания и тире;
Необъявленная переменная окружения в имени контейнера - в качестве исправления указал имя контейнера "terraform_nginx"

5. Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды ```docker ps```.

Ответ:
```Bash
andrey@andrey-VirtualBox:~/ter-homeworks/01/src$
andrey@andrey-VirtualBox:~/ter-homeworks/01/src$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                  NAMES
d5c000f0ff3c   021283c8eb95   "/docker-entrypoint.…"   19 seconds ago   Up 12 seconds   0.0.0.0:8000->80/tcp   terraform_nginx
andrey@andrey-VirtualBox:~/ter-homeworks/01/src$
```

6. Замените имя docker-контейнера в блоке кода на ```hello_world```. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду ```terraform apply -auto-approve```.
Объясните своими словами, в чём может быть опасность применения ключа  ```-auto-approve```. Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды ```docker ps```.

Ответ:
При применение ключа перестает выводить и спрашивать сообщение о применение изменения, т.е в автоматическом режиме применяет конфигурацию. В таком случае если в коде закралась ошибка, то исправить её на данном этапе не предоставится возможным. 
```Bash
andrey@andrey-VirtualBox:~/ter-homeworks/01/src$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                  NAMES
1c72d9d251ba   021283c8eb95   "/docker-entrypoint.…"   13 seconds ago   Up 12 seconds   0.0.0.0:8000->80/tcp   hello_world
andrey@andrey-VirtualBox:~/ter-homeworks/01/src$
```

7. Уничтожьте созданные ресурсы с помощью **terraform**. Убедитесь, что все ресурсы удалены. Приложите содержимое файла **terraform.tfstate**. 

Ответ:
```Bash
{
  "version": 4,
  "terraform_version": "1.12.0",
  "serial": 35,
  "lineage": "1cf12d0a-38e6-05b3-9d5b-cb2235a8d53a",
  "outputs": {},
  "resources": [],
  "check_results": null
}
```

8. Объясните, почему при этом не был удалён docker-образ **nginx:latest**. Ответ **ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ**, а затем **ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ** строчкой из документации [**terraform провайдера docker**](https://library.tf/providers/kreuzwerker/docker/latest).  (ищите в классификаторе resource docker_image )

Ответ:
keep_locally - Если true, то образ Docker не будет удален при операции уничтожения. Если это ложь, он удалит изображение из локального хранилища докера при операции уничтожения. У нас в конфигурации как раз данный параметр "true", поэтому данный образ остался.

keep_locally (Boolean) If true, then the Docker image won't be deleted on destroy operation. If this is false, it will delete the image from the docker local storage on destroy operation.
