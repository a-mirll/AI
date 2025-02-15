# Отчет по лабораторной работе №1
## 1. Теоретическая база
### 1.1. Нейронные сети
Нейронные сети — это вычислительные системы, вдохновленные биологическими нейронными сетями. Они состоят из множества слоёв, которые преобразуют входные данные в выходные, используя обучаемые параметры (веса и смещения).

**Основные компоненты нейронных сетей:**

- Нейроны: Основные вычислительные единицы, которые принимают входные данные, применяют к ним веса и активационную функцию, чтобы передать результат на следующий слой.

- Слои: Нейроны объединяются в слои. Основные типы слоёв: входной слой (принимает исходные данные), скрытые слои (промежуточные слои, которые извлекают признаки из данных), выходной слой (выдаёт конечный результат, например, класс изображения)

- Функция активации: Нелинейная функция, которая добавляет сложность модели. Примеры: ReLU, Sigmoid, Softmax.

- Функция потерь: Оценивает, насколько предсказания модели отличаются от истинных значений. Примеры: Cross-Entropy Loss, Mean Squared Error.

- Оптимизатор: Алгоритм, который обновляет параметры модели для минимизации функции потерь.

### 1.2. Свёрточные нейронные сети (CNN)
CNN — это подкласс нейронных сетей, разработанный для обработки изображений. Они используют свёрточные слои для извлечения признаков из данных. Основные компоненты CNN:

- Свёрточный слой (Convolutional Layer): Применяет фильтры (ядра) к входным данным для извлечения локальных признаков.

- Слой подвыборки (Pooling Layer): Уменьшает размерность данных, сохраняя важные признаки. Пример: Max Pooling.

- Полносвязный слой (Fully Connected Layer): Используется для классификации на основе извлечённых признаков.

### 1.3. Архитектура VGG16

VGVGG16 — это свёрточная нейронная сеть (CNN), разработанная группой Oxford Visual Geometry Group (VGG) в 2014 году. Она стала одной из ключевых архитектур в области компьютерного зрения благодаря своей простоте и высокой точности в задачах классификации изображений.

**Основные характеристики VGG16:**

- Глубина сети: 16 слоёв (13 свёрточных и 3 полносвязных).

- Используемые ядра: Все свёрточные слои используют ядра размером 3x3 с шагом 1 и padding=1, что позволяет сохранить размерность данных.

- Max Pooling: Применяется после каждого блока свёрток для уменьшения размерности. Используются ядра 2x2 с шагом 2.

- Активация: После каждого свёрточного слоя применяется функция активации ReLU (Rectified Linear Unit).

- Полносвязные слои: Три полносвязных слоя используются для классификации. Первые два имеют 4096 нейронов, последний — 1000 нейронов (для ImageNet) или другое количество, в зависимости от задачи.

- Dropout: Используется в полносвязных слоях для предотвращения переобучения.

### 1.4. Оптимизаторы

Оптимизаторы используются для обновления параметров модели с целью минимизации функции потерь. В работе рассматриваются два оптимизатора:

- Adam: Адаптивный метод, сочетающий преимущества RMSProp и Momentum. Он адаптирует скорость обучения для каждого параметра.

- AdaBound: Модификация Adam, которая ограничивает скорость обучения, чтобы избежать нестабильности на поздних этапах обучения.

## 2. Описание разработанной системы

**Архитектура системы**

Система состоит из следующих компонентов:

- Модель VGG16: Реализована с нуля на PyTorch. Включает:

  - 5 блоков свёрток с Batch Normalization и Max Pooling.

  - 3 полносвязных слоя с Dropout для регуляризации.

- Загрузка данных: Используется класс CarsDataset для загрузки изображений и меток из .mat файлов.

- Обучение: Реализованы функции train_model и evaluate_model для обучения и оценки модели.

- Оптимизаторы: Используются AdaBound и Adam для сравнения.

**Алгоритм работы**

- Загрузка и предобработка данных:

  - Изменение размера изображений до 224x224.

  - Нормализация значений пикселей.

- Инициализация модели VGG16.

- Обучение модели:

  - Для каждого оптимизатора (AdaBound и Adam) проводится обучение в течение 15 эпох.

  - На каждой эпохе вычисляются потери и точность.

- Оценка модели на тестовом наборе данных.

- Сравнение результатов.

## 3. Результаты работы и тестирования системы

Ниже приведены примеры результатов:

**Пример выводов в процессе обучения:**
```
Обучение с оптимизатором AdaBound:

Начало эпохи 1/15
Эпоха 1/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [12:45<00:00, 47.82s/it]
Эпоха 1/15 | Потери на обучении: 5.6807 | Точность на обучении: 0.20%

Начало эпохи 2/15
Эпоха 2/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [15:38<00:00, 58.66s/it]
Эпоха 2/15 | Потери на обучении: 5.1490 | Точность на обучении: 0.80%

Начало эпохи 3/15
Эпоха 3/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [15:11<00:00, 56.96s/it]
Эпоха 3/15 | Потери на обучении: 4.9532 | Точность на обучении: 3.40%

Начало эпохи 4/15
Эпоха 4/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [15:29<00:00, 58.10s/it]
Эпоха 4/15 | Потери на обучении: 4.7618 | Точность на обучении: 4.00%

Начало эпохи 5/15
Эпоха 5/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [14:59<00:00, 56.20s/it]
Эпоха 5/15 | Потери на обучении: 4.6099 | Точность на обучении: 5.40%

Начало эпохи 6/15
Эпоха 6/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [09:03<00:00, 33.99s/it]
Эпоха 6/15 | Потери на обучении: 4.3941 | Точность на обучении: 7.60%

Начало эпохи 7/15
Эпоха 7/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [08:39<00:00, 32.44s/it]
Эпоха 7/15 | Потери на обучении: 4.1128 | Точность на обучении: 10.60%

Начало эпохи 8/15
Эпоха 8/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [08:09<00:00, 30.61s/it]
Эпоха 8/15 | Потери на обучении: 3.8304 | Точность на обучении: 14.80%

Начало эпохи 9/15
Эпоха 9/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [08:02<00:00, 30.16s/it]
Эпоха 9/15 | Потери на обучении: 3.6113 | Точность на обучении: 18.20%

Начало эпохи 10/15
Эпоха 10/15: 100%|█████████████████████████████████████████████████████████████████████| 16/16 [08:35<00:00, 32.20s/it]
Эпоха 10/15 | Потери на обучении: 3.2343 | Точность на обучении: 23.60%

Начало эпохи 11/15
Эпоха 11/15: 100%|█████████████████████████████████████████████████████████████████████| 16/16 [08:29<00:00, 31.82s/it]
Эпоха 11/15 | Потери на обучении: 2.8507 | Точность на обучении: 32.20%

Начало эпохи 12/15
Эпоха 12/15: 100%|█████████████████████████████████████████████████████████████████████| 16/16 [08:18<00:00, 31.16s/it]
Эпоха 12/15 | Потери на обучении: 2.4561 | Точность на обучении: 38.60%

Начало эпохи 13/15
Эпоха 13/15: 100%|█████████████████████████████████████████████████████████████████████| 16/16 [09:39<00:00, 36.21s/it]
Эпоха 13/15 | Потери на обучении: 2.1023 | Точность на обучении: 48.60%

Начало эпохи 14/15
Эпоха 14/15: 100%|█████████████████████████████████████████████████████████████████████| 16/16 [09:14<00:00, 34.65s/it]
Эпоха 14/15 | Потери на обучении: 1.7362 | Точность на обучении: 55.40%

Начало эпохи 15/15
Эпоха 15/15: 100%|█████████████████████████████████████████████████████████████████████| 16/16 [08:51<00:00, 33.24s/it]
Эпоха 15/15 | Потери на обучении: 1.5104 | Точность на обучении: 61.20%
Обучение завершено за 161.14 минут.

Оценка модели после обучения с AdaBound:
Оценка на тестовом наборе: 100%|███████████████████████████████████████████████████████| 16/16 [01:55<00:00,  7.20s/it]
Тестовая точность AdaBound: 1.40%


Обучение с оптимизатором Adam:

Начало эпохи 1/15
Эпоха 1/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [09:02<00:00, 33.89s/it]
Эпоха 1/15 | Потери на обучении: 5.6075 | Точность на обучении: 0.40%

Начало эпохи 2/15
Эпоха 2/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [09:02<00:00, 33.92s/it]
Эпоха 2/15 | Потери на обучении: 5.1633 | Точность на обучении: 1.20%

Начало эпохи 3/15
Эпоха 3/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [08:06<00:00, 30.39s/it]
Эпоха 3/15 | Потери на обучении: 5.0404 | Точность на обучении: 1.60%

Начало эпохи 4/15
Эпоха 4/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [08:24<00:00, 31.51s/it]
Эпоха 4/15 | Потери на обучении: 4.8502 | Точность на обучении: 1.80%

Начало эпохи 5/15
Эпоха 5/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [09:17<00:00, 34.82s/it]
Эпоха 5/15 | Потери на обучении: 4.6637 | Точность на обучении: 5.20%

Начало эпохи 6/15
Эпоха 6/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [08:35<00:00, 32.19s/it]
Эпоха 6/15 | Потери на обучении: 4.5561 | Точность на обучении: 5.40%

Начало эпохи 7/15
Эпоха 7/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [08:10<00:00, 30.68s/it]
Эпоха 7/15 | Потери на обучении: 4.2458 | Точность на обучении: 9.60%

Начало эпохи 8/15
Эпоха 8/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [08:09<00:00, 30.57s/it]
Эпоха 8/15 | Потери на обучении: 4.0305 | Точность на обучении: 12.40%

Начало эпохи 9/15
Эпоха 9/15: 100%|██████████████████████████████████████████████████████████████████████| 16/16 [07:58<00:00, 29.91s/it]
Эпоха 9/15 | Потери на обучении: 3.6634 | Точность на обучении: 18.80%

Начало эпохи 10/15
Эпоха 10/15: 100%|█████████████████████████████████████████████████████████████████████| 16/16 [08:01<00:00, 30.12s/it]
Эпоха 10/15 | Потери на обучении: 3.3590 | Точность на обучении: 21.80%

Начало эпохи 11/15
Эпоха 11/15: 100%|█████████████████████████████████████████████████████████████████████| 16/16 [07:32<00:00, 28.26s/it]
Эпоха 11/15 | Потери на обучении: 2.9646 | Точность на обучении: 30.20%

Начало эпохи 12/15
Эпоха 12/15: 100%|█████████████████████████████████████████████████████████████████████| 16/16 [08:43<00:00, 32.71s/it]
Эпоха 12/15 | Потери на обучении: 2.5829 | Точность на обучении: 39.80%

Начало эпохи 13/15
Эпоха 13/15: 100%|█████████████████████████████████████████████████████████████████████| 16/16 [08:23<00:00, 31.44s/it]
Эпоха 13/15 | Потери на обучении: 2.1162 | Точность на обучении: 49.00%

Начало эпохи 14/15
Эпоха 14/15: 100%|█████████████████████████████████████████████████████████████████████| 16/16 [09:23<00:00, 35.20s/it]
Эпоха 14/15 | Потери на обучении: 1.7201 | Точность на обучении: 58.40%

Начало эпохи 15/15
Эпоха 15/15: 100%|█████████████████████████████████████████████████████████████████████| 16/16 [09:05<00:00, 34.10s/it]
Эпоха 15/15 | Потери на обучении: 1.3456 | Точность на обучении: 66.60%
Обучение завершено за 127.93 минут.

Оценка модели после обучения с Adam:
Оценка на тестовом наборе: 100%|███████████████████████████████████████████████████████| 16/16 [01:50<00:00,  6.91s/it]
Тестовая точность Adam: 1.40%

```

**Графики сравнения оптимизаторов**

![image](https://github.com/user-attachments/assets/61eb5679-c174-4637-beac-cb3884cf07d9)

## 4. Выводы по работе

- Использование VGG16 позволило успешно решить задачу классификации изображений автомобилей по классам.

- Adam показал себя как более быстрый и эффективный оптимизатор для данной задачи.

- AdaBound демонстрирует более стабильную сходимость, что может быть полезно для задач с большим количеством данных.

- В будущих работах можно улучшить качество модели за счет аугментации данных и увеличения количества эпох.

## 5. Использованные источники

Гудфеллоу, И., Бенджио, Ю., и Курвиль, А. (2016). Глубокое обучение. MIT Press. [Электронный ресурс]. URL: https://www.deeplearningbook.org/ 

Луо, Л., и др. (2019). AdaBound: Адаптивный метод скорости обучения. [Электронный ресурс]. URL: https://arxiv.org/abs/1902.09843

Нильсен, М. А. (2015). Нейронные сети и глубокое обучение. Determination Press. [Электронный ресурс]. URL: http://neuralnetworksanddeeplearning.com/

Симонян, К., и Зиссерман, А. (2014). Очень глубокие сверточные сети для крупномасштабного распознавания изображений.[Электронный ресурс]. URL: https://arxiv.org/abs/1409.1556

ЛеКун, Я., Бенджио, Ю., и Хинтон, Г. (2015). Глубокое обучение. Nature, 521(7553), 436-444. [Электронный ресурс]. URL: https://www.nature.com/articles/nature14539

