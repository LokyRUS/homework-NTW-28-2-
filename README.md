# Домашнее задание к занятию "Межсетевое экранирование на основе зон, stateful/stateless packet inspection"

### Цель задания

Данное домашнее задание поможет лучше разобраться в основах межсетевого экранирования, глубже понять особенности его работы, попрактиковаться в настройке МСЭ.

В результате выполнения задания вы:
1) Научитесь выбирать подходящий тип инспекции трафика для эффективного управления трафиком
2) Научитесь ориентироваться в синтаксисе Cisco ASA OS
3) Научитесь настраивать stateful и stateless инспекцию, разделять сеть по уровням безопасности

------

### Инструкция к выполнению домашнего задания

1. Скачайте [Шаблон для домашнего задания](https://u.netology.ru/backend/uploads/lms/content_assets/file/281/%D0%A1%D0%94%D0%95%D0%9B%D0%90%D0%99%D0%A2%D0%95_%D0%9A%D0%9E%D0%9F%D0%98%D0%AE_-_%D0%A8%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD_%D0%B4%D0%BB%D1%8F_%D0%B4%D0%BE%D0%BC%D0%B0%D1%88%D0%BD%D0%B5%D0%B3%D0%BE_%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D1%8F_1.1._%D0%9D%D0%B0%D0%B7%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%BB%D0%B5%D0%BA%D1%86%D0%B8%D0%B8_-_%D0%A4%D0%B0%D0%BC%D0%B8%D0%BB%D0%B8%D1%8F_%D0%98%D0%BC%D1%8F.docx) на своё устройство.
2. Откройте скачанный файл на личном диске в Google.
3. В названии файла введите корректное название лекции и ваши фамилию и имя.
4. Зайдите в «Настройки доступа» и выберите доступ «Просматривать могут все в интернете, у кого есть ссылка». Инструкция «Как предоставить доступ к файлам и папкам на Google Диске» [по ссылке](https://support.google.com/docs/answer/2494822?hl=ru&co=GENIE.Platform%3DDesktop).
5. Скопируйте текст задания в свой документ.
6. Выполните задание, запишите ответы и приложите необходимые скриншоты в свой Google-документ.
7. Для проверки домашнего задания отправьте ссылку на ваш Google-документ в личном кабинете.
8. Любые вопросы по решению задач можно задать в чате учебной группы, в чате поддержки или в разделе «Вопросы по заданию» в личном кабинете.
9. Подробнее о работе с Google-документами и загрузке решения на проверку можно найти в [«Руководстве по работе с материалами для обучения»](https://l.netology.ru/instruktsiya-po-materialami-dlya-obucheniya)

---
### Лабораторная работа "Межсетевое экранирование на основе зон, stateful/stateless packet inspection"

Лабораторная работа заключается в настройке МСЭ компании. ЛВС логически разделена на пользовательскую подсеть, подсеть принтеров и подсеть с сервером DMZ. Для каждой из этих зон расписаны правила доступа в соседние.
Схема сети 
![image](https://user-images.githubusercontent.com/5977962/168447439-471d693b-2748-40a5-82c8-97f502243f19.png)


Настройки оборудования:

![image](https://user-images.githubusercontent.com/5977962/168447553-09a16892-4961-4f9f-ba0a-364fce364c5d.png)

Перенесите топологию в PT, настройте оборудование по предложенным данным. 

-----

### Задание 

От службы безопасности были переданы требования для ограничения сетевого доступа между различными подсетями:

1) Разрешить прохождение только ICMP трафика из INSIDE в OUTSIDE, в обратную сторону разрешить ответный трафик
2) Разрешить прохождение только HTTP-трафика из INSIDE в DMZ, в обратную сторону разрешить ответный трафик
3) Разрешить прохождение только ICMP-трафика из INSIDE в PRINTER, в обратную сторону разрешить ответный трафик
4) Из OUTSIDE разрешить инициировать сессии в DMZ по 80 TCP порту, в INSIDE и PRINTER разрешить только ответные пакеты
5) Из PRINTER запретить инициировать трафик во все остальные зоны
6) Из DMZ разрешить инициировать трафик в OUTSIDE по ICMP, в INSIDE и PRINTER  разрешить только ответные пакеты

Необходимо, используя полученные знания о межсетевом экранировании на основе зон, stateful и stateless инспекции, сконфигурировать эти правила на МСЭ и выполнить проверку средствами Packet tracer.

*Cisco PT имеет баг в части настройки МСЭ: политику оп-умолчанию нельзя редактировать. Поэтому чтобы инспекция icmp нормально работала, удалите политику по умолчанию и создайте новую:
no service-policy global_policy global
policy-map test
 class inspection_default
  inspect http 
  inspect icmp 
service-policy test global

*В качестве ответа приложите вывод команды "sh run" с МСЭ*

------
# Ответ

# Первоначальные настройки
```
ciscoasa#configure terminal 
ciscoasa(config)#interface gigabitEthernet 1/1
ciscoasa(config-if)#nameif INSIDE
ciscoasa(config-if)#ip address 192.168.1.1 255.255.255.0
ciscoasa(config-if)#no shutdown 
ciscoasa(config-if)#ex
ciscoasa(config)#interface gigabitEthernet 1/3
ciscoasa(config-if)#nameif Printer
ciscoasa(config-if)#ip address 192.168.2.1
ciscoasa(config-if)#no shutdown 
ciscoasa(config-if)#ex
ciscoasa(config)#interface gigabitEthernet 1/4
ciscoasa(config-if)#nameif DMZ
ciscoasa(config-if)#ip address 192.168.3.1 255.255.255.0
coasa(config-if)#no shutdown 
ciscoasa(config-if)#ex
ciscoasa(config)#interface gigabitEthernet 1/2
ciscoasa(config-if)#nameif OUTSIDE
ciscoasa(config-if)#ip address 10.10.10.1 255.255.255.0
ciscoasa(config-if)#no shutdown 
```
```
ciscoasa#configure terminal 
ciscoasa(config)#interface gigabitEthernet 1/1
ciscoasa(config-if)#security-level 100
ciscoasa(config-if)#ex
ciscoasa(config)#interface gigabitEthernet 1/3
ciscoasa(config-if)#security-level 70
ciscoasa(config-if)#ex
ciscoasa(config)#interface gigabitEthernet 1/4
ciscoasa(config-if)#security-level 50
ciscoasa(config-if)#ex
ciscoasa(config)#interface gig 1/2
ciscoasa(config-if)#security-level 0
ciscoasa(config-if)#

```
# Чтобы инспекция icmp нормально работала, удаляем политику по умолчанию и создайте новую
```
no service-policy global_policy global 

```

```
policy-map test 
class inspection_default
inspect http 
inspect icmp 
service-policy test global
```
 

# Разрешаем прохождение только ICMP трафика из INSIDE в OUTSIDE, в обратную сторону разрешить ответный трафик`
```
ciscoasa(config)#access-list inspect-inside extended permit ip 192.168.1.0 255.255.255.0 host 10.10.10.2
ciscoasa(config)#class-map inside-outside-inspect-icmp
ciscoasa(config-cmap)#match access-list inspect-inside
ciscoasa(config)#policy-map inside-outside
ciscoasa(config-pmap)#class inside-outside-inspect-icmp
ciscoasa(config-pmap-c)#inspect icmp 
ciscoasa(config-pmap-c)#ex
ciscoasa(config)#service-policy inside-outside interface INSIDE
ciscoasa(config)#

```
```
ciscoasa(config)#access-list 100 permit icmp 10.10.10.0 255.255.255.0 192.168.1.0 255.255.255.0
ciscoasa(config)#access-group 100 in interface OUTSIDE 
ciscoasa(config)#ex
```

# Разрешить прохождение только HTTP-трафика из INSIDE в DMZ, в обратную сторону разрешить ответный трафик` 
```
ciscoasa(config)#access-list inspect-http-DMZ extended permit ip 192.168.1.0 255.255.255.0 host 192.168.3.2
ciscoasa(config)#class-map inside-DMZ-inspect-http
ciscoasa(config-cmap)#match access-list inspect-http-DMZ
ciscoasa(config)#policy-map inside-outside
ciscoasa(config-pmap)#class inside-DMZ-inspect-http 
ciscoasa(config-pmap-c)#inspect http 
ciscoasa(config-pmap-c)#ex
ciscoasa(config)#service-policy inside-outside interface INSIDE
ciscoasa(config)#

```
```
ciscoasa(config)#access-list 101 permit tcp 192.168.3.0 255.255.255.0 192.168.1.0 255.255.255.0
ciscoasa(config)#access-group 101 in interface DMZ 
ciscoasa(config)#ex
```
# Разрешаем прохождение только ICMP-трафика из INSIDE в PRINTER, в обратную сторону разрешить ответный трафик
```

ciscoasa(config)#access-list inspect-icmp-printer extended permit ip 192.168.1.0 255.255.255.0 host 192.168.2.2
ciscoasa(config)#class-map inside-printer-inspect-icmp
ciscoasa(config-cmap)#match access-list inspect-icmp-printer
ciscoasa(config)#policy-map inside-outside
ciscoasa(config-pmap)#class inside-printer-inspect-icmp
ciscoasa(config-pmap-c)#inspect icmp
ciscoasa(config-pmap-c)#ex
ciscoasa(config)#service-policy inside-outside interface INSIDE
ciscoasa(config)#
```
```
ciscoasa(config)#access-list 102 permit icmp 192.168.2.0 255.255.255.0 192.168.1.0 255.255.255.0
ciscoasa(config)#access-group 102 in interface PRINTER 
ciscoasa(config)#ex
```
# Из OUTSIDE разрешить инициировать сессии в DMZ по 80 TCP порту
```
ciscoasa(config)#access-list 103 permit tcp 10.10.10.0 255.255.255.0 host 192.168.3.2 eq 80
ciscoasa(config)#access-group 103 in interface OUTSIDE


``
ciscoasa(config)access-list 104 permit tcp 192.168.2.0 255.255.255.0 10.10.10.0 255.255.255.0
ciscoasa(config)#access-group 104 in interface PRINTER

ciscoasa(config)access-list 105 permit tcp 192.168.1.0 255.255.255.0 10.10.10.0 255.255.255.0
ciscoasa(config)#access-group 105 in interface INSIDE

```
`Из DMZ разрешить инициировать трафик в OUTSIDE по ICMP, в INSIDE и PRINTER разрешить только ответные пакеты`
```
ciscoasa(config)#access-list inspect-inside extended permit ip 192.168.3.0 255.255.255.0 host 10.10.10.2
ciscoasa(config)#class-map outside-icmp-DMZ
ciscoasa(config-cmap)#match access-list inspect-inside
ciscoasa(config)#policy-map inside-outside-DMZ
ciscoasa(config-pmap)#class outside-icmp-DMZ
ciscoasa(config-pmap-c)#inspect icmp 
ciscoasa(config-pmap-c)#ex
ciscoasa(config)#service-policy inside-outside-DMZ interface DMZ
ciscoasa(config)#

```
```
ciscoasa(config)#access-list 105 permit icmp 192.168.3.0 255.255.255.0 10.10.10.0 255.255.255.0
access-group 105 in interface DMZ 
```
 
```
ciscoasa(config)#access-list 106 permit icmp 10.10.10.0 255.255.255.0 192.168.3.0 255.255.255.0
ciscoasa(config)#access-group 106 in interface OUTSIDE 
ciscoasa(config)#ex
```
# ![images1](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/1.PNG)
# ![images2](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/2.PNG)
# ![images3](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/3.PNG)

### Правила приема домашнего задания

В личном кабинете отправлена ссылка на документ (Google Doc) с выполненным заданием. В документе настроены права доступа “Просматривать могут все в Интернете, у кого есть ссылка”.

### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
