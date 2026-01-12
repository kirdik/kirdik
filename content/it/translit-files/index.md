
---
title: "Скрипт рекурсивного переименования файлов в транслит"
date: 2026-01-13T00:39:51+03:00
draft: false

author: "Кирилл В. Чеботарёв"
tags: ["транслит", "кракозябры", "скрипт", "python", "bash"]
categories: ["Автоматизация"]

description: "Как переименовать много файлов в транслит"
canonicalURL: "https://arch.folkcentr.ru/all/skript-rekursivnogo-pereimenovaniya-faylov-v-translit/"

showToc: true
TocOpen: false
hidemeta: false
comments: false

disableHLJS: false
disableShare: false
hideSummary: false
searchHidden: false

ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true

cover:
  image: "cover.webp"
  alt: ""
  caption: ""
  relative: true
  hidden: false
---

<!-- Текст статьи начинается здесь.
Картинки можно просто перетаскивать в эту папку. -->
Иногда нужно переименовать большое количество файлов из кириллицы в транслит  
Любимая_песня_мамы.mp3 —> Lubimaja_pesnia_mami.mp3  
В контексте работы с архивом именование файлов латиницей очень важно по нескольким причинам.
<!--more-->
Простой пример: в некоторых случаях файл при публикации в интернете _Любимая_песня_мамы.mp3_ может преобразовываться вот в такое:  
_%D0%9B%D1%8E%D0%B1%D0%B8%D0%BC%D0%B0%D1%8F_%D0%BF%D0%B5%D1%81%D0%BD%D1%8F_%D0%BC%D0%B0%D0%BC%D1%8B.mp3_  
Это не удобно потому, что это нечитаемо, длинна названия увеличивается в три раза. 

Имея опыт работы с разными собирателями могу утверждать, что файлы называют и длиннее, умещая туда как минимум паспорт записи. Это тоже в некоторых случаях вызывает проблемы. Например при копировании на старых версиях Windows. Во всем виноваты устаревшие файловые системы и протоколы. Так же проблема с кириллицей может возникнуть в случае если ваш жесткий диск перестал работать и из него необходимо вытащить специальными программами информацию. Разработчики таких программ мало заботятся о странах не использующих латиницу. Восстановить имена файлов может и не получиться.

## Конвертация

Когда файлов три или пять, это не беда, но если у вас их полторы тысячи и все он лежат в разных папках, то тут нужен скрипт. Я нашел такой в интернете, к сожалению не помню где, но если найду автора, то оставлю ссылку конечно.  
Скрипт работает только в linux и других *nix системах (напрмер в MacOS)  
Копируем текст в файл **rename.sh** 
Даем права файлу на выполнение **chmod u+x** 
Скрипт переименовывает все подряд в том каталоге в котором находится.  
Запуск **./rename.sh**

```
#!/bin/bash
# Перекодирует рекурсивно в текущем каталоге имена
# файлов и каталогов в транслит.

# shopt встроенная команда оболочки. Управляет опциями оболочки.
# Если в каталоге нет ни одного файла, соответствующего шаблону,
# то за имя файла принимается сам шаблон.
# Ключ nullglob исправляет эту ситуацию
shopt -s nullglob
# Перебираем все файлы в текущем каталоге
for NAME in * ; do
# sed-ом заменяем символы кирилицы на символы латиницы
 TRS=`echo $NAME | sed "y/абвгдезийклмнопрстуфхцы/abvgdezijklmnoprstufxcy/"`
 TRS=`echo $TRS | sed "y/АБВГДЕЗИЙКЛМНОПРСТУФХЦЫ/ABVGDEZIJKLMNOPRSTUFXCY/"`
 TRS=${TRS//ч/ch};
 TRS=${TRS//Ч/CH} TRS=${TRS//ш/sh};
 TRS=${TRS//Ш/SH} TRS=${TRS//ё/jo};
 TRS=${TRS//Ё/JO} TRS=${TRS//ж/zh};
 TRS=${TRS//Ж/ZH} TRS=${TRS//щ/sh\'};
 TRS=${TRS///SH\'} TRS=${TRS//э/je};
 TRS=${TRS//Э/JE} TRS=${TRS//ю/ju};
 TRS=${TRS//Ю/JU} TRS=${TRS//я/ja};
 TRS=${TRS//Я/JA} TRS=${TRS//ъ/\`};
 TRS=${TRS//ъ\`} TRS=${TRS//ь/\'};
 TRS=${TRS//Ь/\'}
 TRS=${TRS// /_}
 # переименовываем
 mv -v "$NAME" "$TRS"
# Если это каталог, заходим в него
 if [[ `file -b "$TRS"` == directory ]]; then
 cd "$TRS"
 "$0"
 cd ..
 fi
done
```

Обновлённая версия переписанная на Python, подходит и для Windows, но нуждается в [установке Python на компьютер](https://www.python.org/downloads/).

Как пользоваться:
1)  Копируем код в текстовый фал и называем его translit.py
2) Открываем командную строку и переходим в нужный каталог
3)  Запускаем скрипт ``` python translit.py ```

```
#!/usr/bin/env python3
import os

# Таблица посимвольной замены (аналог sed y///)
TRANSLIT_MAP = str.maketrans({
    "а": "a",  "б": "b",  "в": "v",  "г": "g",  "д": "d",
    "е": "e",  "з": "z",  "и": "i",  "й": "j",  "к": "k",
    "л": "l",  "м": "m",  "н": "n",  "о": "o",  "п": "p",
    "р": "r",  "с": "s",  "т": "t",  "у": "u",  "ф": "f",
    "х": "x",  "ц": "c",  "ы": "y",

    "А": "A",  "Б": "B",  "В": "V",  "Г": "G",  "Д": "D",
    "Е": "E",  "З": "Z",  "И": "I",  "Й": "J",  "К": "K",
    "Л": "L",  "М": "M",  "Н": "N",  "О": "O",  "П": "P",
    "Р": "R",  "С": "S",  "Т": "T",  "У": "U",  "Ф": "F",
    "Х": "X",  "Ц": "C",  "Ы": "Y",
})

# Многосимвольные замены (порядок важен!)
MULTI_REPLACE = [
    ("ч", "ch"), ("Ч", "CH"),
    ("ш", "sh"), ("Ш", "SH"),
    ("ё", "jo"), ("Ё", "JO"),
    ("ж", "zh"), ("Ж", "ZH"),
    ("щ", "sh'"), ("Щ", "SH'"),
    ("э", "je"), ("Э", "JE"),
    ("ю", "ju"), ("Ю", "JU"),
    ("я", "ja"), ("Я", "JA"),
    ("ъ", "`"),  ("ь", "'"),
    ("Ь", "'"),
    (" ", "_"),
]

def translit(name: str) -> str:
    result = name.translate(TRANSLIT_MAP)
    for src, dst in MULTI_REPLACE:
        result = result.replace(src, dst)
    return result

def process_directory(path: str):
    # Сначала переименовываем файлы и каталоги
    entries = os.listdir(path)

    for name in entries:
        old_path = os.path.join(path, name)
        new_name = translit(name)
        new_path = os.path.join(path, new_name)

        if name != new_name:
            # защита от конфликтов имён
            if not os.path.exists(new_path):
                print(f"{old_path} -> {new_path}")
                os.rename(old_path, new_path)
            else:
                print(f"⚠ пропуск (уже существует): {new_path}")
        else:
            new_path = old_path

        # Если каталог — заходим внутрь
        if os.path.isdir(new_path):
            process_directory(new_path)

if __name__ == "__main__":
    process_directory(os.getcwd())

```