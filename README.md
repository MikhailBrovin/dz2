#АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил:
- Тарасов Денис Дмитриевич
- ФО-220007
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:

- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Научиться передавать в Unity данные из Google Sheets с помощью Python.
## Ход работы:
## Задание 1 Выберите одну из компьютерных игр, приведите скриншот её геймплея и краткое описание концепта игры. Выберите одну из игровых переменных в игре (ресурсы, внутри игровая валюта, здоровье персонажей и т.д.), опишите её роль в игре, условия изменения / появления и диапазон допустимых значений. Постройте схему экономической модели в игре и укажите место выбранного ресурса в ней.
### Genshin Impact (кит. 原神 Yuánshén) — бесплатная ролевая игра в жанре action-adventure, разработанная и изданная miHoYo Co., Ltd. За пределами Китая издателем является дочерняя компания miHoYo Cognosphere Pte., Ltd. d/b/a HoYoverse.
В игре есть фэнтезийный открытый мир и боевая система, основанная на использовании магии элементов, переключении персонажей и системы монетизации «гатя». В игру можно играть только с подключением к Интернету, и она имеет ограниченный режим совместной игры, позволяющий играть до четырёх игроков в мире.

Переменной я решил выбрать шанс выпадения 5 звездного персонажа в геншин импакт, на самом деле, все намного сложнее, чем описано у меня в коде, но так как целью является ознакомиться с самим концептом данных методов, то мне не кажестя это грубой ошибкой. Так что с каждой новой круткой я прибавлял 0,01 процент к шансу выпадения персонажа или предмета. Также известно, что за 100 прокруток, точно выпадает 5 звездочный персонаж. 
Скриншот:![637bbafc46787c2c683546824049ff7b](https://github.com/MikhailBrovin/dz2/assets/146543564/9fca7862-f5b1-4437-8a5c-5eb6b2ffdbcd)

Экономическая модель:![2023-10-01_20-59-01](https://github.com/MikhailBrovin/dz2/assets/146543564/968065b9-12a0-4074-b365-d36bc5e40c0e)

## Задание 2
### С помощью скрипта на языке Python заполните google-таблицу данными, описывающими выбранную игровую переменную в выбранной игре (в качестве таких переменных может выступать игровая валюта, ресурсы, здоровье и т.д.). Средствами google-sheets визуализируйте данные в google-таблице (постройте график, диаграмму и пр.) для наглядного представления выбранной игровой величины.
```py

import gspread
import random
import numpy as np
gc = gspread.service_account(filename='unitydatascience-400614-72a1d5be7975.json')
sh = gc.open("Homework2")
mon = list(range(1,100))
success = False
count = 0
chance = 1

while not success:
    sh.sheet1.clear()
    count += 1
    if random.random() < chance/100:
        success = True
    sh.sheet1.append_row([count, chance/100])
    chance += 1
    
print("It took", count, "tries to get at least one success.")
```
Результат: It took 8 tries to get at least one success.

-Ссылка на гугл таблицу https://docs.google.com/spreadsheets/d/1OXSm279BWmZqvQSIAJn7Q_ox-OlH3nsyPfYlKqweCS8/edit#gid=0 

Средствами google-sheets визуализиция данных в google-таблице(график зависимости шанса выпадения от количества круток)![2023-10-01_21-19-51](https://github.com/MikhailBrovin/dz2/assets/146543564/17da7172-975d-44af-a2df-b547897da6d7)

## Задание 3. Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.

В данном задании я решил воспроизвести звуки выпадения звездочного персонажа, на юнити, с помощью этого кода юнити считывал гугл таблицу и воспроизводил нужный звук, а именно звук выпадения 5 звездочного персонажа или обычную прокрутку.
{

    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;
    using UnityEngine.Networking;
    using SimpleJSON;
    
    public class NewBehaviourScript : MonoBehaviour

    public AudioClip goodSpeak;
    public AudioClip badSpeak;
    private AudioSource selectAudio;
    private Dictionary<string,float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i = 1;

    // Start is called before the first frame update
    void Start()
    {
        StartCoroutine(GoogleSheets());
    }

    // Update is called once per frame
    void Update()
    {
        if (i > dataSet.Count) return;
        
        if (dataSet["Res_" + i.ToString()] >= 100 & statusStart == false & i <= dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] >= 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://docs.google.com/spreadsheets/d/1OXSm279BWmZqvQSIAJn7Q_ox-OlH3nsyPfYlKqweCS8/edit#gid=0/Лист1?key=AIzaSyBYZL4JWVCTZTo25QUYE2FbUS7BrpTOwhU");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
        }
    }

    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}

## Выводы
Абзац умных слов о том, что было сделано и что было узнано.
Я научился передавать данные из Python в Google sheets, также я еще больше познакомился с основами программирования, хоть все выполнить не удалось, научился средствами google-sheets визуализировать данные в google-таблице.
| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
