# Pozorniki LLC
<img src="/pozorniki.jpeg" alt="project"/>

<b>О нас:</b>
<br>Наша команда Pozorniki LLC работает на рынке аналитики недвижимости уже около 130 лет.
<br>Сигнатурный подход к аналитике является очень клиенто-ориентирован и затрагивает оценку даже самых сложных показатей, таких как кол-во туалетов на кол-во комнат 
<br>
<b>Пошаговое выполнение проекта:</b>

<b>1. Разведочный анализ.</b> 
<br> Для начала переименовали столбцы на латинские названия для избежания потенциальных ошибок и убрали все города, не входящие в поле анализа, оставив только Москву.
<br> Посмотрели общую информацию в таблице, типы данных в столбцах, проверили данные на дупликаты и проанализировали заполненность столбцов.
<img src="/plot1.png" alt="project"/>

На основании этого мы анализируем столбцы с большим кол-ом пропусков на надобность в заполнении пустых ячеек:
1. Building Series & Building name - не нужны, никакой полезной информации они не несут.
2. Parking - потенциально важный показатель для анализа, пустые значения скорее всего означают отсутствие выделенной парковки и будут заполнены соответственным образом.
3. Ceiling height - возможно нужен. Пустые значения скорее всего просто не заполнялись при создании. Возможно для анализа не будем использовать, в ином случае заполним как 2.5м (стандратной высотой квартир по фильтрам Циана).
4. Trash - потенциально не показательная характеристика, будет проверена на актуальность дополнительно.
5. Rooms_sqm - показатель, который не стоит учитывать в анализе, т.к. слишком много пустых значений, которые могут быть абсолютно разные и применять средние будет нерелевантно.
6. Balcony - может сильно влиять на цену. Пропуски потенциально означают остуствие балкона, которые стоит заполнить нулями
7. Window view - возможно нужный показатель, однако не понятно как заполнять как пропуски, показатель будет изучен дополнительно на следующих этапах
8. Kids_pets - не учитываем, т.к. единственный оставшийся вариант на Циане: "не важно", что никак не дает понять наверняка можно ли с детьми/животными или нельзя.
9. Elevator - не учитываем, т.к. даже в заполненных полях на одинаковых адресах присутствуют разные данные, что только ведут к некорректности
10. Style - качество ремонта явно является важным. Данные будут преобразованы в вид Дорогой/Не дорогой ремонт
11. Bathroom_count - важно, пустые заполним как 1, т.к. слабо представляется объявление на съем квартиры без санузлов.

Понимая, что по-хорошему на каждый дом показатели  наличия парковки, кол-ва лифтов и наличия мусоропровода должно быть одикаковым для всех размещений,
выведем кол-во уникальных значений по этим парамметрам для каждого адреса, чтобы понять насколько данные отражают действительность, учитывая, что данные заполнясь пользователями.
<img src="/plot2.png" alt="project"/>
<img src="/plot3.png" alt="project"/>

По полученным показателям можем сделать вывод, что данные по стобцам мусоропровода и лифтам лучше не использовать при анализе, т.к. слишком много адресов имеют разные значения. 
А данные по парковкам стоит возможно преобразовать в булевые значения - Есть парковка/Нет парковки. Но возможно просто заполним пропуски как "нет парковки".



<b>2. Очистка и преобразование входных данных.</b> 
<br>Приводим цены к одному порядку, переводим валюту в рубли и для удобства выводим в новый столбец, для дальшего анализа и построения графиком с числовой величиной, курс USD и EUR выгружаем через библиотеку xml.etree.
<br>ППриводим данные по площадям квартиры (данные приведены в формате общ.пл/жилая пл./пл. кухни. Нас интересует только общая площадь).
<br>ППриводим данные в числовые значения, считаем количество комнат для каждого объявления.
<br>ПАналогичным оьразом приводим данные в числовые значения по сан.узлам

<br>ПЗаполняем пропусти соответственно установленной логике при анализе величин.




 



<b>3. Feature engineering и глубокий анализ дополнительных характеристик.</b> 
<br>
Пишем функции и рассчитываем дополнительные величины:
1. Всего этажей в доме
2. Этаж квартиры
3. Является ли этаж первым, последним или ни тем ни другим
4. Доступность от метро в минутах пешком (для данных с минутам на авто был установлен коэффициент х4)
5. Есть ли в квартире Кондиционер
6. Считаем общее количество Балконов и Лоджий в квартире
7. Добавляем колонки Округ и Район согласно адресу
8. Проверяем хороший ли ремонт в квартире или косметический

<br>Благодаря поддержке партнеров ООО "Боевые Суслики":
<br>Производится кодировка данных в датасете для последующего машинного обучения.
Создается модель для обучения XGBOOST и расчитывается показатель MAPE (16.83%)

  

