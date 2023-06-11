# my-zdotdir

> Пример конфига для Zsh с использованием менеджера плагинов Antidote.

## Описание

Это — форк оригинального [getantidote/zdot](https://github.com/getantidote/zdot).

Цель этого проекта — дать вам пример конфига Zsh, который использует менеджер плагинов [Antidote] с некоторыми плагинами от комьюнити, доступными из коробки. Считайте это стартовым паком для Antidote и Zsh. Адаптируйте его под свои потребности.

## Что включено

В файле `.zsh_plugins.txt` содержатся директивы для установки плагинов, которые дарят пользователям Zsh следующие фичи:
- Хорошие дефолтные параметры для Zsh от [zsh-utils](https://github.com/sorin-ionescu/prezto).
- Автопредложения от [zsh-users/autosuggestions](https://github.com/zsh-users/zsh-autosuggestions).
- Поиск по истории ввода конкретной команды с [History substring searching](https://github.com/zsh-users/zsh-history-substring-search).
- [Подсветка синтаксиса](https://github.com/zdharma-continuum/fast-syntax-highlighting).
- Подсказки для команд по <kbd>TAB</kbd>.
- Тема командной строки [Powerlevel10k](https://github.com/romkatv/powerlevel10k).
- Кое-что из [OhMyZsh](https://github.com/ohmyzsh/ohmyzsh).
- Отдельная папка для кастомных плагинов.
- Много [полезных плагинов](https://github.com/unixorn/awesome-zsh-plugins).
- Многое другое без компромисса со скоростью шелла. :rocket:

## Что изменено в форке

- Загрузка автодополнений для `yc-cli` через Antidote (требует доработки).
- Тема командной строки изменена с *Pure* на *Powerlevel10k*.
- Возвращены потерянные кейбинды для листания истории ввода по части команды (для плагина *History Substring Search*). Снова можно использовать клавиши <kbd>↑</kbd> и <kbd>↓</kbd>. Секция `# Keybindings` размещена в файле `.zshrc`.
- Возможно, что-то ещё.

---

## Известные проблемы

#### Ошибка, связанная с `yc-cli` и модулем автодополнения для Zsh.

После обновления `yc-cli` при старте шелла появляется сообщение:
```
/Users/$USER/.config/zsh/.zsh_plugins.zsh:source:39: no such file or directory: 
/Users/$USER/.config/zsh/custom/plugins/yc-cli.completion.plugin.zsh
```
Это связано с костыльным решением, которое добавляет автодополнения для `yc-cli` через Antidote. Модуль, который поставляется в составе пакета `yc-cli`, нельзя напрямую подключить к Antidote. Поэтому для него мы создаем символьную ссылку с изменённым именем в директории для кастомных плагинов. Однако, так как в пути до модуля папка имеет название номера версии, после обновления путь к модулю изменяется, а символьная ссылка ведёт к отсутствующему файлу.

После обновления `yc-cli` требуется: 
1. Удалить сломанную символьную ссылку:
```
rm $ZDOTDIR/custom/plugins/yc-cli.completion.plugin.zsh
```
2. Выяснить версию `yc-cli`:
```
ls /usr/local/Caskroom/yandex-cloud-cli/
```
3. Создать новую символьную ссылку:
```
ln -s /usr/local/Caskroom/yandex-cloud-cli/<ВЕРСИЯ yc-cli>/yandex-cloud-cli/completion.zsh.inc \
$ZDOTDIR/custom/plugins/yc-cli.completion.plugin.zsh
```

---

## Установка
#### Для MacOS с установленным из Homebrew `yc-cli`

1. Клонировать репозиторий.
```
ZDOTDIR=~/.config/zsh

git clone --branch refining https://github.com/eximoelle/my-zdotdir $ZDOTDIR
```

2. Выяснить версию `yc-cli`.
```
ls /usr/local/Caskroom/yandex-cloud-cli/
```

3. Создать символьную ссылку:
```
ln -s /usr/local/Caskroom/yandex-cloud-cli/<ВЕРСИЯ yc-cli>/yandex-cloud-cli/completion.zsh.inc \
$ZDOTDIR/custom/plugins/yc-cli.completion.plugin.zsh
```

4. Чтобы включить установленный конфиг, нужно создать символьную ссылку на `.zshenv` в корне домашней директории. Если файл `.zshenv` уже существует — будет сделан бэкап.
```
[[ -f ~/.zshenv ]] && mv -f ~/.zshenv ~/.zshenv.bak
ln -s $ZDOTDIR/.zshenv ~/.zshenv
```

5. Запуск новой сессии Zsh. Введите `zsh` или откройте новое окно терминала.

#### Для MacOS без `yc-cli`

1. Используйте инструкцию выше, пропуская шаги 2-3, при этом после шага 1 откройте файл `$ZDOTDIR/.zsh_plugins.txt` и закомментируйте строку 216 `$ZDOTDIR/custom/plugins/yc-cli.completion.plugin.zsh`, отключив тем самым загрузку модуля автодополнения для `yc-cli`.

---

\[Antidote]: https://getantidote.github.io
