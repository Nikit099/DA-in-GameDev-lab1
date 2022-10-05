# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Нечитайлов Никита Михайлович
- ХПИ31
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
Ознакомиться с основными операторами зыка Python на примере реализации линейной регрессии.

## Задание 1
### Связать google sheets, python, unity с помощью google cloud
Ход работы:
- Создать ключ в google cloud
![image](https://user-images.githubusercontent.com/94524541/194006868-8ab2b2a7-6109-4bb9-926f-aaffa016db98.png)

- Написать код, который подсоединяется к api таблиц и выводит рандомны числа с подсчетом инфляции
![image](https://user-images.githubusercontent.com/94524541/194007049-301b6a79-e2ff-4bfa-9518-d6a634bb87fe.png)

![image](https://user-images.githubusercontent.com/94524541/194007074-e593f012-2a37-4444-b33d-ef6b243820af.png)

- Теперь необходимо подсоединить таблицу с unity, написав код
![image](https://user-images.githubusercontent.com/94524541/194007419-e390385a-f99c-4462-882e-b4d0903100eb.png)

- На выходе мы получаем вывод в консоль данные с нужным нам звуком
![image](https://user-images.githubusercontent.com/94524541/194007566-18551de2-a1a1-4ef6-9f95-c6c08fa1f2ec.png)



## Задание 2
### Необходимо связать данные из ленейной регрессии с кодом и вывести loss в таблицу


- Вложить в цикл создание loss и вывести в таблицу
![image](https://user-images.githubusercontent.com/94524541/194008346-a2dac52d-8955-4708-9b29-3d8d65d27c9f.png)

![image](https://user-images.githubusercontent.com/94524541/194008367-d27053af-fba4-489c-bd67-fdfe92b08658.png)


## Задание 3
### Подсоединить данные из loss с unity

- Изменяем диапазон данных в коде, чтобы одни подходили под диапазон loss
- Изменяем количество колонок
![image](https://user-images.githubusercontent.com/94524541/194008714-9449d602-db47-4627-b5c6-cb2ddfbd930a.png)

![image](https://user-images.githubusercontent.com/94524541/194008730-061f730e-0f82-4896-a509-5277e9d4cfc4.png)


## Выводы

Я выяснил, что можно выводить звук в unity в зависимости от диапазона данных. Можно подсоединять гугл таблицу к коду и записывать в них значения. Можно так же эту таблицу можно связать с unity. 

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
