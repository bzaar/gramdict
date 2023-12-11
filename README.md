# «Грамматический словарь» А. А. Зализняка

Этот репозиторий содержит данные «Грамматического словаря» А. А. Зализняка, основанные на гранках шестого издания (2010 года), в виде текстовых файлов.

«Грамматический словарь» (далее «Словарь», «ГС») описывает склонение и спряжение около 110 тысяч слов, включая имена собственные.

Данные предоставлены правообладателем словаря Анной Андреевной Зализняк и опубликованы с ее согласия под лицензией CC BY-NC.

> Сайт [gramdict.ru](https://gramdict.ru) содержит эти же данные и предоставляет удобный интерфейс для поиска и фильтрации по знакам и пометам.

## Структура каталогов и файлов

Основной массив статей поделен на несколько десятков текстовых файлов [А.txt](dictionary/Нарицательные/А.txt), [Б.txt](dictionary/Нарицательные/Б.txt), ... [Я.txt](dictionary/Нарицательные/Я.txt), названных по _последней_ букве входящих в файл слов. Такое деление сделано для того, чтобы файлы можно было редактировать через веб-интерфейс Гитхаба, который накладывает ограничение в 1М на размер редактируемых файлов.

Слова на букву Ё находятся в файле Е.txt (по аналогии с оригинальным словарем).

Приложение 1 «Имена собственные» идет отдельным файлом ```Собственные.txt```.

## Формат текстовых файлов

В целом формат текстовых файлов максимально приближен к оригиналу: порядок статей внутри каждого файла [инверсионный][1], структура словарной статьи и значения специальных знаков и помет – [как в оригинальном словаре][2].

[1]: https://gramdict.ru/preface1#inverse-order
[2]: https://gramdict.ru/howtouse

Тестовые файлы используют знаки подчеркивания для передачи курсива и жирного шрифта, например: 

```
па́почка ж 3*a (_уменьш. к_ 1/па́пка)
па́почка мо <жо 3*a> (_уменьш. к_ 2/па́пка)
засты́ть св нп 15a [//__засты́нуть__] ◑III
```

В оригинальном словаре жирным шрифтом выделены также леммы (заголовочные слова статей). В файлах это выделение опущено для простоты.

В оригинальном словаре каждая статья разделена на три колонки. Это разделение никак не отражено в текстовых файлах.

Словарь строго различает буквы Е и Ё в леммах.


## Учет ошибок

Ошибки делятся на два вида: 

1. расхождения между оригинальным изданием 2010 года и веткой master (ошибки OCR, опечатки); такие ошибки исправляются коммитами в ветку `master` ([история](https://github.com/gramdict/zalizniak-2010/commits/master))
2. несоответствия издания 2010 года другим словарям, в том числе более ранним изданиям «Грамматического словаря»; например, в издании 2010 года в статье __гнести__ [есть лишняя помета ⑨](https://github.com/gramdict/zalizniak-2010/issues/5), которой нет в издании 1987 года; такие ошибки исправляются коммитами в ветку `corrected`. 

Все найденные в оригинальном издании 2010 года ошибки можно найти на [этой странице](https://github.com/gramdict/zalizniak-2010/compare/corrected#diff).


## Цели проекта

Главная цель проекта – создать эталонную версию «Грамматического словаря» (ГС), которой удобно пользоваться как лингвистам, так и программистам.

Лингвистам (точнее, лексикографам) удобна система индексов, придуманная Зализняком, когда для полного и точного описания всех 120 форм глагола в большинстве случаев достаточно написать индекс вроде «1a». В более редких случаях используются знаки и пометы, сигнализирующие о регулярных отклонениях. В совсем редких случаях в статьях фигурируют словесные пометы, что делает задачу парсинга словаря довольно нетривиальной.

Программистам же удобнее работать с однородными структурами, в которых как можно меньше особых случаев. Поэтому одна из подцелей проекта – создание программных модулей для парсинга Словаря, необходимых для построения словоформ из статей ([#6](https://github.com/gramdict/zalizniak-2010/issues/6)) и других задач, например, доказательства гипотезы комплементарности распределения схем ударения `/c` и  `/c''` в глаголах ([#22](https://github.com/gramdict/zalizniak-2010/issues/22)).

При этом не хочется повторять ошибку, которую допустили многие создатели «клонов» Словаря: как правило, использование словаря начинается с его преобразования в «более удобный» вид (список словоформ или в альтернативный набор парадигм). Ввиду сложности формата ГС, такие преобразования нередко вносят в результат некоторые погрешности, а то и сознательно опускают часть информации из оригинального словаря, например, информацию об ударении или о [затрудненности](https://gramdict.ru/preface1#difficulty) тех или иных форм. 

Это еще полбеды. Настоящая беда начинается тогда, когда авторы «клонов» начинают править в своих версиях _ошибки преобразования_, вместо того, чтобы исправить _само преобразование_, исходный код которого, как водится, уже давно утерян. Примеры:

* [В словаре OpenCorpora восстанавливают информацию о затрудненности.](https://github.com/OpenCorpora/opencorpora/issues/896#issuecomment-699509417)
* [В словаре АОТ пытаются исправить ударения – задача практически неразрешимая ввиду огромного объема словоформ.](https://github.com/sokirko74/aot/issues/2)

Вся эта информация есть в исходном словаре. Если бы было «живо» преобразование, людям не пришлось бы заново выполнять эту работу, однажды проделанную автором словаря.

Поэтому ваш покорный слуга решил «вернуться к истокам» и «начать все с начала». Тем более, что есть опыт работы со Словарем, есть видение, как «сделать всё правильно» и как избежать ненужных ошибок. С вашей помощью.

## Как помочь проекту: исправление ошибок

Создание словаря – огромный ручной труд и без ошибок здесь не обойтись. Учитывая, какими путями были получены живущие в этом репозитории текстовые файлы (ручной набор в Ворде, OCR), они просто обязаны содержать ошибки.

### Как создать pull request

Если вы нашли ошибку и хотите ее исправить, откройте нужный файл (например, [dictionary/Глаголы/Й.txt](dictionary/Глаголы/Й.txt)) и нажмите значок «карандаш» (Edit this file / Редактировать этот файл) в правом-верхнем углу.

Закончив редактирование, нажмите на зеленую кнопку **Propose changes** внизу. Затем нажмите **Create pull request**. Введите описание предлагаемых изменений и нажмите еще одну кнопку **Create pull request**. 

В этот момент модераторы получат уведомление о новом пул-реквесте и любой желающий сможет поучаствовать в обсуждении предлагаемых изменений. При достижении консенсуса PR будет влит в основной репозиторий.

Если у вас есть замечания или предложения общего характера, [заведите issue](https://github.com/gramdict/zalizniak-2010/issues/new).

Участвуя в проекте, вы соглашаетесь с тем, что ваш вклад в проект (вносимые изменения, предложения) автоматически передается в общественное достояние.

## История появления этих файлов

Шестое издание словаря вышло в 2010 году. Его гранки были подготовлены [Еленой Александровной Гришиной](http://www.ruslang.ru/node/986).

В 2017 году Сергей Слепов приобрел неисключительную лицензию на использование словаря и получил гранки в виде файла MS Word. Файл был преобразован в текстовый вид, как в этом репозитории, с помощью [этой программы](https://github.com/gramdict/docx2html). Приложение&nbsp;1 «Имена собственные» было оцифровано при помощи ABBYY FineReader с последующей вычиткой.

15.03.2021 получено разрешение правообладателя на публикацию материалов шестого издания.


## Лицензирование

Исключительные права на Словарь, включая файлы в каталоге [dictionary](dictionary) и любое другое, полное или частичное, воспроизведение Словаря, принадлежат [Анне Андреевне Зализняк](https://w.wiki/37EL).

С ведома и при поддержке правообладателя словарь распространяется под лицензией CC BY-NC. Это означает, что вы можете использовать его бесплатно в некоммерческих целях. Если вам удалось заработать деньги на словаре, прекрасно; не забудьте получить коммерческую лицензию, написав на human@gramdict.ru. 

### История вопроса о лицензировании

За более чем 40 лет, минувших с первого издания Словаря, он послужил основной многих полезных программ, таких как поиск «Яндекса», «ОРФО», встроенного в Microsoft Word, и многих других.

Мы считаем, что потенциал «Грамматического словаря» далеко не исчерпан и он может вдохновить еще много интересных проектов и идей.

Долгое время препятствием к использованию Словаря был его неясный правовой статус. С одной стороны, Словарь и его производные можно найти в открытом доступе на десятках, если не сотнях сайтов. С другой стороны, каждому понятно, что любое произведение охраняется законом и использовать его можно только с разрешения правообладателя.

Теперь все, кто использует словарь для некоммерческих целей, могут вздохнуть свободно: при любых притязаниях вы можете сослаться на данный текст и лицензию ([LICENSE.txt](LICENSE.txt)).

Те, кто зарабатывает на Словаре, теперь получили официальный канал для лицензирования: напишите нам на human@gramdict.ru для получения коммерческой лицензии. Лицензия нужна, даже если вы используете производные Словаря, такие как [словарь АОТ][aot] или [словарь OpenCorpora][opencorpora], или программы на его основе, такие как [pymorphy2] или [mystem].

[aot]: https://github.com/sokirko74/aot
[opencorpora]: http://opencorpora.org/dict.php
[pymorphy2]: https://pymorphy2.readthedocs.io/en/stable/
[mystem]: https://yandex.ru/dev/mystem/
