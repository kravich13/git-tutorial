 # Git всё для всего

## Команды

* ### `git init` - новая инициализация репозитория если его нет на хитхабе и хочешь создать его с нуля.
* ### `git status` - проверка статуса.
* ### `git log` - вывести лог коммитов.
* ### `git clone url` - клонировать содержимое репозитория по указанному адресу.
* ### `git add index.html`   - добавить файл для отслеживания.
* ### `git add *` - добавить все файлы в папке.
* ### `git commit -m "text"` - подтвердить добавленные изменения.
* ### `git remote` - узнать имя репозитория (обычно origin).
* ### `git push origin master` - залить изменения в репозиторий гитхаба.
* ### `git pull origin master` - скачать последние изменения с репозитория.
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