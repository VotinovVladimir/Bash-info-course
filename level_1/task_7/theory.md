## 📘 Перенаправление потоков ввода и вывода в Bash

Работа с потоками ввода и вывода — важный навык для любого DevOps-инженера. Перенаправление потоков позволяет контролировать, куда поступают данные команд и куда отправляются результаты выполнения. Это особенно полезно при автоматизации задач, работе с серверами, Docker-контейнерами и анализе логов.

В Bash команды по умолчанию используют три потока:

* **stdin (стандартный ввод, 0)** — поток, через который команда получает данные, чаще всего с клавиатуры.
* **stdout (стандартный вывод, 1)** — поток, в который команда выводит результат своей работы, обычно на экран терминала.
* **stderr (стандартная ошибка, 2)** — поток, в котором выводятся сообщения об ошибках, возникающих при выполнении команды.

Управляя этими потоками, можно направлять вывод в файлы, подавать данные из файлов или передавать результат одной команды в другую. Для этого используются специальные символы:

* `>` — перенаправляет стандартный вывод в файл, перезаписывая его.
* `>>` — добавляет стандартный вывод в конец файла.
* `<` — перенаправляет стандартный ввод из файла.
* `2>` — перенаправляет поток ошибок в файл.
* `2>>` — добавляет поток ошибок в конец файла.
* `&>` — объединяет stdout и stderr в один файл.
* `|` — «пайп» передаёт stdout одной команды как stdin другой.

### Перенаправление стандартного вывода

Перенаправление стандартного вывода позволяет записывать результат выполнения команды в файл вместо экрана. Например, чтобы сохранить список файлов каталога в файл `test.txt`, используют команду:

<pre class="overflow-visible!" data-start="1878" data-end="1903"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>ls</span><span> > test.txt
</span></span></code></div></div></pre>

Если файл уже существует, его содержимое перезаписывается. Чтобы не потерять данные и добавить результат к существующему файлу, применяется `>>`:

<pre class="overflow-visible!" data-start="2052" data-end="2078"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>ls</span><span> >> test.txt
</span></span></code></div></div></pre>

В DevOps такие подходы применяются для ведения логов скриптов, отчетов о состоянии системы и автоматического сохранения результатов команд.

### Перенаправление стандартного ввода

Стандартный ввод можно перенаправить из файла с помощью `<`. Например, сортировка списка из файла `input.txt` выполняется так:

<pre class="overflow-visible!" data-start="2394" data-end="2422"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>sort</span><span> < input.txt
</span></span></code></div></div></pre>

Вместо ручного ввода данных команда получает их автоматически. Это особенно удобно для обработки больших списков серверов, конфигурационных файлов или логов.

Схожий принцип используется с пайпами `|`, когда вывод одной команды подаётся как ввод другой:

<pre class="overflow-visible!" data-start="2679" data-end="2710"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>ls</span><span> -l | grep </span><span>"test"</span><span>
</span></span></code></div></div></pre>

Здесь команда `grep` ищет слово `test` в выводе `ls -l`.

### Перенаправление ошибок

Отдельный поток ошибок (stderr) можно перенаправлять в файл с помощью `2>`:

<pre class="overflow-visible!" data-start="2880" data-end="2920"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>ls</span><span> /nonexistent 2> error.log
</span></span></code></div></div></pre>

Все сообщения об ошибках команды будут записаны в `error.log`, а стандартный вывод останется на экране. Чтобы добавлять ошибки в конец файла, используется `2>>`:

<pre class="overflow-visible!" data-start="3085" data-end="3126"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>ls</span><span> /nonexistent 2>> error.log
</span></span></code></div></div></pre>

Объединение стандартного вывода и ошибок удобно делать через `&>` или комбинацию `> файл 2>&1`:

<pre class="overflow-visible!" data-start="3225" data-end="3265"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>bash deploy.sh &> deploy.log
</span></span></code></div></div></pre>

Это создаёт полный лог выполнения скрипта, включая ошибки и успешные сообщения.

### Игнорирование потоков и `/dev/null`

Иногда нужно «выбросить» ненужный вывод команды, чтобы он не отображался в терминале и не занимал место в файле. Для этого используется специальное устройство `/dev/null`:

<pre class="overflow-visible!" data-start="3567" data-end="3741"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>./script.sh > /dev/null        </span><span># игнорировать stdout</span><span>
./script.sh 2> /dev/null       </span><span># игнорировать stderr</span><span>
./script.sh &> /dev/null       </span><span># игнорировать оба потока</span><span>
</span></span></code></div></div></pre>

Такой подход полезен для фоновых скриптов или задач, где важны только ошибки или только результат выполнения.

### Комбинированные сценарии

Перенаправления можно комбинировать с пайпами и другими командами. Например:

<pre class="overflow-visible!" data-start="3967" data-end="4034"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>./deploy.sh 2> error.log | grep </span><span>"success"</span><span> > success.log
</span></span></code></div></div></pre>

Здесь ошибки сохраняются в `error.log`, а строки с текстом `success` попадают в `success.log`.

В DevOps такие комбинации помогают:

* следить за состоянием систем и приложений;
* фильтровать логи в реальном времени;
* собирать отчёты о работе скриптов и приложений;
* автоматизировать обработку ошибок и результатов.

### Практические советы

1. Всегда проверяйте, какой поток вы перенаправляете: stdout или stderr.
2. Используйте `>>`, чтобы не перезаписывать важные файлы логов.
3. Команда `tee` позволяет одновременно выводить данные на экран и в файл:

<pre class="overflow-visible!" data-start="4613" data-end="4653"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>./deploy.sh | </span><span>tee</span><span> deploy.log
</span></span></code></div></div></pre>

4. Для изучения доступных опций используйте `man` и `--help` команд.
5. При работе с пайпами и скриптами комбинируйте команды для автоматической обработки данных и логирования.

Перенаправление потоков — один из ключевых инструментов Bash. Оно позволяет контролировать, куда идут данные и куда записываются результаты команд, а также организовывать автоматизацию, логирование и мониторинг. Освоив эти техники, начинающий DevOps-инженер сможет писать более надёжные скрипты, анализировать работу систем и эффективно управлять процессами на серверах и в контейнерах.

### Практика

1. **Фильтрация логов с пайпами и stderr**
   У вас есть файл `app.log`, в котором есть как информационные сообщения, так и ошибки.

   * Выведите только строки с ошибками (`error`) и перенаправьте их в файл `errors_only.log`.
   * Добавьте новые ошибки в тот же файл, не перезаписывая его.
2. **Комбинированное использование stdout и stderr**
   Выполните команду, которая создаёт файлы в недоступной директории, например:

   <pre class="overflow-visible!" data-start="734" data-end="771"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>mkdir</span><span> /root/testdir
   </span></span></code></div></div></pre>

   * Перенаправьте успешные сообщения в `success.log`.
   * Перенаправьте ошибки в `errors.log`.
   * Объедините оба потока в один файл `combined.log`.
3. **Использование `tee` для логирования и мониторинга**
   Запустите команду `ping -c 5 8.8.8.8` и сохраните результат в файл `ping.log`, одновременно выводя его на экран.
4. **Отслеживание обновлений логов в реальном времени**
   Используйте `tail -f` на лог-файле `/var/log/syslog` (или любого локального файла `system.log`) и одновременно фильтруйте строки с ключевым словом `error` с помощью `grep`.

   * Попробуйте перенаправить результат в `live_errors.log`.
5. **Комбинация сортировки и перенаправления**
   У вас есть файл `numbers.txt` с случайными числами.

   * Отсортируйте его по возрастанию и сохраните результат в `sorted.txt`.
   * Добавьте к отсортированному файлу новые числа и снова отсортируйте всё в `sorted.txt`, не теряя существующие данные.
6. **Скрипт для DevOps‑мониторинга**
   Напишите простой скрипт `check_disk.sh`, который проверяет использование диска (`df -h`).

   * Перенаправьте успешный вывод в `disk_status.log`.
   * Перенаправьте ошибки (если диск недоступен) в `disk_errors.log`.
   * Настройте комбинированный вывод для полного лога `disk_full.log`.
7. **Фильтрация командной цепочки**
   Используйте команду `ps aux | grep ssh` и сохраните все процессы SSH в файл `ssh_processes.log`.

   * Добавьте к этой команде перенаправление ошибок в отдельный файл `ps_errors.log`.
8. **Игнорирование ненужного вывода**
   Выполните команду, которая генерирует много стандартного вывода, например `find / -name "*.tmp"`:

   * Отправьте stdout в `/dev/null`, оставив только ошибки для анализа.
