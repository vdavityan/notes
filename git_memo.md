# Git memo: команды

## Конфигурация

### Имя и электронная почта
    git config --global user.name "Your Name"
    git config --global user.email "email@example.com"

### Окончания строк

#### Unix/Mac
    git config --global core.autocrlf input
    git config --global core.safecrlf warn

#### Windows

    git config --global core.autocrlf true
    git config --global core.safecrlf warn

### Unicode

    git config --global core.quotepath off

### Алиасы
    git config --global alias.co checkout
    git config --global alias.ci commit
    git config --global alias.st status
    git config --global alias.br branch
    git config --global alias.hist "log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short"
    git config --global alias.type 'cat-file -t'
    git config --global alias.dump 'cat-file -p'

### Алиасы в командной строке
    alias gs='git status '
    alias ga='git add '
    alias gb='git branch '
    alias gc='git commit'
    alias gd='git diff'
    alias gco='git checkout '
    alias gk='gitk --all&'
    alias gx='gitx --all'

    alias got='git '
    alias get='git '

## Инициализация, состояние, история.
    git init
    git status
    git log 
    git log --all - все ветки
    git log --all --graph --oneline  --decorate
    git reflog

### Контроль отображения записей
    git log --pretty=oneline --max-count=2
    git log --pretty=oneline --since='5 minutes ago'
    git log --pretty=oneline --until='5 minutes ago'
    git log --pretty=oneline --author=<your name>
    git log --pretty=oneline --all
    git log --all --pretty=format:"%h %cd %s (%an)" --since='7 days ago'
    git log --pretty=format:"%h %ad | %s%d [%an]" --graph --date=short

## Индексация и коммит
    git add <file>
    git commit -m "Add file <file>"

## Создание тегов версий
### Легковесный тег
    git tag 1.0.0

### Аннотируемый
    git tag -a 1.0.0

### Работа с тегами
    git checkout 1.0.0
    git checkout 1.0.0~1
#### Отправить на удаленный
    git push origin v1.0.0
    git push origin --tags
#### Удаление
1. Локально
    * `git tag -d 1.0.0`
2. Удаленный
    * `git push origin :refs/tags/1.0.0`
    * `git push origin --delete 1.0.0`


## Ветки
### Создание
* `git branch <branch>`
* `git branch -b <branch>` - создать и перейти
### Навигация
    git checkout <branch>
### Слияние master с веткой dev
    git checkout master
    git merge dev
### Список всех веток
    git branch -a
### Отслеживание удаленной ветки
    git branch --track <branch> origin/<branch>

## Отмена изменений

### Измените предыдущий коммит
    git commit --amend

### Перемещение файлов
    git mv <old file> <new file>

### Возвращать рабочий каталог к любому предыдущему состоянию.
    git checkout <commit hash>
    git chekout HEAD~1 (на один коммит назад)

### Вернитесь к последней версии в ветке master
    git checkout master

### Отмена изменений файла в рабочем каталоге
    git checkout <file>

### Отмена проиндексированных изменений    
    git reset HEAD <file>
    git restore --staged <file>

### Отмена коммитов
#### С сохранением истории (для публичных веток)
    git revert HEAD
    git revert <hash>
#### Удаление комитов из ветки (для локальных веток)
    git reset --hard <hash>
    git reset --hard <tag>
#### Перебазирование
    git rebase master
