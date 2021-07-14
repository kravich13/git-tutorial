 - [Git всё для всего](#git-всё-для-всего)
  - [Команды](#команды)
  - [Работа с форматом вывода коммитов](#работа-с-форматом-вывода-коммитов)
  - [Отмена изменений и откат назад](#отмена-изменений-и-откат-назад)
  - [Создание ветки внутри репозитория](#создание-ветки-внутри-репозитория)
  - [Слияние веток](#слияние-веток)
  - [Различные комбинации и ситуации](#различные-комбинации-и-ситуации)
 
 # Git всё для всего

## Команды

* `git init` - новая инициализация репозитория если его нет на хитхабе и хочешь создать его с нуля.
*  `git status` - проверка статуса.
*  `git log` - вывести лог коммитов.
*  `git clone url` - клонировать содержимое репозитория по указанному адресу.
*  `git add index.html`   - добавить файл для отслеживания.
*  `git add *` - добавить все файлы в папке.
*  `git commit -m "text"` - подтвердить добавленные изменения.
*  `git remote` - узнать имя репозитория (обычно origin).
*  `git push origin master` - залить изменения в репозиторий гитхаба.
*  `git pull origin master` - скачать последние изменения с репозитория.
*  `git branch` - посмотреть текущие ветки
*  `git branch kravich` - создать новую ветку кравич
*  `git branch -d kravich` - удалить ветку
*  `git switch -c kravich` - создать ветку и сразу на неё переключиться
*  `git switch html` - переключиться на такую ветку
*  `git checkout <hash>` - вернуться к этой версии хеша.
*  `git checkout master` - `master` имя ветки по умолчанию, откатиться к последней версии.
*  `git merge master` - слияние ветки `master` со второстепенной веткой `style`.
*  `git rebase master` - почти тоже самое, что `merge`
*  Не использовать, если ветка является публичной и расшаренной. Переписывание общих веток будет мешать работе других членов команды.
*  Не использовать, когда важна точная история коммитов ветки (так как команда rebase переписывает историю коммитов).
*  `git cat file.name` - посмотреть содержимое файла.
*  `git tag v1` - установка тега в хеш коммита (можно обращаться как checkout v1). Также действует при откате на какую либо версию, установка имени его тега сохранится навсегда.
*  `git tag` - просмотр имён всех указанных тегов.
*  `git reset --hard <hash/tag>` - удалить локально коммиты до этого коммита (именно до).
*  `git tag d <hash/tag>` - удалить глобально указанный коммит из всей истории `hist --all`.
*  `git commit --amend -m "new commit"` - изменение последнего добавленного коммита:
    ```bash
    git add hello.html
    git commit -m "блабла"

    # поняли, что нужно изменить коммит т.к. что-то добавили в файл

    # изменяет старый коммист на новый
    git add hello.html
    git commit --amend -m "finaly commit"
    ```
***

## Работа с форматом вывода коммитов

Удобно смотреть к примеру хеш, дату, название коммита и от кого отправлено. 

```bash
git log --pretty=format:"%h %ad | %s%d [%an]" --graph --date=short

# Вывод в терминале
* 4830260 2020-10-13 | Added HTML header (HEAD -> master) [Vlad Kravich]
* 3243cb4 2020-10-13 | Added standart HTML page tags [Vlad Kravich]
* 2647139 2020-10-13 | -m "Added h1 tag" [Vlad Kravich]
* 2a36b35 2020-10-13 | First Commit [Vlad Kravich]
```

* `--pretty="..."` — определяет формат вывода.
* `%h` — укороченный хэш коммита
* `%d` — дополнения коммита («головы» веток или теги)
* `%ad` — дата коммита
* `%s` — комментарий
* `%an` — имя автора
* `--graph` — отображает дерево коммитов в виде ASCII-графика
* `--date=short` — сохраняет формат даты коротким и симпатичным

Также удобно смотреть все указанные теги в этом же логе с помощью `master --all`: 

```bash
[vladislav@vladislav-ms7996 hello]$ git hist (hist - всё что сверху) master --all

#изменный тег v1, master - 
* 4830260 2020-10-13 | Added HTML header (tag: v1, master) [Vlad Kravich]
* 3243cb4 2020-10-13 | Added standart HTML page tags (HEAD, tag: v1-beta) [Vlad Kravich]
* 2647139 2020-10-13 | -m "Added h1 tag" [Vlad Kravich]
* 2a36b35 2020-10-13 | First Commit [Vlad Kravich]
```
***

## Отмена изменений и откат назад

* Иногда случается, что был изменён файл в рабочем каталоге и нужно отменить последние коммиты. С этим справится команда `checkout`:

```bash
# изменил файл hello.html к примеру и нужно удалить эти изменения

git status
modified:   hello.html

# Для отмены изменений:
git checkout hello.html
git status 
cat hello.html

# Теперь hello.html был откатан до последнего коммита
```

* Отмена действий перед коммитом. Чтобы не вносить лишние изменения - придумана команда `reset`:

```bash
# был изменен файл и был сделан:
git add hello.html

# но коммитить его не хочется, приходит на помощь reset:
[vladislav@vladislav-ms7996 hello]$ git reset HEAD hello.html

Непроиндексированные изменения после сброса:
M       hello.html

# Эта команда никак не очищает сам файл, просто отменяет действия с гитом, чтобы откатить изменения в файле назад - примеры выше.
```

***

## Создание ветки внутри репозитория

Для создания веток внутри репозитория нужно:

```bash
# 1) добавить и закоммитить изменения
git add all
git commit -m "text"
git status 

# 2) Создать новую ветку
git checkout style

# внести добавление конкретно в эту ветку и добавить новые изменения
git add all
git commit -m "text"

# 3) После переключения на основную ветку master, можно убедиться, что в ней никаких изменений не было
git checkout master
git hist --all

# в каждой ветке будут видны свои коммиты
```
***

## Слияние веток

Слияние веток заменяет файлы из одной ветки в другой (если такие есть), если же их нет - добавляет.

```bash
# переход на вторую ветку и её слияение с веткой master
git checkout style
git merge master

```
***

## Различные комбинации и ситуации

* Пример скачивания, изменения и пуша изменений обратно в репозиторий гитхаба:

```bash
# Репозиторий на gitHub с названием js

# 1) В папке, куда хотим скачать репозиторий открываем терминал: 

#скачалась папка js со всем содержимым
git clone https://github.com/kravich13/js.git 


# добавили файл app.js

# 2) В папке репозитория (js) открываем терминал и добавляем изменения:
git add * 
git commit -m "text"
git push origin master

# теперь репозиторий js имеет новый добавленный файл app.js
```

* Пример с изменением самой структуры репозитория, когда удалились некоторые папки и добавились новые:

```bash
# Зашли в нужную папку репозитория на компе и скачали: 
git pull origin master

# 1) Изменили структуру папок, что-то добавили, что-то удалили.
git add *
git commit -m "text"

# Высветится то, что было удалено и изменено

# 2) Для удаления удаленных файлов с компа в репозитории прописываем: 
git add .
git commit -m "deleted"

# Все папки и файлы, котоырые были удалены в папке на компе теперь будут удалены с репозитория в гите
git push origin master
```

* Генерация `SSH` ключей для `git clone ssh-url`:

```bash
# В терминале со всеми репозиториями на компе: 

ssh-keygen
# далее далее далее

# будет показана папка с путём к файлу с ключём в скрытых файлах
# /home/vladislav/.ssh/id_rsa.pub

# копируем весь ключ вида: 
# ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDFh9qXUoPuzUTIDUSLtnEHQEtCp4XnYElKeS22hSCmFKHo8FjmnMkgz9O1kRJBLBE/ZtbU93c1STj+NfbVRXxNSEz9UV0R+cXMERH4gTM4DJShoxjApo7uwlSFNXNDZR1E+PFMha1f5AtuGjqX0XeLnzEEjLZvzXNNx+zGOwKBqqJco0JCm+R/EHp9FUtHIwDyuBQFruxh03W+sA36M6D7XMUPqwE7YrgvLYVrgDtNKlpmt/v9fE5BmV6612TtmWfxnC6Zl7/+GK+SzrfOpARFkiLCMwvm10JUqHaZwDeoFiZtfL+joJcZs6KZvUXZ6xKNMj7C+hP6LB5CtTBa72B+xaK2SadWLsF9QvsStsufKYoLHSeQWTY0SI0GWGhv1Ap+YhUtM4Uf3ABrRoB8MY3aXcdKkvtGOA6sP9LMPf+FJw/b7hD508V7ieMTv8dKrsnpoViifvjL5fbstI16p+NgQwfovkevi6c5aYhAqo1xSn847lAmQQ/5ZKlKJ63HL6E= 

# после преходим в git - setting - SSH - new Add key и вводим тут этот ключ.

git init ssh-url
```