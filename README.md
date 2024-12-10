# Домашнее задание к занятию "Сбор конфигураций, анализ конфигурационных изменений"
# Исполнитель: Смирнов К.Е.

### Цель задания

Данное домашнее задание позволит вам разобраться в подходах к резервному копированию конфигурации АСО.

В результате выполнения задания вы научитесь:  
1. Устанавливать и выполнять базовые настойки ПО сбора конфигураций Rancid
2. Снимать в автоматическом режиме по расписанию копии конфигураций АСО и хранить их на удалённом сервере
3. Выполнять расширенные настройки ПО Rancid

------

### Лабораторная работа "Бэкап и восстановление конфигурации АСО"

![image](https://user-images.githubusercontent.com/5977962/215810640-195417b6-0a14-4035-a69c-7e84d7b4264d.png)



### Инструкция к выполнению

1. Установите ПО Rancid на виртуальной машине, следуя шагам на слайдах
2. Проверьте, создался ли пользователь rancid. Если нет, создайте его, установите пароль и дайте права к папкам из инструкции
3. Создайте запись в hosts для тестового маршрутизатора. Назовите его netology-router
4. Проверьте доступ по ssh к виртуальному маршрутизатору
5. Выполните базовые настройки Rancid

### Инструменты/ дополнительные материалы, которые пригодятся для выполнения задания

Доступ к виртуальному маршрутизатору Cisco CSR по ssh:   
IP: 45.134.127.23   
Логин: netology_student    
Пароль: iamanetworkengineer!23   


---

### Задание 1 

1. Вместо команды show running-config настройте выполнение команды sh startup-config.
2. Все остальные команды сбора статистики и логов закомментируйте. Должна остаться только команда сбора конфигурации.
3. Запустите процесс сбора данных rancid.
4. Убедитесь, что диагностические команды выполнены и сохранены в файл.

*В качестве ответа приложите файл, созданный rancid и файл конфигурации, в который вносились изменения по заданию*

# Ответ

### Установка RANCID

- ! переходим под суперпользователя и усстанавливаем RANCID 
```
su -
```
- Установка 
# ![images1](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/images/1.PNG)

- Проверяем пользователя и меняем ему креды

# ![images2](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/images/2.PNG)

- Бэкапим дефолтную конфигурацию 
```

sudo cp /etc/rancid/rancid.conf /etc/rancid/rancid.conf.ORIGINAL
```
- Настройка групп опрашиваемых устройств
```
sudo nano /etc/rancid/rancid.conf
```
Разкоменчиваем строку в болоке как на скрине и происыаем
```
LIST_OF_GROUPS="netology"
```
# ![images3](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/images/3.PNG)

- Создаем репозиторий CVS
```
sudo su -c /var/lib/rancid/bin/rancid-cvs -s /bin/bash -l rancid
```
# ![images4](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/images/4.PNG)

- После выполнения скрипта нужно проверить, что в директории /var/lib/rancid создались папки с именами групп устройств, а в каждой папке есть база устройств router.db:
```
find /var/lib/rancid -type f -name router.db
```
```
find /var/lib/rancid/netology/router.db
```
- Вносим запись в файл /etc/hosts
```
nano /etc/hosts
```
# ![images5](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/images/5.PNG)

- Проверяем запись

# ![images6](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/images/6.PNG)
 
- Проверяем ssh подключение к учебному роутеру 

# ![images7](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/images/7.PNG)

- Настройка списка устройств для сбора конфига

!! Переходим под пользователя rancid 
```
su rancid
```
Вносим запись
```
nano /var/lib/rancid/netology/router.db
```
# ![images8](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/images/8.PNG)

- Настройка аутентификации

```
nano /var/lib/rancid/.cloginrc
```
# ![images9](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/images/9.PNG)

### !!! Важно
Файл .cloginrc должен быть доступен для чтения
только пользователю rancid

```
 cd /var/lib/rancid/
```
``` 
chmod 600 .cloginrc
```
# ![images10](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/images/10.PNG)

!! Можно добавить пользователя rancid в /etc/sudoers, если команда не отработает 
 
## Запуск rancid для сбора конфигов

```
sudo su -c /var/lib/rancid/bin/rancid-run -s /bin/bash -l rancid
```


Файл с конфигурациями устройств  
```
/var/lib/rancid/netology/configs
```
Файлик с выпоняемыми командами рансидом 
```
cd /etc/rancid/rancid.types.base

```

Файлик с изменениями конфигов, хронтся в домашей папке rancid
```
rancid@vbox:~/CVS/netology/configs$
```

### Задание 2 

1. Удалите созданный rancid файл конфигураций тестового роутера.
2. Настройте cron на запуск rancid раз в 5 минут.
3. Подождите 10 минут.
4. Убедитесь, что диагностические команды выполнены. И сохранены в файл.

*В качестве ответа приложите настройку cron*

------
Задание №2 

# Ответ 

```
crontab -e
```

```
*/5 * * * * /usr/bin/rancid-run

```

# ![images11](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/images/11.PNG)
# ![images12](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/images/12.PNG)

### Правила приема домашнего задания

В личном кабинете отправлены:

- ссылка на документ (Google Doc) с выполненным заданием. В документе настроены права доступа “Просматривать могут все в Интернете, у кого есть ссылка”;
- файл в формате .png или .jpg.


### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.

