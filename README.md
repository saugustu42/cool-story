# Проверки в 21. Лекция Хирьянова про ub и не только.

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
