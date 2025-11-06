## 📘 Пайпы в Bash: передача данных между командами

В Bash пайпы (`|`) позволяют передавать **вывод одной команды (stdout)** напрямую **в качестве входа другой команды (stdin)**. Это один из самых мощных инструментов командной строки, который позволяет создавать цепочки команд для фильтрации, обработки и анализа данных без создания временных файлов.

Для DevOps‑инженера пайпы особенно полезны при:

* анализе логов;
* фильтрации информации о процессах;
* комбинировании вывода нескольких утилит для автоматизации;
* быстром диагностировании состояния системы.

### Идея и назначение

Пайпы (`|`) в Unix/Linux придуманы для того, чтобы **соединять команды между собой** и создавать **цепочки обработки данных без промежуточных файлов**.

Вместо того чтобы сохранять вывод одной команды в файл, а потом читать этот файл другой командой, пайпы позволяют **передавать данные напрямую от одной программы к другой**.

Примеры задач, где это особенно удобно:

* Просмотр только нужных строк в логах (`dmesg | grep error`)
* Подсчёт количества процессов, соответствующих критериям (`ps aux | grep nginx | wc -l`)
* Сортировка и фильтрация данных, получаемых с разных команд (`cat /var/log/syslog | grep ssh | sort | uniq`)

Идея проста: **одна команда генерирует поток данных, другая команда этот поток потребляет**. Это делает работу в терминале гибкой и эффективной, особенно когда обрабатывается большое количество информации.

### Как работает пайп

Синтаксис очень простой:

<pre class="overflow-visible!" data-start="785" data-end="816"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>команда1 | команда2
</span></span></code></div></div></pre>

* `команда1` выполняется и отправляет свой стандартный вывод через пайп.
* `команда2` получает этот вывод как стандартный ввод и продолжает обработку.

Например, чтобы найти все файлы с расширением `.log` в текущем каталоге:

<pre class="overflow-visible!" data-start="1048" data-end="1079"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>ls</span><span> -l | grep </span><span>".log"</span><span>
</span></span></code></div></div></pre>

* `ls -l` выводит полный список файлов.
* `grep ".log"` фильтрует строки, оставляя только файлы с `.log`.

Такой подход позволяет создавать цепочки команд, которые выполняют сложные действия без промежуточных файлов.

### Как это работает внутри

Когда вы пишете команду с пайпом происходит следующее:

1. **Создание канала (pipe)**
   Операционная система создаёт специальный канал (в памяти), который соединяет stdout первой команды с stdin второй команды.
2. **Буферизация данных**
   Вывод первой команды временно хранится в буфере канала. Вторая команда читает данные из этого буфера по мере необходимости. Это происходит **почти одновременно**: первая команда продолжает генерировать данные, а вторая — их потребляет.
3. **Передача только stdout**
   По умолчанию пайпы передают только стандартный вывод (stdout). Стандартная ошибка (stderr) не передаётся, если её явно не перенаправить (`2>&1`).
4. **Производительность**
   Так как данные передаются через память, пайпы очень быстрые и не создают временных файлов на диске. Это важно, когда на сервере работает много процессов или обрабатываются большие лог-файлы.

### Примеры использования пайпов

1. **Фильтрация процессов по имени**

<pre class="overflow-visible!" data-start="1380" data-end="1409"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>ps aux | grep ssh
</span></span></code></div></div></pre>

Выводит все процессы, связанные с SSH, без необходимости вручную просматривать весь список процессов.

2. **Подсчёт количества файлов или строк**

<pre class="overflow-visible!" data-start="1558" data-end="1580"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>ls</span><span> | </span><span>wc</span><span> -l
</span></span></code></div></div></pre>

* `ls` выводит список файлов и папок.
* `wc -l` считает количество строк, то есть количество файлов.

3. **Сочетание нескольких команд**

<pre class="overflow-visible!" data-start="1724" data-end="1767"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>dmesg | grep error | </span><span>tail</span><span> -n 20
</span></span></code></div></div></pre>

* `dmesg` выводит системные сообщения ядра.
* `grep error` оставляет только строки с ошибками.
* `tail -n 20` показывает последние 20 ошибок.

Такой приём особенно полезен для DevOps при мониторинге серверов или приложений в реальном времени.

4. **Пайп с сортировкой и уникальными значениями**

<pre class="overflow-visible!" data-start="2071" data-end="2127"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>cat</span><span> /var/log/syslog | grep ssh | </span><span>sort</span><span> | </span><span>uniq</span><span>
</span></span></code></div></div></pre>

* Фильтруем строки с `ssh`.
* Сортируем их для упорядочивания.
* `uniq` убирает дубликаты, оставляя только уникальные записи.

5. **Использование пайпов с подсчетом**

<pre class="overflow-visible!" data-start="2303" data-end="2342"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>ps aux | grep nginx | </span><span>wc</span><span> -l
</span></span></code></div></div></pre>

* Подсчитываем количество процессов `nginx` на сервере.

### Полезно знать

* Пайпы передают **только stdout**, поток ошибок (stderr) по умолчанию не передаётся. Чтобы включить ошибки в пайп, нужно перенаправить stderr в stdout:

<pre class="overflow-visible!" data-start="2581" data-end="2623"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>command</span><span> 2>&1 | another_command
</span></span></code></div></div></pre>

* Можно строить длинные цепочки пайпов, например: `command1 | command2 | command3`, что позволяет последовательно фильтровать и обрабатывать данные.
* Пайпы очень часто применяются вместе с `grep`, `awk`, `sed`, `sort`, `uniq`, `wc` для анализа логов, мониторинга процессов и обработки данных на серверах.

### Практические задания

1. Выведите список всех процессов, связанных с Python, и подсчитайте их количество:

<pre class="overflow-visible!" data-start="3051" data-end="3091"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>ps aux | grep python | </span><span>wc</span><span> -l
</span></span></code></div></div></pre>

2. Найдите все файлы `.conf` в текущем каталоге и выведите только уникальные имена файлов:

<pre class="overflow-visible!" data-start="3185" data-end="3250"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>ls</span><span> -l | grep </span><span>".conf"</span><span> | awk </span><span>'{print $9}'</span><span> | </span><span>sort</span><span> | </span><span>uniq</span><span>
</span></span></code></div></div></pre>

3. Просмотрите системные логи `/var/log/syslog` и найдите последние 10 ошибок с помощью `dmesg` и пайпов.
4. Создайте цепочку, которая:
   * выводит список всех пользователей системы (`cat /etc/passwd`),
   * оставляет только пользователей с домашними директориями `/home`,
   * сортирует их по алфавиту и показывает первые 5 записей.
