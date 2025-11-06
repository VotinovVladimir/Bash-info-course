## 📘 Просмотр содержимого файлов

Умение просматривать содержимое файлов — ключевой навык работы в терминале. DevOps-инженеру часто нужно быстро анализировать конфигурации, логи или скрипты без открытия графического редактора. В Linux, macOS и WSL есть несколько команд для просмотра файлов различными способами: полностью, постранично или с определённого конца.

Файлы могут быть любого типа, но для большинства текстовых файлов удобнее всего использовать стандартные инструменты командной строки.

### Команда `cat`

Команда `cat` (от **concatenate** — «соединять») выводит полный текст файла в терминал. Она проста и часто используется для просмотра небольших файлов.

Примеры использования:

<pre class="overflow-visible!" data-start="706" data-end="926"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>cat</span><span> file.txt                  </span><span># выводит содержимое file.txt</span><span>
</span><span>cat</span><span> file1.txt file2.txt       </span><span># выводит содержимое двух файлов подряд</span><span>
</span><span>cat</span><span> > newfile.txt             </span><span># создаёт новый файл и вводит текст с клавиатуры</span><span>
</span></span></code></div></div></pre>

Особенности: `cat` сразу выводит весь файл в терминал, поэтому для больших файлов лучше использовать постраничный просмотр.

### Команда `less`

Команда `less` позволяет просматривать содержимое файлов постранично и навигировать внутри них. Аббревиатура неточная, но `less` считается улучшенной версией команды `more`.

Примеры использования:

<pre class="overflow-visible!" data-start="1279" data-end="1421"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>less file.txt                 </span><span># открывает файл для постраничного просмотра</span><span>
less /var/log/syslog           </span><span># удобный просмотр логов</span><span>
</span></span></code></div></div></pre>

Навигация внутри `less`:

* `Space` — перейти на следующую страницу
* `b` — вернуться на страницу назад
* `/текст` — поиск по файлу
* `q` — выйти из просмотра

`less` не загружает весь файл в память, поэтому удобно работать с очень большими файлами.

### Команда `more`

Команда `more` похожа на `less`, но менее функциональна. Она также выводит текст постранично, но не позволяет прокручивать назад без специальных опций.

Пример использования:

<pre class="overflow-visible!" data-start="1885" data-end="1956"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>more file.txt                 </span><span># постраничный просмотр файла</span><span>
</span></span></code></div></div></pre>

`more` полезна для быстрого просмотра небольших файлов или логов, но `less` более универсальна.

### Команда `head`

Команда `head` выводит **первые строки** файла. По умолчанию — 10 строк.

Примеры использования:

<pre class="overflow-visible!" data-start="2180" data-end="2339"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>head</span><span> file.txt                 </span><span># первые 10 строк</span><span>
</span><span>head</span><span> -n 5 file.txt            </span><span># первые 5 строк</span><span>
</span><span>head</span><span> -c 50 file.txt           </span><span># первые 50 байт файла</span><span>
</span></span></code></div></div></pre>

`head` особенно удобна для быстрого просмотра начала логов или конфигурационных файлов.

### Команда `tail`

Команда `tail` выводит **последние строки** файла. По умолчанию — 10 строк.

Примеры использования:

<pre class="overflow-visible!" data-start="2558" data-end="2752"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>tail</span><span> file.txt                 </span><span># последние 10 строк</span><span>
</span><span>tail</span><span> -n 20 file.txt           </span><span># последние 20 строк</span><span>
</span><span>tail</span><span> -f /var/log/syslog       </span><span># следить за обновлениями файла в реальном времени</span><span>
</span></span></code></div></div></pre>

Ключ `-f` (follow) делает `tail` незаменимой для мониторинга логов в реальном времени.

### Полезные заметки

* Для больших файлов предпочтительнее использовать `less` или `tail -f`, чтобы не перегружать терминал.
* `cat` удобен для объединения файлов в один поток или создания новых файлов.
* `head` и `tail` позволяют быстро проверить начало или конец файла без просмотра всего содержимого.
* Эти команды часто комбинируются с `grep`, чтобы фильтровать содержимое по шаблону:

<pre class="overflow-visible!" data-start="3246" data-end="3299"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>grep </span><span>"error"</span><span> /var/log/syslog | </span><span>tail</span><span> -n 20
</span></span></code></div></div></pre>

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
