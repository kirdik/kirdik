
---
title: "Проблема с отображением времени в старых mp3 файлах"
date: 2026-01-13T00:13:57+03:00
draft: false

author: "Кирилл В. Чеботарёв"
tags: ["mp3"]
categories: ["Звук"]

description: "Поллема с отображением времени в mp3"
canonicalURL: "https://arch.folkcentr.ru/all/problema-s-otobrazheniem-vremeni-v-staryh-mp3-faylah/"

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

<!--more-->
Те кто работают с архивами и старыми оцифрованными записями периодически могут встретить такую неприятную вещь, как некорректное отображение общего звучания файла и некорректное отображение времени воспроизведения. Это мешает правильно нарезать файл.  
То есть вы слушаете в проигрывателе какую-то длинную запись, выписываете себе начало песни на 00:35, а при открытии в аудио редакторе оказывается, что метка сместилась.  
Эта проблема связана с кодеком [MP3Pro](https://ru.wikipedia.org/wiki/Mp3PRO) до версии 0.93 особенно при кодировании в переменный битрейт VBR. В последующий версиях проблема была решена, но такие файлы периодически еще могут встречаться.  
Решается всё перекодированием файла. Теоретически это снижает качество, для перестраховки можно перекодировать в постоянный битрейт.