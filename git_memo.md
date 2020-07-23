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

### Контроль отображения записей
    git log --pretty=oneline --max-count=2
    git log --pretty=oneline --since='5 minutes ago'
    git log --pretty=oneline --until='5 minutes ago'
    git log --pretty=oneline --author=<your name>
    git log --pretty=oneline --all
    git log --all --pretty=format:"%h %cd %s (%an)" --since='7 days ago'
    git log --pretty=format:"%h %ad | %s%d [%an]" --graph --date=short

## Индексация и коммит
    git add file_name
    git commit -m "Add file file_name"

