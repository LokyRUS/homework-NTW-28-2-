# Домашнее задание к занятию "Построение сетей дата-центра"

### Цель задания

В результате выполнения этого задания вы научитесь собирать простую underlay сеть ДЦ на основе протокола BGP.

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

### Лабораторная работа "Построение сетей Дата Центра"

### Задание 1

![image](https://user-images.githubusercontent.com/77394491/175958359-1d8ca2c3-3cbe-4c51-a0dc-2c64fbb1a4d6.png)


Для указанной топологии необходимо настроить маршрутизацию eBGP для underlay сети DC.  

Номера AS используйте из топологии. Адресацию стыковочных линков и импортируемых сетей выберите на свое усмотрение.  
!!! Важно, чтобы в сети были доступны по BGP 2 разных маршрута до каждой клиентской сети через разные spine.  
Количество подключенных конечных хостов можно не соблюдать, достаточно двух.


*Отправьте полный список конфигураций: каждого leaf и spine маршрутизаторов.*

 # Ответ
## Топология
![images](https://github.com/LokyRUS/homework-NTW-28-2-/blob/nevidimka/1.PNG)

[Скачать файл.pkt]()

## Настройка Spine5
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Spine5(config)#router bgp 65000
Spine5(config-router)#network 10.10.0.0 mask 255.255.255.252
Spine5(config-router)#network 10.30.0.0 mask 255.255.255.252
Spine5(config-router)#network 10.50.0.0 mask 255.255.255.252
Spine5(config-router)#network 10.70.0.0 mask 255.255.255.252
Spine5(config-router)#neighbor 10.10.0.2 remote-as 65001
Spine5(config-router)#neighbor 10.30.0.2 remote-as 65002
Spine5(config-router)#neighbor 10.50.0.2 remote-as 65003
Spine5(config-router)#neighbor 10.70.0.2 remote-as 65004
```
## Настройка Spine6
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Spine6(config)#router bgp 65000
Spine6(config-router)#network 10.20.0.0 mask 255.255.255.252
Spine6(config-router)#network 10.40.0.0 mask 255.255.255.252
Spine6(config-router)#network 10.60.0.0 mask 255.255.255.252
Spine6(config-router)#network 10.80.0.0 mask 255.255.255.252
Spine6(config-router)#neighbor 10.20.0.2 remote-as 65001
Spine6(config-router)#neighbor 10.40.0.2 remote-as 65002
Spine6(config-router)#neighbor 10.60.0.2 remote-as 65003
Spine6(config-router)#neighbor 10.80.0.2 remote-as 65004
```
## Настройка Leaf1
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Leaf1(config)#router bgp 65001
Leaf1(config-router)#network 192.168.10.0 mask 255.255.255.0
Leaf1(config-router)#network 10.10.0.0 mask 255.255.255.252
Leaf1(config-router)#network 10.20.0.0 mask 255.255.255.252
Leaf1(config-router)#neighbor 10.10.0.1 remote-as 65000
Leaf1(config-router)#neighbor 10.20.0.1 remote-as 65000
```
## Настройка Leaf2
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Leaf2(config)#router bgp 65002
Leaf2(config-router)#network 10.30.0.0 mask 255.255.255.252
Leaf2(config-router)#network 10.40.0.0 mask 255.255.255.252
Leaf2(config-router)#neighbor 10.30.0.1 remote-as 65000
Leaf2(config-router)#neighbor 10.40.0.1 remote-as 65000
```
## Настройка Leaf3
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Leaf3(config)#router bgp 65003
Leaf3(config-router)#network 10.50.0.0 mask 255.255.255.252
Leaf3(config-router)#network 10.60.0.0 mask 255.255.255.252
Leaf3(config-router)#neighbor 10.50.0.1 remote-as 65000
Leaf3(config-router)#neighbor 10.60.0.1 remote-as 65000
```
## Настройка Leaf4
```

Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Leaf4(config)#router bgp 65004
Leaf4(config-router)#network 10.70.0.0 mask 255.255.255.252
Leaf4(config-router)#network 10.80.0.0 mask 255.255.255.252
Leaf4(config-router)#neighbor 10.70.0.1 remote-as 65000
Leaf4(config-router)#neighbor 10.80.0.1 remote-as 65000
```

## Дополнительное задание (со звездочкой*)

Эти задания дополнительные (не обязательные к выполнению) и никак не повлияют на получение вами зачета по этому домашнему заданию. Вы можете их выполнить, если хотите глубже и/или шире разобраться в материале.

### Задание 2*

Настройте ECMP для балансировки трафика в Underlay сети по линкам до разных spine

*Отправьте полный список конфигураций, где для соответствующей AF включена балансировка.*

### Правила приема домашнего задания

В личном кабинете отправлены:

- ссылка на документ (Google Doc) с выполненным заданием. В документе настроены права доступа “Просматривать могут все в Интернете, у кого есть ссылка”;
- файл в формате .png или .jpg.

### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
