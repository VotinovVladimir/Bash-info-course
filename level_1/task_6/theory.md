## 📘 Просмотр содержимого файлов

В DevOps часто приходится работать с конфигурациями, логами и скриптами прямо в терминале. Для этого Linux, macOS и WSL предлагают несколько инструментов для просмотра файлов: полностью, постранично или с конца файла. Они позволяют анализировать состояние системы, проверять ошибки и отслеживать обновления без графических редакторов.

Большинство файлов на серверах — текстовые, поэтому эти команды работают именно с ними, но некоторые флаги полезны и для бинарных или больших логов.

### Команда `cat` — полный вывод файла

`cat` (от **concatenate** — «соединять») выводит содержимое одного или нескольких файлов в терминал.

Примеры и флаги:

<pre class="overflow-visible!" data-start="976" data-end="1382"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>cat</span><span> file.txt                  </span><span># вывод всего содержимого файла</span><span>
</span><span>cat</span><span> file1.txt file2.txt       </span><span># вывод нескольких файлов подряд</span><span>
</span><span>cat</span><span> -n file.txt               </span><span># нумерует строки при выводе</span><span>
</span><span>cat</span><span> -b file.txt               </span><span># нумерует только непустые строки</span><span>
</span><span>cat</span><span> > newfile.txt             </span><span># создаёт новый файл и принимает ввод с клавиатуры</span><span>
</span><span>cat</span><span> file1.txt file2.txt > merged.txt   </span><span># объединяет файлы в новый</span><span>
</span></span></code></div></div></pre>

**Где применяют в DevOps:**

* Быстрый просмотр небольших конфигурационных файлов (`nginx.conf`, `.env`).
* Объединение логов для анализа.
* Создание временных файлов при тестировании скриптов.

⚠️ Для больших файлов `cat` не лучший вариант, так как вывод может «завалить» терминал.

### Команда `less` — постраничный просмотр

`less` позволяет просматривать файлы постранично, перемещаться вперёд и назад, искать строки и не загружать весь файл в память. Часто используется вместо устаревшей команды `more`.

Примеры и флаги:

<pre class="overflow-visible!" data-start="1928" data-end="2125"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>less file.txt                 </span><span># открыть файл для постраничного просмотра</span><span>
less +F /var/log/syslog       </span><span># аналог tail -f внутри less</span><span>
less -N file.txt              </span><span># показать номера строк</span><span>
</span></span></code></div></div></pre>

**Основные команды внутри less:**

* `Space` — следующая страница
* `b` — предыдущая страница
* `/текст` — поиск по файлу
* `n` — перейти к следующему результату поиска
* `q` — выйти

**Где применяют в DevOps:**

* Просмотр больших логов и конфигураций.
* Поиск ошибок в системных логах.
* Анализ результатов скриптов или CI/CD-процессов.

### Команда `more` — базовый постраничный просмотр

`more` выводит текст постранично, но не умеет прокручивать назад без дополнительных опций.

<pre class="overflow-visible!" data-start="2634" data-end="2705"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>more file.txt
more +5 file.txt        </span><span># начать с 5-й строки</span><span>
</span></span></code></div></div></pre>

**Где применяют:**

* Быстрый просмотр небольших логов.
* Сравнительно редкая команда, в современных системах `less` предпочтительнее.

### Команда `head` — начало файла

`head` выводит первые строки или байты файла. По умолчанию — 10 строк.

<pre class="overflow-visible!" data-start="2960" data-end="3174"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>head</span><span> file.txt            </span><span># первые 10 строк</span><span>
</span><span>head</span><span> -n 5 file.txt       </span><span># первые 5 строк</span><span>
</span><span>head</span><span> -c 50 file.txt      </span><span># первые 50 байт файла</span><span>
</span><span>head</span><span> -v file1.txt file2.txt  </span><span># выводить имена файлов перед содержимым</span><span>
</span></span></code></div></div></pre>

**Где применяют:**

* Проверка заголовков логов и конфигурационных файлов.
* Быстрый просмотр первых строк скриптов или результатов генерации.
* Использование в пайплайнах для фильтрации данных (`head | grep`).

### Команда `tail` — конец файла

`tail` выводит последние строки или байты файла, по умолчанию — 10 строк.

<pre class="overflow-visible!" data-start="3511" data-end="3825"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>tail</span><span> file.txt               </span><span># последние 10 строк</span><span>
</span><span>tail</span><span> -n 20 file.txt         </span><span># последние 20 строк</span><span>
</span><span>tail</span><span> -c 50 file.txt         </span><span># последние 50 байт</span><span>
</span><span>tail</span><span> -f /var/log/syslog     </span><span># следить за обновлениями файла в реальном времени</span><span>
</span><span>tail</span><span> -F /var/log/syslog     </span><span># как -f, но продолжает следить при ротации файла</span><span>
</span></span></code></div></div></pre>

**Где применяют:**

* Мониторинг логов приложений и системных сервисов.
* Отслеживание ошибок и статусов в режиме реального времени.
* Использование в скриптах для уведомлений и триггеров на события.

### Полезные советы и комбинации

* Для больших файлов лучше использовать `less` или `tail -f`, чтобы не перегружать терминал.
* `cat` подходит для маленьких файлов и объединения нескольких файлов в поток.
* `head` и `tail` позволяют быстро проверить начало или конец файла без лишнего вывода.
* Команды часто комбинируют с `grep` для фильтрации:

<pre class="overflow-visible!" data-start="4391" data-end="4444"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>grep </span><span>"error"</span><span> /var/log/syslog | </span><span>tail</span><span> -n 20
</span></span></code></div></div></pre>

* Можно использовать пайплайны, например:

<pre class="overflow-visible!" data-start="4487" data-end="4534"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>cat</span><span> log.txt | grep </span><span>"timeout"</span><span> | less
</span></span></code></div></div></pre>

Это позволяет искать и просматривать результаты постранично.

### Практика

1. Просмотрите содержимое файла `example.txt` с помощью `cat`.
2. Выведите первые 10 строк файла `example.txt` с помощью `head`.
3. Выведите последние 15 строк файла `example.txt` с помощью `tail -n 15`.
4. Следите за обновлениями файла `/var/log/syslog` с помощью `tail -f`.
5. Откройте файл `config.conf` постранично с помощью `less`.
6. Найдите слово `error` в файле `log.txt` с помощью `less` (`/error`).
7. Попробуйте использовать команду `more` для просмотра файла `example.txt`.
8. Объедините два файла `file1.txt` и `file2.txt` в поток с помощью `cat`.
9. Выведите первые 50 байт файла `example.txt` с помощью `head -c`.
10. Сочетайте `tail` и `grep`, чтобы отслеживать последние ошибки в лог-файле: `tail -f /var/log/syslog | grep error`.
