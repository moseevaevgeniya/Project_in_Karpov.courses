## Проект-3: Планирование и запуск А/В тестирования  

### 1. Описание задачи №1:

- У нас есть данные АА-теста с '2022-11-25' по '2022-12-01'. Нам нужно сделать симуляцию, как будто мы провели 10000 АА-тестов. На каждой итерации нам нужно сформировать подвыборки без повторения в 500 юзеров из 2 и 3 экспериментальной группы. Провести сравнение этих подвыборок t-testом.  
- Построить гистограмму распределения получившихся 10000 p-values  
- Посчитать, какой процент p values оказался меньше либо равен 0.05  
- Написать вывод по проведенному АА-тесту, корректно ли работает наша система сплитования  


#### Вот что у нас получилось:

- [Проведение АА-тестов на  пользователях ленты новостей](https://github.com/moseevaevgeniya/Project_in_Karpov.courses/blob/e3d79d3ab934d1bc6677197e40a3ebf126162536/3.%D0%9F%D1%80%D0%BE%D0%B5%D0%BA%D1%82:%D0%9F%D0%BB%D0%B0%D0%BD%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5%20%D0%B8%20%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA%20%D0%90%D0%92%20%D1%82%D0%B5%D1%81%D1%82%D0%B0/Task_1_AA_test__1_.ipynb)  
- P-values оказалось менее 5%, что свидетельствует о том, что наша система сплитования работает корректно и АА-тест пройден успешно  
- Несмотря на то, что процент "больших" p-value больше 5, это еще не будет значить, что сплитование некорректно. Тут как раз правильно смотреть на совокупность: равномерность распределение - небольшая доля маленьких pvalue  


### 2. Описание задачи №2:

Пришло время проанализировать результаты эксперимента, который мы провели вместе с командой дата сайентистов. Эксперимент проходил с 2022-12-02 по 2022-12-08 включительно. Для эксперимента были задействованы 2 и 1 группы.  

В группе 2 был использован один из новых алгоритмов рекомендации постов, группа 1 использовалась в качестве контроля.  

**Основная гипотеза заключается в том, что новый алгоритм во 2-й группе приведет к увеличению CTR.**  

- Наша задача — проанализировать данные АB-теста  
- Выбрать метод анализа и сравнить CTR в двух группах (мы разбирали t-тест, Пуассоновский бутстреп, тест Манна-Уитни, t-тест на сглаженном ctr (α=5) а также t-тест и тест Манна-Уитни поверх бакетного преобразования)  
- Сравнить данные этими тестами. А еще посмотрите на распределения глазами. Почему тесты сработали так как сработали?  
- Описать потенциальную ситуацию, когда такое изменение могло произойти  
- Написать рекомендацию, будем ли мы раскатывать новый алгоритм на всех новых пользователей или все-таки не стоит  


#### Вот что у нас получилось:

- [Оценка результатов проведения А/В тестирования данных пользователей ленты новостей](https://github.com/moseevaevgeniya/Project_in_Karpov.courses/blob/8e21ab631382098ef543f21ac17687b18c44e23e/3.%D0%9F%D1%80%D0%BE%D0%B5%D0%BA%D1%82:%D0%9F%D0%BB%D0%B0%D0%BD%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5%20%D0%B8%20%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA%20%D0%90%D0%92%20%D1%82%D0%B5%D1%81%D1%82%D0%B0/AB_test_task_2__1_.ipynb)
- Оценка тестами результатов А/В тестирования показал различные результаты и однозначных выводо сделать нельзя. В связи с чем, эксперимент будем считать неудачным, и в  продакшн такой алгоритм запускать нельзя.


### 3. Описание задачи №3:

Относительно недавно (в 2018-м году) исследователи из Яндекса разработали классный метод анализа тестов над метриками-отношениями (прямо как у нас) вида  𝑥/𝑦  (У нас  likes/clicks).  

**Идея метода заключается в следующем:**  

- Вместо того, чтобы заталкивать в тест «поюзерные» CTR, можно сконструировать другую метрику и анализировать ее, но при этом гарантируется (в отличие от сглаженного CTR), что если тест на этой другой метрике «прокрасится» и увидит изменения, значит изменения есть и в метрике исходной (то есть в лайках на пользователя и в пользовательских CTR  
- При этом метод сам по себе очень прост. Что это за метрика такая?  

  - Считаем общий CTR в контрольной группе 𝐶𝑇𝑅𝑐𝑜𝑛𝑡𝑟𝑜𝑙=𝑠𝑢𝑚(𝑙𝑖𝑘𝑒𝑠)/𝑠𝑢𝑚(𝑣𝑖𝑒𝑤𝑠)  
  - Посчитаем в обеих группах поюзерную метрику  𝑙𝑖𝑛𝑒𝑎𝑟𝑖𝑧𝑒𝑑_𝑙𝑖𝑘𝑒𝑠=𝑙𝑖𝑘𝑒𝑠−𝐶𝑇𝑅𝑐𝑜𝑛𝑡𝑟𝑜𝑙∗𝑣𝑖𝑒𝑤𝑠  
  - После чего сравним  t-тестом отличия в группах по метрике 𝑙𝑖𝑛𝑒𝑎𝑟𝑖𝑧𝑒𝑑_𝑙𝑖𝑘𝑒𝑠  


- Метод простой, гарантируется, что при приличном размере выборки (как у нас — подойдет) можно бесплатно увеличить чувствительность вашей метрики (или, по крайней мере, не сделать хуже). Это ОЧЕНЬ круто.  


**Наша задача:**  

- Проанализировать тест между группами 0 и 3 по метрике линеаризованных лайков. Видно ли отличие? Стало ли 𝑝−𝑣𝑎𝑙𝑢𝑒 меньше?  
- Проанализировать тест между группами 1 и 2 по метрике линеаризованных лайков. Видно ли отличие? Стало ли 𝑝−𝑣𝑎𝑙𝑢𝑒 меньше?  


#### Вот что у нас получилось:  

- [Анализ АВ теста по метрике линеаризованных лайков](https://github.com/moseevaevgeniya/Project_in_Karpov.courses/blob/9405b4546d63e71484a34435c8ac43347440e32c/3.%D0%9F%D1%80%D0%BE%D0%B5%D0%BA%D1%82:%D0%9F%D0%BB%D0%B0%D0%BD%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5%20%D0%B8%20%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA%20%D0%90%D0%92%20%D1%82%D0%B5%D1%81%D1%82%D0%B0/AB_test_linea.ipynb)  
- Для экспериментальных групп 0 и 3 t-тест показал статистически значимые различия с очень малыми p-value как на линеаризованных лайках, так и на CTR'ах. Но критерий стат. значимости после t-теста на линеаризованных лайках стал на 9 порядков меньше, чем после теста на CTR'ах, что свидетельствует о бОльшей чувствительности метрики линеаризованных лайков по сравнению с чистыми CTR'ами  
- Для экспериментальных групп 1 и 2 t-тест НЕ показал статистически значимых различий на метрике CTR, но тест на линеаризованных лайках дал p-value << 0.05, следовательно показал статистически значимые различия между группами. Такие различия в результатах между тестами еще больше убеждают меня в том, что метрика линеаризованных лайков  гораздо чувствительнее чистых CTR'ов.  


### 4. Теги:  

- ClickHouse  
- SQL Lab  
- JUPYTERHUB  
- Python  
- Библиотеки: pandas, pandahouse, numpy, matplotlib.pyplot,seaborn, scipy,stats,tqdm.auto,tqdm  
