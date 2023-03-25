## Part 1. Установка ОС

**Установлен `Ubuntu 20.04 Server LTS` без графического интерфейса.
команда для просмотра `cat /etc/issue`**

![Ubuntu installing](pics/1.png)

---

## Part 2. Создание пользователя

**1. Создаю нового пользователя командой `sudo adducer` username (вместо username мы должны записать имя пользователя который мы создадим, в моём случае "new")**

![adding new user](pics/2.png)

**2. даю права командой `sudo usermod -aG adm user`**

**3. перехожу к пользователю командой `sudo su user`**

**4. проверяю записался ли мой новый пользователь в группу командой `groups`**

**5. и в конце проверка командой `cat /etc/passwd`**
![checking user in admin groups](pics/2,2.png)

---

## Part 3. Настройка сети ОС

**1. `sudo hostname user-1` - меняем имя хоста**

**2. `cat /etc/hostname` - проверяем поменялось ли если да то все четко, если нет то**
![checking hostname](pics/3,1.png)
**3. `sudo vim /etc/hostname` - этой командой вручную меняем**

**4. `sudo vim /etc/hosts` - и в этом файле тоже**

**5. `sudo timedatectl set-timezone Europe/Moscow` - меняем часовой пояс**

**6. Проверяем этими двумя командами `timedatectl` и `cat /etc/timezone`**
![checking time and timezone](pics/3,2.png)

**7. `sudo apt install net-tools` - скачиваем `net-tools`**
![install net-tools](pics/3,3.png)

**8. `ifconfig` - для просмотра нашей сетевой конфигурации**

**9. Сбрасываем свой старый ip адрес командой `sudo dhclient -r enp0s3`**
![reset ip](pics/3,4.png)

**10. И получаем новый командой `sudo dhclient -v enp0s3`**
![getting new ip](pics/3,5.png)

**11. Проверить все это можно командой `ifconfig`**
![checking ip](pics/3,6.png)

**12. Узнаем свой внешний ip address командой `wget -O - -q icanhazip.com`**

**13. Внутренний ip можно узнать командой `route -n`**
![checking inner ip](pics/3,7.png)

**`ls /etc/netplan` - проверяем директорию**
![checking directory](pics/3,8.png)

**14. `sudo vim /etc/netplan/00-installer-config.yaml` - В этом файле у нас уже имеется шаблон для нашей сети**
**enp0s3. Наша задача - отключить получение адресов от DHCP и присвоить свой статический адрес. Для этого**

**15. `"dhcp4: true"` меняем на `"dhcp4: false"` и ниже добавляем строки с выбранным нами IP, шлюзом и** **DNS-серверами**

**16. Сначала смотрим наш шлюз командой `route -n`, в этом диапазоне адресов задаём IP, а в качестве DNS выбираем гугловские `1.1.1.1 и 8.8.8.8`, как и рекомендуется в задании**
![changing dhcp4 and dns](pics/3,9.png)

**17. `sudo netplan apply` - для применения конфигураций**

**18. `sudo netplan try` - для применения изменений**

**19. `ifconfig` - проверяем получили ли мы статистический айпишники**
![checking static ip](pics/3,10.png)

**20. `ping ya.ru` - и убеждаемся что все работает**
![checking ping](pics/3,11.png)

---

## Part 4. Обновление ОС

**1. `sudo apt update` - обновляю Ubuntu**
![updating ubuntu](pics/4.1.png)

**2. `sudo apt dist-upgrade` - обновляю версию пакетов**
![updating dist-upgrade](pics/4.2.png)

**3. И `sudo apt update` проверю на обновление**
![checking updates](pics/4.3.png)

---

## Part 5. Использование команды sudo

**1. Команда sudo - позволяет строго определенным пользователям выполнять указанные программы с административными привилегиями без ввода пароля суперпользователя root**

**2. `sudo touch /etc/sudoers.d/new` - создаю файл для нашего пользователя**

**3. `sudo vim /etc/sudoers.d/new` - открываю в вим и записываю такую строку `new ALL=(ALL:ALL) ALL`**
![new ALL=(ALL:ALL) ALL](pics/5.1.png)

**4. И перехожу к нему командой su new**
![changing user for new](pics/5.2.png)

**5. И проверим командой `sudo apt update` чтобы убедится что все работает**
**6. `sudo vim /etc/hostname` - поменяю на new**

**7. Перезапускаем виртуальную машину и видим внесенные изменения.**
 ![reboot system](pics/5.3.png)
---

## Part 6. Установка и настройка службы времени

**1. `sudo apt install -y ntp` - устанавливаем**

![installing ntp](pics/6.1.png)

**2. `dpkg -l | grep "ntp"` - проверяем все ли установилось успешно**

![checking ntp installed](pics/6.2.png)

**3. `sudo apt update` - снова обновляем чтобы проверить все ли нормально**

![updating machine](pics/6.3.png)

**4. `ntpq -p` - Проверим, что `ntp` подключён к серверам времен**
![connecting to time server](pics/6.4.png)

**5. `sudo systemctl stop ntp` - остановим нашу команду**
**6. `sudo ntpd -gq` - и принудительно синхронизируем все это**
![synchron ntp&d](pics/6.5.png)

**7. `sudo systemctl start ntp` - снова все запускаем**
**8. `sudo systemctl status ntp` - и убеждаемся что все нормально**
![restart and checking](pics/6.6.png)

**9. `timedatectl show` - и в конце проверяем еще раз**
![show time](pics/6.7.png)

---

## Part 7. Установка и использование текстовых редакторов

###JOE
**1. `sudo apt install Joe` - скачиваю `Joe` текстовый редактор.**
**и есть у нас уже установленные два редактора это vim и nano**
![instaling Joe](pics/7.1.png)
**2. `sudo Joe test_JOE.txt` - создаю .txt файл и открываю его для записи**
![ulling file](pics/7.4.png)
**Записал все что мне нужно**
**3. `control+k` а потом нажимаю `Q` и он спросит сохранить изменения.**
**я нажимаю кнопку y и он все сохранит и выйдет**
**4. Снова открывю файл и меняю содержимое на `21 School 21`, но уже без сохраниния**
![change file without saving](pics/7.41.png)
**Проверим, что файл, остался без изменений**
![checking file](pics/7.42/.png)
**5. Для поиска я нажимаю `control+k` и потом `f`**
![find symb](pics/8.5/.png)
**6. И пишу исомое слово и `enter`**
![find symb](pics/7,joe,zam.png)
**7. И нам показывают еще несколько команд, из них нам понадобиться только `R`**
**8. И слово на которое мы заменим в моем случае это `daniella`**
![last vers](pics/7Joe.png)
**9. Сохраняем и выходим**

###VIM
**1. `sudo vim test_VIM.txt` - создаю `.txt` файл и открываю его для записи**
**2. Нажимаю `i` чтобы у меня появился возможность писать**
**3. Пишу свой юзернейм нажимаю `escape`**
![1](pics/7.3.png)
**4. `:wq` и добавляю вот таким образом команду чтобы оно сохранилось и вышло**
![2](pics/7.32.png)
**5. `sudo vim test_VIM.txt` - открываю файл заново для записи**
**6. Нажимаю кнопку `i` чтобы у меня появилось возможность писать**
**7. Меняю `user3` на `21 School 21`**
![3](pics/7.31.png)
**8. Нажимаю `escape`**
**9. `:q!` - и ставлю вот такую команду для того чтобы он вышел не сохраняя изменения**
**10. Проверяем командой `sudo cat test_VIM.txt`**
![4](pics/7.32.png)
**11. Для поиска и замены нам понадобится открыть файл**
**12. Нажать `escape` и ввести `:s/искомое слово/слово на которую мы заменим/g` - этот флаг отвечает за то чтобы во всем текстовом файле искомое слово заменили**
![5](pics/7,vim.png)
**13. Сохраняем и выходим**
![6](pics/7Vim.png)

###NANO
**1. `sudo nano test_NANO.txt` - создаю `.txt` файл и открываю его для записи**
![1](pics/7.2.png)
**2. Записываю. Нажимаю команду `control+x`**
**3. И меня спрашивают сохранить изменения? Я пишу `y` и нажимаю кнопку `enter`**
**И он сохраняет и выходит.**
**4.Снова открывем файл и переписываем его на 21 Shooll 21**
![2](pics/7.21.png)
**а не сохранения изменений аналогично но вместо `y` нужно нажать кнопку `n` и `enter`**
![3](pics/7.22.png)
**5. А для того чтобы он произвел поиск и замену текста мы нажимаем `control+\`**
**6. И внизу (как на фото) появится поле для слово которое мы ищем. В моем случае это `ll`**
**7. И нажимаю `enter`**
![4](pics/7,nano_zam.png)
**8. И так у нас появился еще что-то. В этот раз в то же место мы записываем слово на которую мы хотим поменять**
**9. И он нас спрашивает заменить? Мы говорим да и жмем кнопку `y`**
![5](pics/7,nano.png)
**10. И сохраняем и выходим**

**И смотрим конечный результат всех файлов**
![6](pics/7Nano.png)

---

## Part 8. Установка и базовая настройка сервиса SSHD

**1. Установить службу SSHd.**
**Установить SSH-сервер в системе, команда: `sudo apt-get install openssh-server`**
![install openssh-server](pics/8.1.png)

**2. Добавить автостарт службы при загрузке системы.**
**Для включения автостарта службы воспользуемся командой: `sudo systemctl enable ssh`**
![active autostart](pics/8.2.png)

**3.Перенастроить службу SSHd на порт 2022.**
**Для этого открыл файл конфигурации с помощью команды: vim /etc/ssh/sshd_config**
**Нашёл строку, определяющую порт: Port 22**
**Поменял его на 2022 и раскомментировал строку.**

![Port 22->2022](pics/8.3.png)

> ps (от англ. process status) — программа в Unix-подобных операционных системах, выводящая отчёт о работающих процессах. Ключ -А (-е) позволяет вывести все процессы.

**4. Сохранил и перезапустил командой `sudo service sshd restart` и проверю командой `sudo service sshd status`**

![restart system](pics/8.4.png)

**5.Проверка стату фаербола командой `sudo ufw status` оно было `inactive` и я исправила это командой `sudo ufw enable`**
![checking fireboll](pics/8.6.png)
**6. Открываю порт командой `sudo ufw allow 2022` и еще раз проверил статус на всякий случай**
![checking status](pics/8.7.png)
**9. Перезапуск системы командой  `sudo reboot`**
**10. Скачивание утилиты netstat `sudo apt netstall net-tools`**
**11. Вывод командв  `netstan -tan`**
![netstan -tan](pics/8.9.png)

>netstat (network statistics) — утилита командной строки, выводящая на дисплей состояние TCP-соединений (как входящих, так и исходящих), таблицы маршрутизации, число сетевых интерфейсов и сетевую статистику по протоколам. Основное назначение утилиты — поиск сетевых проблем и определение производительности сети.

#### Используемые ключи:
* t (--tcp) - показывать только TCP порты.
* a (--all) - показывать состояние всех сокетов.
* -n (--numeric) - показывать сетевые адреса как числа (например 127.0.0.53:53 вместо localhost:domain)

#### Значения столбцов 
* Proto - протокол, используемый сокетом. Так как была использована опция [-t|--tcp], в выводе пристутвуют только TCP-сокеты.
* Recv-Q - счётчик байт, не скопированных программой пользователя из этого сокета.
* Send-Q - счётчик байтов, не подтверждённых удалённым узлом.
* Local Address - адрес и номер порта локального конца сокета. Если указана опция [-n|--numeric], вывод в формате [адрес сокета:номер порта], иначе - [каноническое имя узла:соответствующее имя службы]. В интересующей нас строчке 0.0.0.0 - адрес локального конца сокета, 2022 - номер порта, который мы поменяли с 22 на 2022. Адрес 0.0.0.0 означает, что удаленный конец сокета будет доступен всем локальным ip-адресам.
* Foreign Address - адрес и номер порта удалённого конца сокета.
* State - состояние сокета. Состояние LISTEN означает, что сокет ожидает входящих подключений.
  
---

## Part 9. Установка и использование утилит `top, htop`

**1. Установка `sudo install top/htop` информация о состоянии системы из выовода утилиты top **

![top](pics/9.1.png)

* uptime - 8min
* количество авторизованных пользователей - 1
* общую загрузку системы(load avarage) - 0.01, 0.10, 0.08
* общее количество процессов(Tasks) - 97 total, 1 running, 96 sleeping, 0 stopped, 0 zombie
* загрузку cpu - 0,0 us, 0,0 su, 0,0 ni, 100,0 id, 0,0 wa, 0,0 hi, 0,0 si, 0,0 st
* загрузку памяти (Mib Mem) - 1983,4 total, 1065,8 free, 163,0 used, 754.7 buff/cache
* pid процесса занимающего больше всего памяти - 2076
* pid процесса, занимающего больше всего процессорного времени - 1
![](screenshots/part9/test2.png)

**2. отсортировка по `PID`**

![top](pics/9.3.png)

**3. отсортировка по `PERCENT_CPU`**
![cpu%](pics/9.2.png)

**4. отсортировка по `PERCENT_MEM`**
![mem](pics/9.5.png)

**5. отсортировка по `TIME`**
![time](pics/9.6.png)

**6. отфлиртованный процесс `sshd`**
![time](pics/9.7.png)

**7. `syslog`**
![syslog](pics/syslog.png)

**8. с добавление `hostname, clocl, uptime`**
![time](pics/time.png)

---

## Part 10. Использование утилиты `fdisk`

**1. `/dev/sda 10GIB, 10737418240 bytes, 20971520 sector`**

![fdisk](pics/10.1.png)

* Имя /dev/sda
* Размер 10 Гб
* Колличество секторов 20971520
* Размер swap 1.5 Gb 
> Узнать размер swap можно с помощью команды `free -h`
![time](pics/swap.png)

---

## Part 11. Использование утилиты `df`

**1. Для корневого раздела c командой `df /`**

![df](pics/11.1.png)

* размер раздела - 8408452
* размер занятого пространства - 4249936
* размер свободного пространства - 3709800
* процент использования - 54%
* Определить и написать в отчёт единицу измерения в выводе: Килобайт

**2. Для корневого раздела с командой `df -Th /`**

![df -Th](pics/11.2.png)

* размер раздела - 8,1G**
* размер занятого пространства - 4,1G
* размер свободного пространства - 3,G
* процент использования - 54%
* тип файловой системы для раздела : ext4

---

## Part 12. Использование утилиты `du`

**1. `du -B1 -d0 /home /var /var/log`**

![/home /var /var/log](pics/12.1.png)

`du -h -d0 /home /var /var/log`

![/home /var /var/log](pics/12.2.png)

**2. Размер всего содержимого в /var/log.**

![/var/log](pics/12.3.png)
---

## Part 13. Установка и использование утилиты ncdu

**1. Установка с командой `sud apt install ncdu`**
![install ncdu](pics/13.1.png)

**2. `/home`**
![/](pics/13.2.png)

**3. `/var`**
![/](pics/13.3.png)

**4. `/var/log`**
![/](pics/13.4.png)

---

## Part 14. Работа с системными журналами

**`/var/log/dmesg` содержит информацию о драйверах устройств**

![/](pics/14.1.png)

**`/var/log/auth.log` информация об авторизации пользователей, включая удачные и неудачные попытки входа в систему, а также задействованные механизмы аутентификации.**

![/](pics/14.2.png)

**`/var/log/syslog` одержит глобальный системный журнал, в котором пишутся сообщения от ядра Linux, различных служб, сетевых интерфейсов и т.д. с момента запуска системы.**

![/](pics/14.3.png)

**Информация об авторизации: `sudo cat /var/log/auth.log | grep login`**
![/](pics/14.4.png)
---

## Part 15. Использование планировщика заданий CRON

**1. для создания задачи я открыл файл планировщик командой `contrab -e` и вписала следующую строку */2 * * * * Далее `contrab -e` позволяет посмотреть этот файл**
![install and contrab -e/l](pics/15.1.png)

**2. Записи в /var/log/syslog:**
![/](pics/15.3.png)

**3. Удаление конфигурационного файла: `contrab -r` Попытка вывести список задач после удаления**
![/](pics/15.l.png)

---
