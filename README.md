

# Теория машинного обучения #
## Метрические алгоритмы классификации ##
Метрические алгоритмы классификации - алгоритмы, основанные на вычислении оценок сходства между объектами на основе весовых функций.
### Метод *k* ближайших соседей (*kNN*) ###
Один из наиболее простых метрических алгоритмов классификации.
Работает следующим образом: дан классифицируемый объект *z* и обучающая выборка ![](http://latex.codecogs.com/gif.latex?%24X%5El%24). Требуется определить класс объекта *z* на основе данных из обучающей выборки. Для этого:
1. Вся выборка ![](http://latex.codecogs.com/gif.latex?%24X%5El%24) сортируется по возрастанию расстояния от объекта *z* до каждого объекта выборки.
2. Проверяются классы *k* ближайших соседей объекта *z*. Класс, встречаемый наиболее часто среди *k* соседей, присваивается объекту *z*.  

Исходными данными для алгоритма являются: классифицируемый объект, обучающая выборка и параметр *k* - число рассматриваемых ближайших соседей.
Результатом работы метода является класс классифицируемого объекта.

При *k = 1* алгоритм превращается в частный случай *1NN*. В таком слачае, рассматриваемый объект *z* присваивается к классу его первого ближайшего соседа. В свою очередь, остальные объекты не рассматриваются.

Пример работы *1NN* при использовании в качестве обучающей выборки Ирисы Фишера:

![1NN.png](https://github.com/VladislavDuma/SMPR/blob/master/img/1nn_allelem_1.png)

Для проверки оптимальности *k* используется Критерий Скользящего Контроля *LOO* (Leave One Out).
Данный критерий проверяет оптимальность значения *k* следующим образом:
1. Из обучающей выборки удаляется *i*-й объект ![](http://latex.codecogs.com/gif.latex?%24x%5Ei%24).
2. Запоминаем "старый" класс *i*-го объекта.
3. Запускаем алгоритм для оставшейся выборки. В результате работы *i*-му элементу присваивается "новый" класс на основе имеющейся выборки. Если значения "нового" и "старого" класса совпали, то *i*-ый элемент классифицировало верно. При их же несовпадении сумма ошибки увеличивается на 1.
4. Шаги 1-3 повторяются для каждого объекта выборки при фиксированном *k*. По окончании работы алгоритма полученная сумма ошибки *sum* делится на размер выборки *l*: ![sum=sum/l](http://latex.codecogs.com/gif.latex?sum%3D%20%5Cfrac%7Bsum%7D%7Bl%7D) .  Потом значение *k* меняется, и алгоритм повторяется для нового значения. *k* с наименьшим значением суммы ошибки будет оптимальным.
#### Реализация
При реализации алгоритма, в качестве обучающей выборки использовалась выборка ирисов Фишера. В качестве признаков объектов использовались значения длины и ширины лепестка. Значение *k* подбиралось по *LOO*.

Алгоритм:

    kNN <- function(xl, k, z) {
	  orderedXL <- sortObjectByDist(xl, z)
	  n <- dim(orderedXL)[2]
	  classes <- orderedXL[1:k, n] 
	  counts <- table(classes) # Таблица встречаемости каждого класса среди k ближайших соседей объекта
	  class <- names(which.max(counts)) # Наиболее часто встречаемый класс
	  return (class)
	}
где *xl* - обучающая выборка.

Достоинства алгоритма:
1. Простота реализации
2. Хорошее качество, при правильно подобранной метрике и параметре *k*

Недостатки алгоритма:
1. Необходимость хранить выборку целиком
2. Малый набор параметров
3. Качество классификации сильно зависит от выбранной метрики
