# Работа с пользователями в linux

## Создание пользователя

```
sudo useradd <options> <username>
```

Создание пользователя с домашней директорией с флагом `-m (--create-home)`:

```
sudo useradd -m deployer
```
Проверить содержимое домашней директории:

```
ls -a /home/deployer
.  ..  .bash_logout  .bashrc  .profile
```

Установить пароль для нового пользователя:

```
sudo passwd deployer
```
Установить zsh как оболочку для юзера:

```
sudo chsh --shell /bin/zsh deployer
```
Переключиться на нового пользователя:

```
su deployer
```

## Полезные команды для работы с пользователями

Посмотреть список пользователей

```
cat /etc/passwd
```

Добавить юзера в группу:

```
sudo usermod -aG <groupname> <username>
```

Например, можно добавить юзера в sudoers (добавить в группу sudo, выполнять от рута):

```
sudo usermod -aG sudo deployer
```