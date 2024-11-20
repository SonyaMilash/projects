Классификация ЭКГ-сигналов

Задача и данные для нее были взяты с платформы MedBench (https://medbench.ru/ru/?page=1) от лаборатории искусственного интеллекта Сбербанка. MedBench содержит открытые наборы задач в области здравоохранения. Для исследования несбалансированных данных в классификации с несколькими метками была использована задача ECG2Pathology. Данными для нее являются ЭКГ-сигналы из открытого датасета PTB-XL (https://physionet.org/content/ptb-xl/1.0.3/). Разметка сигналов выполнена 3-мя кардиологами и проверена врачом-модератором в соответствии с тезаурусом диагностических заключений. Для каждого тестового примера нужно предсказать список из 73 возможных элементов тезауруса (сердечных патологий или служебно-технических классов).
В отличие от исходного датасета, датасет, предоставленный MedBench, содержит уменьшенное количество объектов, они сразу разделены на train (2101 пример) и test (449 пример), а также ЭКГ-сигналы сегментировали с помощью модуля Wave From Database (WFDB) для обработки базы данных. 
Характеристики для оценки состава меток в наборе данных multilabel классификации:
1.	Коэффициент дисбаланса на метку (IRLbl) 
2.	Средний коэффициент дисбаланса (MeanIR) 
3.	Коэффициент вариации IRLbl (CVIR)
4.	Локальная плотность меток. 
5.	Глобальная плотность меток 
6.	Уровень совпадения (SCUMBLE)

Методы балансировки
1.	Multi-Label Random Oversampling (MLROS) [1]
2.	Multi-label Synthetic Minority Oversampling Technique (MLSMOTE) [2]
3.	Resampling Multi-label datasets by Decoupling highly Imbalanced Labels (REMEDIAL). [3]
4.	Multi-Label UndersampLing (MLUL) [4]
5.	Multi-Label Synthetic Oversampling based on Label Imbalance Rate and Neighborhood Distribution (MLSIN)  [3]

Использованные алгоритмы.
•	Адаптированный MLkNN 
•	Базовый классификатор Random Forest Classifier с преобразованием задачи Label Powerset
•	Базовый классификатор Logistic Regression с преобразованием задачи Classifier Chain
•	Адаптированный Logistic Regression


1D Convolution.
Первая простая модель, обученная на метаданных, состоит из пяти полносвязных слоев Dense, дополненных слоями регуляризации Dropoout. На последнем Dense слое функция активации – сигмоида.
Вторая модель представляет собой улучшенную метаданными модель 1D Convolution для кривых ЭКГ.
Модель для ЭКГ сигналов состоит из трех слоев Conv1D, дополненными слоями нормализации и активации. Слои разделены слоями MaxPool1D, которые уменьшают размерность временных рядов входного тензора. С другой стороны, слои Conv1D имеют растущее количество фильтров, поэтому расширяется измерение объектов. Вся модель дорабатывается слоем GlobalAveragePooling1D и регуляризацией Dropout.
Далее модель кривых ЭКГ и модель метаданных пациентов объединяются в одну модель, в которой есть два входных набора данных и один выходной. Результат конкатенации передан на два полносвязных слоя Dense, за которыми следуют Dropout и последний слой Dense с сигмоидой.

________________________________________
1.	F. Charte, A.J. Rivera, M.J. del Jesus, F. Herrera, Addressing imbalance in multilabel classification: Measures and random resampling algorithms, Neurocomputing (2015), doi:10.1016/j.neucom.2014.08.091
2.	F. Charte, A.J. Rivera, M.J. del Jesus, F. Herrera, MLSMOTE: approaching imbalanced multilabel learning through synthetic instance generation, Knowl.- Based Syst. 89 (2015) 385–397, doi:10.1016/j.knosys.2015.07.019
3.	Charte, F., Rivera, A. J., del Jesus, M. J., & Herrera, F. (2019). Dealing with difficult minority labels in imbalanced mutilabel data sets. Neurocomputing, 326–327, 39–53. 10.1016/j.neucom.2016.08.158
4.	B. Liu, K.Blekas, G.Tsoumakas. Multi-Label Sampling based on Local Label Imbalance. 2022.https://doi.org/10.1016/j.patcog.2021.108294

