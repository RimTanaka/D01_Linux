# Результаты выполнения заданий

## 1. Проверка версии Ubuntu `cat /etc/issue.`
![1 version ubuntu](./img/1 version ubuntu.png)

## 2. Создание нового пользователя и добавление в группу adm
### Создание нового пользователя `sudo adduser newuser`
### Добавление нового пользователя в группу adm `sudo usermod -aG adm newuser`
### Проверка, что новый пользователь добавлен в систему `cat /etc/passwd`
![2 new user adm](./img/2 new user adm.png)
### Создание домашнего католога для пользователя
```sudo mkdir /home/rim
sudo chown rim:rim /home/rim
```
### Создание стандартных конфиг файлов в дом катологе пользователя
```sudo cp -r /etc/skel/. /home/rim/
sudo chown -R rim:rim /home/rim/
```
### Устанавливаем пароль для нового пользователя `sudo passwd rim`

## 3. Настройка статических сетевых параметров
### Задаем название машины вида user-1 `sudo hostnamectl set-hostname user-1`
### Установка временной зоны `sudo timedatectl set-timezone Europe/Moscow`
### Вывод сетевых интерфейсов `ip link show`

![3 hostname and timedatectl](./img/3 hostname and timedatectl.png)

### Объяснение наличию интерфейса lo
Интерфейс `lo` (loopback) используется для сетевого взаимодействия внутри самой машины. Он позволяет программам общаться с сетью на самом компьютере без необходимости использовать реальные сетевые интерфейсы.

### IP адрес от DHCP сервера и маршруты `ip addr show`

![3 ip show](./img/3 ip show.png)

### Расшифровка DHCP
DHCP (Dynamic Host Configuration Protocol) — это сетевой протокол, используемый для автоматической конфигурации сетевых параметров устройства. Он позволяет устройствам получать IP-адреса и другие параметры сети (например, шлюз и DNS-серверы) автоматически при подключении к сети.

### Определение внешнего и внутреннего IP-адреса шлюза 
#### Внутренний IP-адрес шлюза (IP по умолчанию) `ip route | grep default`
#### Внешний IP-адрес шлюза `curl ifconfig.me`
![3 ip dhcp, ip route](./img/3 ip dhcp, ip route.png)
### Настройка Ethernet
### Задание статических сетевых настроек`sudo vim /etc/netplan/01-netcfg.yaml`
Добавить
```network:
  version: 2
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

![3 add static ethernet sitting](./img/3 add static ethernet sitting.png)
### Применение изменений `sudo netplan apply`
![3 applay changes](./img/3 applay changes.png)
### Перезагрузка машины `sudo reboot`
![3 reboot](./img/3 reboot.png)
### Проверка статичных настроек
```ip addr show
ip route show
cat /etc/resolv.conf
```
![3 route and resolv](./img/3 route and resolv.png)
### Пинг 1.1.1.1 `ping -c 4 1.1.1.1`
![3 ping 1 1 1 1](./img/3 ping 1 1 1 1.png)
### Пинг ya.ru `ping -c 4 ya.ru`
![3 ping ya.ru](./img/3 ping ya.ru.png)

## 4. Обновление системных пакетов `sudo apt update` -> `sudo apt upgrade -y` -> `sudo apt update`
![4 update upgrade system](./img/4 update upgrade system.png)

## 5. Настройка пользователя rim для выполнения команд sudo

### Объяснение команды sudo
Команда sudo (от английского "superuser do") позволяет пользователю выполнять команды с привилегиями суперпользователя (root) или другого пользователя. Это необходимо для выполнения административных задач, таких как установка программ, изменение системных настроек и управление пользователями. Команда sudo предоставляет временный доступ к суперпользовательским правам, что помогает избежать постоянного использования root-аккаунта и повышает безопасность системы.

### add to group `sudo` `sudo usermod -aG sudo rim`
### checkout to rim `su - rim`
### Создание домашней директории и переключение на пользователя rim
![5 add dir for Rim, su rim](./img/5 add dir for Rim, su rim.png)
### Проверка переключения на пользователя rim
### check group user `groups rim`
![5 add su rim](./img/5 add su rim.png)
### Изменение hostname `sudo hostnamectl set-hostname user-2`
### Проверка результата `hostnamectl`
![5 check result](./img/5 check result.png)

## 6. Настройка службы автоматической синхронизации времени
### check systemd-timesyncd
```sudo systemctl enable systemd-timesyncd
sudo systemctl start systemd-timesyncd
```
### check status `systemctl status systemd-timesyncd`
### set time zone `sudo timedatectl set-timezone Europe/Moscow`
### check result `timedatectl`
### Синхронизация времени c NTP-серверами `timedatectl show`
![6 sync timedatectl](./img/6 sync timedatectl.png)

## 7. Работа с текстовыми редакторами
### Install editor
```sudo apt update
sudo apt install vim nano joe
```

### VIM
#### Сохранение изменений
![7 vim wq](./img/7 vim wq.png)
Для сохранения изменений и выхода в VIM используется команда `:wq`.

#### Выход без сохранения
![7 vim q!](./img/7 vim q!.png)
Для выхода без сохранения в VIM используется команда `:q!`.

#### Поиск и замена текста
![7 VIM find and change](./img/7 VIM find and change .png)
![7 VIM find and change result](./img/7 VIM find and change result.png)
Для поиска текста в VIM используется команда `/слово`, для замены текста используется команда `:%s/старое_слово/новое_слово/g`.

### NANO
#### Сохранение изменений
![7 nano save](./img/7 nano save.png)
Для сохранения изменений и выхода в NANO используется комбинация клавиш `Ctrl+O` для сохранения и `Ctrl+X` для выхода.

#### Выход без сохранения
![7 nano no save](./img/7 nano no save.png)
Для выхода без сохранения в NANO используется комбинация клавиш `Ctrl+X` и затем `N` для подтверждения.

#### Поиск и замена текста
![7 nano find and change](./img/7 nano find and change.png)
![7 nano find and change result](./img/7 nano find and change result.png)
Для поиска текста в NANO используется комбинация клавиш `Ctrl+W`, для замены текста используется комбинация клавиш `Ctrl+\\`.

### JOE
#### Сохранение изменений
![7 joe save ctrl+k - x](./img/7 joe save ctrl+k - x.png)
Для сохранения изменений и выхода в JOE используется комбинация клавиш `Ctrl+K` затем `X`.

#### Выход без сохранения
![7 joe no save ctrl+c - d](./img/7 joe no save ctrl+c - d.png)
Для выхода без сохранения в JOE используется комбинация клавиш `Ctrl+C` затем `D`.

#### Поиск и замена текста
![7 JOE find and chang](./img/7 JOE find and chang.png)
![7 JOE find and chang result](./img/7 JOE find and chang result.png)
Для поиска текста в JOE используется комбинация клавиш `Ctrl+K` затем `F`, для замены текста используется комбинация клавиш `Ctrl+K` затем `H`.

## 8. Настройка SSH
### install SSHd `sudo apt install openssh-server`
### add autostart `sudo systemctl enable ssh`

### Перенастройка службы SSHd
`sudo vim /etc/ssh/sshd_config` -> /#Port 22 -> Port 2022
![8 ssh PORT 2022](./img/8 ssh PORT 2022.png)

### Проверка службы SSHd

### Перезапуск службы SSH `sudo systemctl restart ssh`
### Проверка процесса SSHd `ps aux | grep sshd`

![8 check SSHd servicec](./img/8 check SSHd servicec.png)

### Использование команды ps для проверки процесса sshd
Команда `ps aux | grep sshd` используется для проверки наличия процесса sshd. В этой команде:
- `ps` - отображает информацию о запущенных процессах.
- `a` - показывает процессы всех пользователей.
- `u` - отображает процессы в удобочитаемом формате.
- `x` - показывает процессы, не привязанные к терминалу.

### Перезагрузка системы `sudo reboot`
### Проверка порта SSHd
```sudo apt install net-tools  # Установить netstat, если еще не установлен
netstat -tan | grep 2022
```
![8 netstat checking port result](./img/8 netstat checking port result.png)

### Объяснение ключей netstat -tan
- `-t` - отображает только TCP-соединения.
- `-a` - отображает все соединения и прослушивающие порты.
- `-n` - отображает адреса и порты в числовом виде.

Столбцы вывода команды netstat:
- `Proto` - протокол (TCP/UDP).
- `Recv-Q` - количество байт, ожидающих приема.
- `Send-Q` - количество байт, ожидающих отправки.
- `Local Address` - локальный адрес и порт.
- `Foreign Address` - удаленный адрес и порт.
- `State` - состояние соединения.

Значение `0.0.0.0` означает, что сервис прослушивает на всех интерфейсах.

## 9. Использование утилит top и htop `sudo apt install top htop`
### Вывод top
#### Общий вывод
![9 top](./img/9 top.png)
#### PID процесса с максимальным использованием памяти
![9 top PID max mem](./img/9 top PID max mem.png)
#### Процесс с максимальным использованием CPU
![9 top max CPU](./img/9 top max CPU.png)
#### Процесс с максимальным временем работы
![9 top max time](./img/9 top max time.png)

### Вывод `htop`
![9 htop](./img/9 htop.png)
#### Сортировка по PID `F6` -> `PID` -> `Enter`
![9 htop sort PID](./img/9 htop sort PID.png)
#### Сортировка по PERCENT_CPU `F6` -> `PERCENT_CPU` -> `Enter`
![9 htop sort PERCENT_CPU](./img/9 htop sort PERCENT_CPU.png)
#### Сортировка по PERCENT_MEM `F6` -> `PERCENT_MEM` -> `Enter`
![9 htop sort PERCENT_MEM](./img/9 htop sort PERCENT_MEM.png)
#### Сортировка по TIME `F6` -> `TIME` -> `Enter`
![9 htop sort time](./img/9 htop sort time.png)
#### Фильтрация для процесса sshd `F4` -> `sshd` -> `Enter`
![9 htop F4 SSHD](./img/9 htop F4 SSHD.png)
#### Поиск процесса syslog `/` -> `syslog` -> `Enter`
![9 htop syslog](./img/9 htop syslog.png)
#### Общий вывод с добавлением hostname, clock и uptime 
`F2` -> `Meters` -> tab x3 column `Avilable meters` -> On `Cloc, Hostname, Uptime`-> in column `Right column` will append `Cloc, Hostname, Uptime` -> `F10`

![9 htops hostname  clock uptime](./img/9 htops hostname  clock uptime.png)


## 10. Использование утилиты fdisk `fdisk -l`

### Вывод команды `fdisk -l`
![fdisk -l](./img/10 fdisk_l_output.png)

### Название жесткого диска
Жесткий диск: `/dev/sda`

### Размер жесткого диска
Размер: 25 GiB

### Количество секторов
Количество секторов: 52428800

### Размер swap
Swap раздел не найден в выводе `fdisk -l`. Вместо этого был создан swap-файл размером 2 GiB.

### Создание swap-file:
```
sudo fallocate -l 2G /swapfile # Создайте файл для swap
sudo chmod 600 /swapfile # Назначьте правильные разрешения
sudo mkswap /swapfile # Настройте файл как swap пространство
sudo swapon /swapfile # Активация swap файла
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab # Добавление swap-файл в fstab для авто подключения при старте
```

![add swap fdisk](./img/10 add swap fdisk.png)

Swap-файл не отображается в выводе команды `fdisk -l`, так как `fdisk` работает только с физическими разделами диска. Swap-файл — это обычный файл в файловой системе, и его наличие проверяется другими средствами.

### Проверка наличия и использования swap-файла

Для проверки текущего состояния swap-файла и его использования, используется команда `sudo swapon --show`.

![check swap fdisk](./img/10 check swap fdisk.png)


## 11. Использование утилиты `df`

### Команда `df` (disk free) используется для отображения информации о файловых системах. Она показывает размер, использованное и свободное пространство на файловых системах, а также процент использования дискового пространства.

### Информация о корневом разделе (`/`) с использованием команды `df`

Команда: `df`

![df](./img/11_df.png)

#### Информация о корневом разделе (`/`):

- **Размер раздела**: 11758760 KB
- **Размер занятого пространства**: 7082872 KB
- **Размер свободного пространства**: 4056780 KB
- **Процент использования**: 64%

#### Единица измерения в выводе:

Единица измерения в выводе команды `df` - KB (килобайты).

### Информация о корневом разделе (`/`) с использованием команды `df -Th`

Команда: `df -Th`

`-T`: Показывает тип файловой системы.

`-h`: Отображает размеры в читаемом для человека формате (например, M для мегабайт, G для гигабайт).

![df -Th](./img/11_df_-Th.png)

#### Информация о корневом разделе (`/`):

- **Размер раздела**: 12 GB
- **Размер занятого пространства**: 6.8 GB
- **Размер свободного пространства**: 3.9 GB
- **Процент использования**: 64%
- **Тип файловой системы**: ext4


## 12. Использование утилиты `du`

Команда `du` (disk usage) используется для оценки использования дискового пространства файлов и каталогов. Она отображает количество места, занимаемого файлами и директориями.


`du`: Команда для оценки использования дискового пространства.
`-s`: Параметр, указывающий команду du вывести общий размер указанного каталога.
`-h`: Параметр для отображения размеров в человекочитаемом виде (например, K, M, G).

### Размер папок:

**/home** `du -sh /home`
![du home](./img/12_du_home.png)

**/var** `sudo du -sh /var`
![du var](./img/12_du_var.png)

**/var/log ** `sudo du -sh /var/log`
![du var/log](./img/12_du_var_log.png)

**/var/log/\* ** `sudo du -sh /var/log/*`
![du var/log/*](./img/12_du_var_log_all.png)


## 13. Установка и использование утилиты `ncdu`

**ncdu (NCurses Disk Usage)** — утилита для анализа дискового пространства, работающая в режиме реального времени и предоставляющая удобный интерфейс для просмотра использования дискового пространства.

### Установка ncdu `sudo apt install ncdu`
![ins ncdu](./img/13_inst_ncdu.png)

### Размер папок:

**/home** `ncdu /home`
![ncdu home](./img/13_ncdu_home.png)

**/var** `sudo ncdu /var`
![ncdu var](./img/13_ncdu_var.png)

**/var/log** `sudo ncdu /var/log`
![ncdu var/log](./img/13_ncdu_var_log.png)

## 14. Работа с системными журналами

**/var/log/dmesg** `vim /var/log/dmesg`

![/var/log/dmesg](./img/14_var_log_dmesg.png)

**/var/log/syslog** `vim /var/log/syslog`

![/var/log/syslog](./img/14_var_log_syslog.png)

**/var/log/auth.log** `vim /var/log/auth.log`

![/var/log/auth](./img/14_var_log_auth.png)

**Время последней успешной авторизации** `grep "systemd-logind" /var/log/auth.log | tail -1`, `grep "TTY" /var/log/auth.log | tail -1`
```
Время последней успешной авторизации: Jun 27 06:10:58
Имя пользователя: rim
Метод входа в систему: tty (авторизация через физический терминал)
```

![14_auth_last_user1](./img/14_auth_last_user1.png)
![14_auth_last_user2](./img/14_auth_last_user2.png)

**Перезапуск службы SSHd** `sudo systemctl restart sshd`

**Отоброжение перезапуска службы в логах** `vim /var/log/auth.log`

![14_auth_rest_sshd](./img/14_auth_rest_sshd.png)


## 15. Использование планировщика заданий CRON

**Открываем CRON** `crontab -e`

**Добавляем цель, чтобы каждые 2 мин запускалась uptime** `*/2 * * * * /usr/bin/uptime`

![15_add_2min_uptime](./img/15_add_2min_uptime.png)

**Проверяем исполнение цели в системных логах** `sudo grep "uptime" /var/log/syslog`

![15_uptime_syslog](./img/15_uptime_syslog.png)

**Отоброзим все заданные цели в CRON** `crontab -l`

![15_check_all_target](./img/15_check_all_target.png)

**Удалим все цели из планировщика CRON** `crontab -r`

**Отоброзим все заданные цели в CRON** `crontab -l`

![15_del_all_target](./img/15_del_all_target.png)
