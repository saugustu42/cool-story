# Проверки в школе 21. Лекция Хирьянова про ub и не только.

Я давно читаю чат телеги четвертой волны и знаю, что к сожалению, качество огромного количества проверок оставляет желать лучшего. Ещё с мая я знаю, что как бы ты не написал либу, пиры всегда могут завалить тебя. 
Имплементировал в первой части поведение системных функций - лови неуд за сегу (при входящем NULL) от пира, который не понимает концепцию undefined behaviour.
Защитил функции не только второй части но и первой, чтобы оставаться последовательным? Лови неуд от другого пира, который тоже не понимает концепцию ub. Типа "а почему это при неопределенном поведении твои функции не ведут себя ОПРЕДЕЛЕННЫМ образом?". Да уж, действительно!

Я уже две недели как защитил свою либу и как бы, уже проскочил эти проверки и мог бы "забить" и пилить следующие проекты.
Но я не хочу чтобы в школе21 отсутствие компетенции пиров в теме ub становилось печальной традицией. 
Я хочу, чтобы мы проводили качественные проверки, которые предполагает методология ecole42. Чтобы мы внимательно проводили код ревью и обсуждали имплементации. Чтобы коллективный разум студентов прогрессировал с каждой проверкой, приближая всех нас к становлению грамотными инженерами computer science, а не к быдлокодерам. Я не хочу чтобы проверки "тебе неуд потому что unit test нарисовал тут красненьким" считались нормой. Не хочу чтобы мы неверно понимали техническую суть задания, и вместо беглого прогона чеклиста cub3d и далее обсуждения тонкостей реализации графических алгоритмов.. Пытались сломать человеку парсер наркоманскими импутами и довольные собой отправляли его на пересдачу.

Читая обсуждения, как проводят проверки либы на моей 6 волне, я увидел что в результате преемственности мы копируем ошибки волны предыдущей.

Желая изменить ситуацию, 19 ноября я написал обращение Тимофею Федоровичу Хирьянову с просьбой провести ликбез для студентов школы по теме undefined behaviour. 

Я написал герою - и он откликнулся


![alt text](khir.jpg "school-21-Hero")


Утром 20 ноября произошёл мой созвон с Тимофеем Федоровичем. Он поведал мне, что тема неопределенного поведения - большая, и что для проведения хорошей лекции ему нужно подготовиться. Также, он сказал мудрую вещь что не считает правильным разбирать как именно мы должны защищать/ не защищать свои функции с учётом требований сабжекта, ведь в нашем обучении должны сохраняться дискуссии. Слово за слово, Тимофей Федорович по инерции и по привычке начал читать мне лекцию, которую я с превеликим удовольствием слушал и задавал уточняющие вопросы. Прервался он лишь когда ему позвонили спустя час по срочному делу.
Я считаю что преступлением было бы после дарованной мне индивидуальной лекции, не сделать для пиров конспект услышанного. 

________________________________


Тимофей Федорович начал свой рассказ с того, что Си – низкоуровневый язык. Язык «опасный». Что несмотря на желание писать безопасные программы, на Си в полной мере сделать этого невозможно, потому что в нём нет соответствующих инструментов. В нём нет безопасности по доступу к переменным.
Если программа в процессе выполнения, выйдет за границы массива или за границы аллоцированной памяти, она может например повредить данные переменной, используемой этой же самой программой (например счетчик цикла). И дальнейшее поведение программы предсказать невозможно. В си нет типа данных string. То что мы называем строкой, по сути является тупо адресом, и по этому адресу аллоцировано сколько-то памяти. И неизвестно, сколько памяти там аллоцировано, это нигде не записано. Мы должны сами хранить такие данные в специально созданных переменных. А в более навороченных C++ с этих проблем нет, потому что в них есть std::string, который сам по себе следит за количеством аллоцированной памяти. Говоря языком ООП, std::string – класс, а в терминах си, по сути своей – структура. В стд стринг помимо таких переменных, как например количество аллоцированной памяти, инкапсулирована куча полезных методов и функций и механизмы реаллокации. std::string многое умеет, и делает это безопасно. 
А работать с памятью на си (через чары как в наших mem функциях) – это всё равно что ехать на танке. Едешь и сносишь всё на своем пути. Поэтому работать с памятью таким образом необходимо очень аккуратно, точно зная что ты делаешь. В си ты лёгким движением руки можешь реинтерпретировать память как какой хочешь тип и  записать куда угодно что угодно или считать что угодно. Кроме той ситуации, когда операционная система бьёт тебя за это по рукам. Когда операционка через прерывание процесса через контроллер памяти, отследив что программа полезла «не в свою» память, выдаёт Segmemtation fault. 
Есть места, в которых undefined behavior известно и ожидаемо. Выход за границы аллоцированной памяти, выход за границы массива..
Но также, ub встречается и в несколько неожиданных местах, например код
x = x++ + ++x. Так писать нельзя.
После этого примера, а также после того как я спросил «является ли поведение строковой функции после подачи NULL поинтера на вход, неопределенным поведением?», Тимофей Федорович ответил «не всегда». Я понял, что тема неопределенного поведения много обширней чем мне казалось. Что на те вопросы, которые я изначально ему сформулировал, лектору не удастся ответить тезисно ввиду объёмности темы.
Далее мы обсудили тему защиты функций. 
Функция, работающая со строками, не будет вести себя неопределённо, когда программист предполагал, что NULL-импут может поступить на вход и добавил в функцию проверку входящих данных.
Лектор поведал, что NULL поинтер – это ещё не самое худшее что мы можем пихнуть в функцию. А вот если ей дать какой-нибудь неопределённый указатель, то это уже намного хуже. NULL поинтер отличается тем, что ты можешь его проверить. NULL это штука известная, это «определённое никуда». 

Как альтернатива проверке принимаемых аргументов, есть такая вещь как «контракт». То есть когда ты пишешь какую-то функцию, есть набор входных данных и есть выходные данные. Для языка си есть оговорка, что мы можем return только что-то одно. Поэтому иногда выходные данные помещаются по адресу, на который указывает указатель, поданный тебе на вход. 
Но в любом случае, когда есть контракт, функция должна иметь право на отказ выполнять работу. 
Суть в том, что интерфейс входных и выходных данных должен быть
формально зафиксирован, то есть должны быть чёткие требования, какие инпуты должны придти на вход функции чтобы она начала работу, а в каком случае функция имеет право отказать в выполнении своей деятельности в зависимости от значений поданных параметров. Типы проверятся компилятором автоматически.
В контракте функции может быть написано, что адрес не имеет права быть NULL, он обязан быть валидным. И, например, дополнительно мы можем наложить требования, что по этому адресу должно быть аллоцировано не менее чем (сколько-то).
Я позволил себе перебить повествование Тимофея Федоровича, напомнив, что в нашей методике обучения присутствует большое количество запретов: функций, макросов.. одна норма чего стоит. И что учитывая эти ограничения, я не уверен, что есть возможность запилить такой контракт в наши учебные проекты (по крайней мере начальные).  Учитель продемонстрировал знание факта о наших ограничениях и дал свой комментарий, что эти ограничения – это одна из фишек нашего учебного трека. Продолжив, он отметил, что необязательно эти контракты писать в теле функций. Документацию к функции (в формате комментария) можно написать над функцией. 

/*
** norminette really allows us
** to write comments like this
*/

Если эта функция описывается в твоей самописной библиотеке, тогда её описание должно находиться в заголовочном файле. В любом случае, есть такой механизм под названием doxygen, который умеет из сишных / плюсовых / и некоторых других файлов, вытаскивать  данные и автоматически генерировать  документацию. Самое главное, что пока твои комментарии находятся прямо в исходном коде, они остаются более менее актуальными. Потому что программист, изменяющий содержимое функции, должен обязательно обновить документацию. Иначе, если её не обновлять, то уже документация станет невалидной и весь смысл этих заявлений и требований контракта теряется.

От себя добавлю интересный пост про наши ограничения, написанный в телеге в начале ноября:

*“Сабджекты сабджектами, но полезно понимать, как в боевых проектах код пишут. И почему. И разделять всякий трэш, который мы делаем в рамках школьных проектов (потому что так в сабджекте написано) и то, как это всё используется в жизни. Фильтровать, и ни в коем случае не воспринимать школу как однозначный источник правильных практик и привычек.”*
							Sky

Но это уже следующий уровень, ребят. Хочется верить, что норму разрабатывали не дураки, и давайте на первых порах следовать ей, успокаивая себя, что в этом есть польза. А что нам ещё остаётся? )


Так вот, Тимофей Федорович сказал, что вне зависимости от учебной программы ecole42, глобально, в работе программиста:
В функции самое главное, чтобы её контракт был понятен. Как с ней можно, как с ней нельзя. Потому что undefined behavior возникает в первую очередь от недопонимания программистов, использующих функцию. Когда человек пытается воспользоваться функцией не так, как надо, и он ожидает от неё какого-то другого поведения.

С неопределенным поведением, помимо очевидных кейсов его возникновения, бывают и подлые моменты, про которые просто надо знать. В частности, когда мы работаем с сишными строками, то нам приходится иногда эмулировать поведение std::string из плюсов хотя бы в том, что за каждой строкой нужно по-хорошему тащить её длину. Мы можем сделать в си структуру struct my_str, где лежит указатель на строковый буфер char *buf, аллоцированный размер этого буфера, а также длину строки. Передавать длину строки нужно для того чтобы не тратить ресурсы на её перевычисление.

Соответственно, возникает следующий вопрос: а что если нам передадут «разломанный» экземпляр этой структуры? Например в структуре  указано что длина строки 100, аллоцированная память 1000000, а буфер будет NULL ptr, или вообще битый указатель? Возникает вопрос, а что нам в этом случае делать?
В первом варианте функция начинает быть параноиком. Принимая аргументы, она начинает их перепроверять. Каждый раз, когда она принимает на вход структуру, она всё перепроверяет: например пересчитывает длину строки. При этом функции могут подать на вход невалидный указатель buf в структуре. Функция хотя бы может сделать  проверку является ли буфер NULL ptr. Но если buf – это вообще невалидный указатель, то это плохо и от  segmentation fault функция никак не убережется. 
При этом, на эту паранойю мы потратим время, прямо пропорциональное длине строки. То есть для длинных строк, там где возникали асимптотические проблемы – там эти проблемы останутся. Из-за того что каждое действие со строкой требует перепрохождения её от начала до конца, из-за этого многие проблемы возникали в windows, когда ты корзину открываешь, а там 10000 файлов. И идёт strcat много раз для каждого файла и тормозит. Потому что по сути там квадратичное время от количества файлов. Требовалось чтобы открыть папку в какой-то винде, сейчас это наверняка исправили. Такое было не только в винде: много где встретились с этой проблемой с нультерминированными строками. Проблема связана со скоростью, с асимптотикой. 

Так вот, дело в том, что вот эта структура данных, которую ты создашь на чистом си, окажется в потенциально невалидном состоянии. И ты либо будешь каждый раз параноиком и будешь каждый раз перепроверять, всё ли там соответствует правде. (Но некоторые вещи ты и не сможешь проверить). Либо ты должен требовать гарантированной целостности такой структуры. А как её можно требовать? Например в документации. По сути в комментарии к функции. Но проблема вот в чём: «а как программист, который будет использовать эту написанную тобой функцию, будет её использовать? Станет ли читать документацию? Не всегда».

Но вся соль ситуации в том, что ОБА эти подхода – плохие. И паранойя с проверками значений импутов плоха. И заявления и требования «на бумаге» (документация) – тоже плохой вариант. Потому что люди не будут выполнять эти требования. 

Тимофей.Ф:   А теперь давай помечтаем. Представь, что эта структура my_str будет устроена таким образом, что тот кто ей пользуется, не сможет сделать её невалидной. Что длина строки, хранящаяся в структуре данных, автоматически перевычисляется в тот момент, когда мы удлиняем содержимое буфера в рамках аллоцированного размера.
saugustu:        Вы говорите о python?
Тимофей.Ф:   Нет! Но ты понимаешь, что такая автоматизация процесса – это очень удобно?
saugustu:        Безусловно!
Тимофей.Ф:   А как же нам добиться такой автоматизации? Более того, чтобы программист об этом почти не думал, чтобы он этого не замечал. При этом целостность структуры гарантирована. Тогда нет проблем с паранойей: не надо делать никаких проверок! Уж если тебе дали такую структуру my_str, то там всегда всё будет в порядке! И ты будешь опираться на неё с уверенностью. Согласен что здорово?
saugustu:         Конечно! Мы сейчас мечтаем, а как мы будем это реализовывать?
Тимофей.Ф:    Нет, подожди, давай ещё домечтаем. Ещё раз: паранойя с такой структурой - излишня. С программиста, который формирует эту структуру, снята нагрузка. Он может пользоваться этой строкой-структурой естественным образом, а она сама вычисляет длину и так далее. И остаётся всегда целостной, консистентной.
И последнее. Вот эта структура, о которой мы сейчас мечтаем, конечно же существует в реальности. Это std::string 
saugustu:         Как я понимаю, это на плюсах, а в си, соответственно такого нету
Тимофей.Ф:    Конечно нету! Понимаешь в чём дело: всё дело в применимости. Для чего мы используем язык программирования. Язык си используется для того, чтобы программировать микроконтроллеры. Какой-нибудь восьмибитный. Вся оперативная память на нём и постоянная память тоже на нём. Сможет ли мы на таком микроконтроллере реализовать структуры, которые занимаются самопроверкой? В которых есть функции с проверками контрактов. То есть если железка мелкая и у нее мало памяти, а её быстродействие 8МГц. Или 4Мгц. Для автоматизации микроволновки / кондиционера / автоматического шлагбаума, этого более чем достаточно. А более современные чипы уже выпускают со встроенным Wi-Fi. Соответственно его можно подцепить к локальной сети и он умеет как простейший веб-сервер откликаться на твои запросы.
saugustu:        Это Internet of things?
Тимофей.Ф:   По сути Arduino и подобные вещи – да, в конечном счёте интернет вещей, но это скорее локалка вещей, необязательно же прям к интернету подцепляться.
saugustu:         Позвольте уточнить: вы сказали что в таких чипах не используются сложные программные структуры. Это просто потому что у них памяти мало?
Тимофей.Ф:    Да. Но не только. Также у них невысокое быстродействие. Когда-то давно компьютеры были таковы, что у них не хватало вычислительных мощностей на «сборщик мусора». Это штука очень удобная, но она ресурсоемкая! Представь себе: после каждой операции он просматривает, не освободилась ли память. Для этого должны быть реализованы сложные алгоритмы. Захочешь ты например питоновский словарь или map из С++. Ассоциативный словарь, в котором ты по строке быстро находишь число или иное значение.
saugustu:          Хэш-таблица?
Тимофей.Ф:    Да, только map – это не хэш-таблица, а двоичное дерево поиска.
Но суть в другом. Во-первых, это довольно большие алгоритмы, то есть это большое количество кода, который машинными кодами должен быть прописан в сегменте кода. Соответственно, у тебя очень быстро заполняется пространство твоего микроконтроллера и у него остается мало памяти. Поэтому стандартная библиотека плюсов std с ее удобными возможностями, туда просто не влезет. И на старых компьютерах, подобные удобные вещи, с ростом ресурсов компьютеров, появлялись постепенно. Сейчас компьютеры очень скоростные, поэтому сейчас найти современный язык без сборщика мусора сложно. Строки и массивы, вектора, какие-то другие структуры данных, завернуты в приятные структуры, которые называются словом класс. Чем класс отличается от структуры: только тем, что он запрещает доступ к своему содержимому посторонним. В нём написано «private». А все публичные способы к нему обратиться, сделаны безопасно. Естественно, когда программист пишет класс, ему приходится его протестировать. Мало ли этот класс можно скомпрометировать, сунуть в какую-нибудь из функций, принадлежащих этому классу, сунуть такое, чтобы он всё-таки взял и испортил сам себе память. В этом смысле взламывать можно не только программы – можно взломать класс. Пользуясь чистым C, или C++ который имеет все те же самые инструменты, мы можем взять указатель на объект этого класса, сделать его void* или char* и ехать на танке и делать что хотим.
saugustu:         Я слышал цитату "Указатель на void* - это самое ужасное нарушение типобезопасности, которое можно придумать.", что это вообще кошмар и так делать никогда нельзя.
Тимофей.Ф:     Ну, это сказано довольно абстрактно. В чистом си для реализации многих вещей без него не обойтись. Здесь возникает некий уровень абстракции. Когда мы работаем на чистом си, мы очень многое можем. Ты как Нео смотришь на мир. Как Нео из Матрицы. И у тебя мир – это байты. Биты и байты. Пожалуйста бери их, вытаскивай, делай с ними битовое И / ИЛИ.  Чего хочешь твори. В рамках того, за что тебе матрица не даст по башке. В какой-то момент тебя выкинет операционная система, когда ты полезешь совсем в чужую память. А если уж ты сам являешься частью операционной системы.. Если ты и есть драйвер или если ты и есть ядро операционной системы, так самое то: как раз чистый си тебе в руки! Бери, влезай во внутренности, в мысли программ. В их сегмент кода, если тебе очень надо. Понятно?
saugustu:         Да, это как агент Смит!
Тимофей.Ф:    Да.
Мы немного посмеялись, и учитель продолжил.
Тимофей.Ф:     Язык си как раз предназначен как целевой язык. Сейчас он имеет свою нишу. Это во-первых, программирование микроконтроллеров. Его оттуда никто наверняка не изгонит. Нет, ну может какой-нибудь Rust в какой-то момент. Хотя сами микроконтроллеры делают всё более мощными, но и простые тоже остаются, они тоже никуда не деваются.
 saugustu:         Знаете, я как раз хотел у вас спросить когда вы говорили что «там используется си потому что памяти мало», и потом добавили про быстродействие.. Я хотел задать вопрос «а может быть с развитием чипов в итоге у них станет много ресурсов, и там будут на них работать какие-то сложные штуки? Отвечая самому себе, наверное все-таки нет, потому что это медленнее, чем чистый си?» 
Тимофей.Ф:     Нет, не в этом дело.
saugustu:           А в чём?
Тимофей.Ф:     Не слишком быстродействующие чипы никуда не денутся. Знаешь почему?
saugustu:           …
Тимофей.Ф:     Потому что есть такое понятие как «адекватность использования». У тебя в квартире есть посудомоечная машина?
saugustu:           Да
Тимофей.Ф:      Скажи, а ты когда-нибудь моешь чашку из-под чая руками?
saugustu:           Да
Тимофей.Ф:      А почему?? Из твоего ответа следует, что у тебя есть губка для посуды! А зачем она тебе, разве у тебя нет посудомоечной машины? Причина: адекватность использования! Нафига включать посудомойку ради одной чашки?
Или вот, смотри что у меня есть.
saugustu:           А, вижу, raspberry.
Тимофей.Ф:      Raspberry Pi. И он потребляет до трёх ампер. То есть его можно конечно питать батарейками, но их будет ненадолго хватать. Ноутбук на современных процессорах является более-менее энергоэффективным. Ты можешь вообще десктопный компьютер запитать от аккумулятора?
saugustu:           Трудно сказать.
Тимофей.Ф:      От источника бесперебойного питания сколько он выдерживает?
saugustu:            А, этот вопрос я изучал. Крайне мало. Пару часов?
Тимофей.Ф:       Это максимум. На самом деле нагруженный вычисляющий компьютер съест небольшой ИБП за 15 минут. Полчаса – это верх. То есть когда у тебя выключили электричество, и десктопный компьютер запитан от аккумулятора, то срочно нужно сохранять работу и выключать
saugustu:            Припоминаю, был у меня ИБП
Тимофей.Ф:       Ноутбуки –это уже энергоэффективные процессоры. Процессор специально подкрутили чтобы он работал не слишком быстро и поменьше потреблял. И он работаей от литий-йонного, относительно маленького аккумулятора. Работает часа 2-3-4. Уже разумное время, работает часы. А представь себе, можно ли запитать ноутбук от батареек? Он же потребляет сколько вольт после понижения, допустим 19 вольт. Набираем батарейками, десяток положили и запитали. Как ты думаешь, много он протянет?
saugustu:            Считаю, что мало
Тимофей.Ф:       (Смеясь)  Правильно считаешь! А в raspberry мы берём и понижаем мощность и скорость этого одноплатного маленького компьютера. Он построен на arm’овском процессоре, который сделан для смартфонов. Вот для смартфонов, производители бьются за энергоэффективность arm’овских процессоров чтобы батарейка не так быстро садилась. Соответственно подобный десктопный процессор сколько протянет на батарейках? Ну тут уже ничего так, он протянет уже не слишком мало. То есть можно запитаться батарейками или в power bank сунуть его, запитав от usb. Денёк можно поработать, и это круто! Но возникает вопрос: если тебе нужно поставить нечто умное в автоматический шлагбаум, чтобы ты мог по сотовому телефону позвонить и шлагбаум открылся. Ну, в Москве такие есть.
saugustu:           Видел такие, ага
Тимофей.Ф:      Или в микроволновке чтобы ты мог выбрать на индикаторе какое-то время, оно высветилось, отработало, и микроволновка выключилась. И вот вопрос «а что, разве туда мощный процессор надо ставить?» Интеловский? Amd? Nvidia какой-нибудь?
saugustu:           Ну понятно, что не нужно.
Тимофей.Ф:      Не нужно. Значит останутся жить самые простейшие микроконтроллеры. Они никуда не денутся. Вокруг нас микроконтроллеров полно. Возьмём какой-нибудь соединитель чего-нибудь с чем-нибудь. Вот у меня есть такой переходник, соединяющий телефон с клавиатурой. Usb-c на usb-a. Взял, вставил в телефон, вставил клавиатуру и поехали. И вот что ты думаешь внутри? 
saugustu:           Тоже чип какой-то?
Тимофей.Ф:      Да, там есть микроконтроллер. Ему нужно суметь всё это дело друг с другом садаптировать. То есть микроконтроллеры никуда не делись. Вопрос в том что он вшит и спрятан и тебе неудобно с ним работать, он слишком мелкий. Как резистор в 220 Ом. Недавно я попаял немножко маленькие резисторы, микросхемки такие. Как на материнке. Маленьким пинцетом под большой лупой. Резисторы малюсенькие 220 Ом выпускают. А зачем продолжают выпускать промышленно резисторы длиной в несколько сантиметров? Если 220 Ом можно сделать малюсенький? А? Зачем?
saugustu:          Кстати не доходит до меня.
Тимофей.Ф:     Да потому что не удобно маленькие паять! Вот есть такое понимание: адекватность и удобство. И микроконтроллеры никуда не денутся. И крупные «ножки», которые удобно паять вручную, потому что самопальное изготовление микроконтроллеров – это отрасль народного хозяйства. Люди продолжают печь блины у себя дома на плите, хотя могли бы уже давным-давно только покупать их в магазинах. И тем не менее, тесто продолжают месить и хлеб продолжают печь в духовке потому что хотят свои блины испечь, сделать свой каравай, а не покупать стандартный упакованный. Поэтому конечно же есть заранее запрограммированные микроконтроллеры, которые делает дядя Ли из Китая или Южной Кореи, а есть микроконтроллеры, которые делает дядя Вася в соседнем подъезде, который умеет их прошивать на чистом С через какой-нибудь arduino ide. Поэтому чистый си никуда не денется, вопрос в том что у него должно быть адекватное использование. Писать на чистом си какой-нибудь веб-сервер мы не будем.



