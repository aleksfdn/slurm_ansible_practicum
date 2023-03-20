##############################
# Практикум по SLURM ANSIBLE #
##############################

# Шаги выполнения заданий

# Генерация ключей для подключения к ВМ
1. Переходим в папку  ansible/ssh-keys и запускаем генерацию ssh-ключей:
```sh
cd ssh-keys
ssh-keygen -f ansible
```
# Создание ВМ с помощью vagrant
2. В корневой директории проекта запускаем инициализацию виртуальных машин:
```sh
vagrant up
```
# Подключение к ВМ
3. После инициализации виртуальных машин подключаемся к машине с установленным Ansible:
```sh
vagrant ssh controlnode
```
# Запуск выполнения плейбука ansible (заранее подготовленного)
4. На controlnode переходим в папку /home/vagrant/ansible и запускаем плейбук:
```sh
ansible-playbook playbook.yml
```
# При выполнении плейбука будет установлено согласно настроенным ролям:
 ```sh
  - install nginx
  - install postgresql
  - customization os (install packages, + ruby 2.6)
  - customization nginx
  - install and customization xpaste
```
# Проверка результата выполнения задания
5. После успешного завершения работы плейбука открываем веб-браузер и переходим по ссылке:
```sh
http://192.168.60.105
```