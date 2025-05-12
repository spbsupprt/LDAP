# LDAP

Что нужно сделать?

- Установить FreeIPA

- Написать Ansible-playbook для конфигурации клиента

---

Дано:

![image](https://github.com/user-attachments/assets/39ad0c96-ed52-46fd-b5d0-965421346194)

Применим плейбук  https://github.com/spbsupprt/LDAP/blob/main/ipa.yml


![image](https://github.com/user-attachments/assets/e0092e64-1231-4575-a639-485ba7a8d719)


Проведем проверки:


### 1. web-интерфейс FreeIPA:

![image](https://github.com/user-attachments/assets/88731e3e-75c4-4f1a-b3be-65d7eca149dd)


### 2. Проверка подключения к FreeIPA домену

```
# Проверка членства в домене
$ ipa-client-status

Directory Service: LDAP
Server: ipa.otus.lan
Base DN: dc=otus,dc=lan

Connected to server: ipa.otus.lan
Domain: otus.lan
KDC: ipa.otus.lan
Realm: OTUS.LAN
```
### 3. Проверка аутентификации через Kerberos

```
# Получение Kerberos-билета для администратора
$ kinit admin
Password for admin@OTUS.LAN: ********

# Проверка наличия билета
$ klist

Ticket cache: KEYRING:persistent:0:0
Default principal: admin@OTUS.LAN

Valid starting       Expires              Service principal
03/15/2024 10:00:00  03/16/2024 10:00:00  krbtgt/OTUS.LAN@OTUS.LAN
```


### 4. Проверка автоматического создания домашних каталогов


```
# Вход под пользователем из FreeIPA (должен автоматически создать домашний каталог)
$ ssh admin@client1.otus.lan
Creating home directory for admin...
Last login: Fri Mar 15 10:05:12 2024 from 192.168.57.1

# Проверка существования домашнего каталога
$ ls -ld /home/admin
drwx------. 2 admin admins 4096 Mar 15 10:05 /home/admin
```

