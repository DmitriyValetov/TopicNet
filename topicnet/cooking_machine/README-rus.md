# Cooking Machine

### Общее описание
Эксперимент состоит из моделей и кубиков, которые добавляют в эти модели новые регуляризаторы и осущетвляют подбор различных параметров тематичеких моделей на каждом этапе обучения.

Важно отметить, что на каждом шаге эксперимента используется один и тот же кубик для дальнейшей настройки моделей.
Если для разным тематических моделей после очередного этапа обучения применяются разные кубики, то эксперимент разделяется на два отдельных эксперимента, которые отличаются данным и последующими этапами обучения, иными словами последовательностями применения кубиков с соответствующими им параметрам.
Объект эксперимента содержит всю необходимую информацию для воспроизведения данного эксперимента с нуля и изменяется в процессе добавления кубиков пользователя.
При использовании кубиков также изменяется информация о моделях и их описание, создаются новые модели копии основной, которые соответствуют перебираемым внутри кубика параметрам.
Описание моделей также позволяет построить их по описанию и создать скрипт их создания и обучения.
Таким образом, ключевым объектом - является кубик, а точнее их последовательность, которые взаимодействуют с объектами модель и эксперимент.

#### Кубик
Смысловая единица обучения модели, которая вводит регуляризатор/регуляризаторы и перебирает его/их коэффициенты по сетке.
Внутри себя кубик вносит изменения в объекты модель (он еще и создает новые модели) и в объект эксперимент.

**Вход:** модель или модели, регуляризатор или регуляризаторы, параметры перебора (сетка), число итераций или метод выбора числа итераций, внешние метрики (которых нет в BigArtm, опционально).  
**Выход:** модели (их число совпадает с числом параметров в сетке или наборов параметров, если сетка многомерная).  
**Тело:** внутри себя содержит функциональные действия с `artm` моделью, вносить изменения в объект модели, создает модели, вносит изменения в объект эксперимента.

#### Модель
Смысловая единица хранения модели и ее описания:

* хранит своё описание;
* выводит описание в human-readable виде;
* преобразует описание модели в скрипт создания этой модели;
* не дает с собой ничего сделать, кроме как подгрузить и сделать свою копию (immutable), сама artm-модель хранится как атрибут этой модели, для ее изменения, ее нужно достать и самой модели, а после изменения заложить обратно;
* хранит id эксперимента;
* хранит id модели-родителя (если это пустая модель, то None);
* хранит имена тем;
* хранит список регуляризаторов с коэффициентами и другими параметрами;
* хранит список модальностей с весами;
* хранит пути к данным и сохраненной модели и информации о ней;
* хранит значения метрик;
* хранит результаты обучения.

#### Эксперимент
Смысловая единица описания эксперимента с обучением тематических моделей:

* хранит своё описание;
* выводит описание в human-readable виде;
* хранит описание стратегии обучения;
* проверяет свою целостность при считывании;
* делает свою копию, копию части себя.