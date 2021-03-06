[Я не туда нажал и хочу обратно в оглавление](./000%20toc.md)
# Проверяем, что все установилось и начинаем работать в терминале
Подавляющее большинство серверных приложений (и наши для тестов в их числе) запускаются, настраиваются, стартуют и останавливаются через терминал, и в принципе не имеют графического интерфейса. Потому что, чем меньше у тебя ***GUI***, тем меньше ошибок в программе.

## Найдем терминал
![Стартуем терминал](./img/006%20UbuRunTerminal.png)

Что нам сейчас нужно проверить:
1. Мы сделали корректные настройки сети в VirtualBox и система получает IP адрес
2. Мы можем достучаться до виртуальной машины из нашей машины-хозяина

### Немного терминологии
**Машина-хозяин (host)** в нашем случае — это тот компьютер, клавиатура от которого у нас на столе. Более широко **хозяин** — это та машина, которая предоставляет свои ресурсы для гостевых машин.

**Машина-гость (guest)** — это Нео, который живет в Матрице, т.е. это машина, которая потребляет ресурсы хозяина и что-то делает. Ресурсы ей отдает программа, которая называется гипервизор.

**Гипервизор (hypervisor)** — софт, который отбирает ресурсы у богатенького хоста и отдает их гостю. По сути гипервизор — это и есть Матрица. Он обманывает гостевую ОС, говоря ей, что у нее есть память, диск, USB и все, что так любят операционные системы. В нашем примере гипервизор — это VirtualBox.

## Проверяем IP адрес
Как оказалось, для линуксов есть просто ужасно большое количество способов узнать, какой IP адрес у машины.

Вот парочка из них:

``` hostname -I ```

Вывод команды будет довольно спартанский:

![hostname -I](./img/006%20CheckHostnameI.png)


``` ip addr show ```

Вывод команды будет немного побогаче предыдущей, но вам решать, нужно ли это на текущем этапе:

![hostname -I](./img/006%20CheckIpAddrShow.png)

Эта команда уже добавляет информацию о MAC адресе и CIDR нотацию (192.168.1.xxx ***/24***), которая неискушенному пользователю может показаться страшной или непонятной. Ну, и ладно.

Обе команды показали, что IP адрес у гостевой системы есть. Это IP адрес из той же подсети, в которой находится машина-хозяин.

### Полезно знать: история команд терминала
Терминал сохраняет некоторую историю команд, что позволяет порой экономить много времени, и не печатать их заново с теми же или схожими параметрами.

История вызывается нажатием клавиш Вверх и Вниз. Чем больше нажимаете Вверх, тем далее в историю команд вы погружаетесь.

###  А что нам скажет наш домашний роутер?

![роутер тоже нас нашел](./img/006%20checkRouter.png)

Роутер нас тоже нашел. В дальнейшем к MAC-адресу виртуальной машины можно будет привязать статический IP-адрес на роутере и нам не потребуется его каждый раз проверять (по умолчанию все роутеры раздают адреса динамически).
#### Важно знать
Если вы меняете сетевые настройки для гостевой ОС на гипервизоре, то он может сгенерировать новый MAC-адрес. Возможно, имеет смысл сохранить строку MAC-адреса где-нибудь, чтобы не нужно было переделывать настройки в локальном браузере при повторном создании виртуальной машины (ну, мало ли).

### Немного командной строки Windows, %username%

Теперь проверим, можем ли мы достучаться до гостя из машины хозяина.
#### Жмём Win + R
Это сочетание клавиш открывает окно для ввода команд, которые вы хотите заставить вашу Виндовс выполнять. Нам нужно запустить командный интерпретатор **cmd.exe**

![cmd](./img/006%20CheckRunCmd.png)

Откроется черное окно, которое как бы приглашает нас делать всякие тёмные делишки.

Запускам в черном окне:

``` ping 192.168.1.xxx ```, 

где xxx — это тот реальный кусок IP-адреса, который вам вернула гостевая система.

![ping guest](./img/006%20CheckPingGuest.png)

Если Lost = 0, то мы можем достучаться до гостевой системы и все сетевые настройки были сделаны корректно.

### Остановка и перезагрузка сервера. Права root.
Начнем, конечно же, в обратном порядке.

Чтобы пользователь мог выполнять команды, которые с точки зрения ОС Линукс являются критичными для системы, права пользователя должны быть выше, чем права обычного пользователя. Для этого в Linux используется команда ``` sudo ``` , она должна предшествовать всем командам, которые могут нарушить спокойный ход событий.

#### Перезагрузка сервера
Эта операция является критической. Следовательно нужно использовать ```sudo```:

```sudo reboot```

#### Останов сервера
Эта операция является критической. Следовательно нужно использовать ```sudo```:

```sudo shutdown```

Остановка сервера — процедура, которую можно отменить. Выключение без ипользования доп. параметров происходит через 1 минуту. 

![ping guest](./img/006%20CheckShutdown.png)

```shutdown -c```

Теперь мы можем переходить настройкам ssh на локальной windows машине.

- [x] [Создадим виртуальную машину.](005%20vm%20and%20ubuntu.md)
- [x] [Установим на нее Ubuntu.](005%20vm%20and%20ubuntu.md)
- [x] [Проверим, что все установилось.](006%20checkWeAreOkay.md) 
- [x] [Да! Еще выучим пару команд линуха.](006%20checkWeAreOkay.md) :arrow_left: Вы ща здесь
- [ ] [Настроим ssh на локальной Windows машине.](007%20sshLocalWindows.md)
- [ ] Настроим ssh сервер на Ubuntu.
- [ ] Настроим аутентификацию при помощи публичного ключа на Ubuntu.
- [ ] Начнем работать с Ubuntu только через терминал.
- [ ] Установим JDK.
- [ ] Установим SDK.
- [ ] Установим Gradle.
- [ ] Установим Jenkins.
- [ ] Установим Docker.
- [ ] Установим Selenoid.

[Оглавление](./000%20toc.md)