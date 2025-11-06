## 📘 Работа с файлами и папками

Работа с файлами и папками — это основа управления информацией в любой операционной системе. Любой DevOps-инженер или системный администратор ежедневно создаёт, перемещает, копирует и удаляет файлы и директории. Эти навыки особенно важны при работе с серверами, Docker-контейнерами и автоматизацией задач с помощью Bash-скриптов.

Файлы могут содержать текст, программы, конфигурации и другие данные. Папки (каталоги) служат для их организации. Linux и macOS используют иерархическую файловую систему с корнем `/`, а Windows — через диски (`C:\`, `D:\` и т.д.), хотя в WSL тоже используется Linux-структура.

### Команда `touch`

Команда `touch` создаёт пустой файл или обновляет дату последнего изменения существующего файла. Название команды буквально переводится как «прикоснуться».

Примеры использования:

<pre class="overflow-visible!" data-start="849" data-end="1055"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>touch</span><span> file.txt        </span><span># создаёт пустой файл file.txt</span><span>
</span><span>touch</span><span> report.log      </span><span># создаёт пустой файл report.log</span><span>
</span><span>touch</span><span> file.txt        </span><span># если файл уже существует, обновляет время последнего изменения</span><span>
</span></span></code></div></div></pre>

### Команда `mkdir`

Команда `mkdir` предназначена для создания новых папок. Аббревиатура `mkdir` расшифровывается как **make directory** — «создать каталог».

Примеры использования:

<pre class="overflow-visible!" data-start="1243" data-end="1382"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>mkdir</span><span> Test            </span><span># создаёт папку Test в текущем каталоге</span><span>
</span><span>mkdir</span><span> -p dir1/dir2    </span><span># создаёт вложенные папки dir1 и dir2 сразу</span><span>
</span></span></code></div></div></pre>

Ключ `-p` позволяет создавать сразу несколько уровней директорий, если они ещё не существуют.

### Команда `cp`

Команда `cp` используется для копирования файлов и папок. Аббревиатура `cp` — **copy**.

Примеры использования:

<pre class="overflow-visible!" data-start="1612" data-end="1778"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>cp</span><span> file.txt backup.txt        </span><span># копирует file.txt в backup.txt</span><span>
</span><span>cp</span><span> -r folder1 folder2         </span><span># рекурсивно копирует папку folder1 и её содержимое в folder2</span><span>
</span></span></code></div></div></pre>

Ключ `-r` необходим для копирования каталогов с содержимым.

### Команда `mv`

Команда `mv` предназначена для перемещения файлов и папок или переименования их. Аббревиатура `mv` — **move**.

Примеры использования:

<pre class="overflow-visible!" data-start="1997" data-end="2144"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>mv</span><span> file.txt /home/user/Documents/    </span><span># перемещает файл в указанный каталог</span><span>
</span><span>mv</span><span> oldname.txt newname.txt            </span><span># переименовывает файл</span><span>
</span></span></code></div></div></pre>

Команда `mv` удобна как для изменения расположения, так и для смены имени объекта.

### Команда `rm`

Команда `rm` удаляет файлы и папки. Аббревиатура `rm` расшифровывается как **remove** — «удалить».

Примеры использования:

<pre class="overflow-visible!" data-start="2374" data-end="2583"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>rm</span><span> file.txt             </span><span># удаляет файл file.txt</span><span>
</span><span>rm</span><span> -r folder1           </span><span># рекурсивно удаляет папку folder1 и все её содержимое</span><span>
</span><span>rm</span><span> -f file.txt          </span><span># принудительно удаляет файл без подтверждения</span><span>
</span></span></code></div></div></pre>

Ключ `-r` необходим для удаления каталогов с содержимым, а `-f` — для принудительного удаления без запросов.

### Полезные заметки

При работе с файлами важно помнить про права доступа: удалять или изменять файлы, принадлежащие другим пользователям, без прав администратора нельзя. Комбинация команд `ls -l` и `chmod` помогает контролировать доступ.

Также стоит использовать автодополнение через `Tab`, чтобы не ошибиться в именах файлов и папок. Всегда проверяйте путь с помощью `pwd` перед созданием, копированием или удалением объектов, чтобы не потерять важные данные.

### 10 практических задач

1. Создайте пустой файл `example.txt` с помощью `touch`.
2. Создайте папку `Projects` и вложенную папку `Test` внутри неё с помощью `mkdir -p`.
3. Скопируйте файл `example.txt` в папку `Projects/Test`.
4. Переименуйте скопированный файл в `example_copy.txt` с помощью `mv`.
5. Создайте несколько файлов (`file1.txt`, `file2.txt`, `file3.txt`) в текущей директории.
6. Используя `ls -l`, проверьте, что файлы созданы.
7. Переместите все созданные файлы в папку `Projects`.
8. Удалите один файл из папки `Projects` с помощью `rm`.
9. Создайте ещё одну папку и попробуйте рекурсивно скопировать все содержимое первой папки в новую.
10. Удалите все созданные папки и файлы с помощью `rm -r`, проверяя результаты командой `ls`.
