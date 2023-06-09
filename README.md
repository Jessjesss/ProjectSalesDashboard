# Дашборд по продажам сети магазинов

## Цель проекта:

Компании необходимо понять, где лучше всего открыть новую точку, какие магазины перегружены заказами, в каких районах больше всего продаж, какие клиенты приносят наибольшую прибыль, какие товары являются ключевыми. 

В результате должен быть разработан дашборд, который наглядно покажет всю картину и ответит на эти вопросы для принятия бизнес-решений.

## Задачи:

1. Анализ данных о продажах из БД за 1 год.
2. Аналитика клиентов.
3. Визуализация геоданных.
4. ABC-анализ товаров.

## Процесс выполнения проекта:

Все данные хранятся в базе данных Clickhouse.

Я создала подключение, используя встроенный коннектор:

![img](https://github.com/Jessjesss/Project_SalesDashboard/blob/master/img/1.png)

К таблице **MS_SalesFacts присоединила**:

**MS_Shops,**

**MS_Products,**

**MS_Clients**.

![img](https://github.com/Jessjesss/Project_SalesDashboard/blob/master/img/2.png)

Переименовала названия полей с английского языка на русский и агрегировала нужные данные (сумма, количество уникальных)

Создала новые поля, используя функции DataLens:

| Название поля | Функция |
| --- | --- |
| Дата заказа - месяц | datetrunc([Дата заказа ], "month") |
| Дата последнего заказа | MAX([Дата заказа ]) |
| Количество дней со дня последнего заказа | ([Дата последнего заказа]-DATE("2020-01-01"))- ([Дата последнего заказа]-DATE("2020-01-01"))*2 |
| Продажи прошлого года | AGO( SUM([Продажи]), [Datepart], "year" , 1) |
| Среднее количество заказов | [Продажи] / [Количество заказов] |
| Темп роста за год | SUM([Продажи])/AGO( SUM([Продажи]), [Datepart], "year" , 1) |
| Темп роста за месяц | SUM([Продажи])/AGO( SUM([Продажи]), [Дата заказа - месяц], "month" , 1) |
| Сравнение -продажи год назад | AGO([Продажи], [Дата заказа ], "year" , 1) |
| Группировка по категориям | SUM(SUM(int([RFM категории])) TOTAL) |
| Rank Продаж | RANK(SUM([Продажи]), "desc") |

Для визуализации геоданных я использовала географические координаты из таблицы **MS_Shops** , с помощью встроенных функций Yandex DataLens **** изменила тип данных на: “геополигон” и “геоточка”. Создала чарт: “Магазины на карте”.

![img](https://github.com/Jessjesss/Project_SalesDashboard/blob/master/img/3.png)

В результате разработки я создала 22 чарта, 6 селекторов, 2 страницы (продажи/клиенты). 

Готовый дашборд доступен по ссылке: https://datalens.yandex/wragy4w4mu1km:

Выполненные задачи:
1. Анализ данных о продажах: дашборд предоставляет информацию о продажах во всех точках сети, а также позволяет определить наиболее прибыльные магазины и районы. 
2. Анализ клиентов: Благодаря проведенному анализу клиентов, было возможно классифицировать клиентов с помощью RFM-анализа и определить, какие клиенты приносят наибольшую прибыль. Это позволит компании сосредоточить усилия на наиболее ценных клиентах и разработать меры для удержания и привлечения новых клиентов.
3. Визуализация геоданных: с ее помощью компания может увидеть, в каких районах больше всего продаж и определить перегруженные точки, а так же принять решение об открытии нового магазина в наиболее перспективном месте.
4. ABC-анализ товаров: Был проведен ABC-анализ товаров, который позволит выделить наиболее важные товары с точки зрения прибыльности. Это поможет компании оптимизировать управление запасами и сконцентрировать усилия на продаже ключевых товаров.

![img](https://github.com/Jessjesss/Project_SalesDashboard/blob/master/img/4.png)

![img](https://github.com/Jessjesss/Project_SalesDashboard/blob/master/img/5.png)
