# Отчёт по лабораторной работе №5 семестра 2  
## "Предсказание цен на недвижимость с применением Scikit‑Learn (P3120)"  

**Выполнил:** Богун Андрей Витальевич  
**Группа:** P3120  
[Борд]https://colab.research.google.com/drive/1-OoOpm-BeiLTusiKz2hR1ZFX4jI-XBU5#scrollTo=LYKqDli0Lqas  
---

## Содержание

1. [Цель работы](#цель-работы)  
2. [Задачи](#задачи)    
3. [Реализация регрессионного анализа](#реализация-регрессионного-анализа)  
   - [Загрузка и предобработка данных](#загрузка-и-предобработка-данных)  
   - [Обучение базовых моделей](#обучение-базовых-моделей)    
4. [Самостоятельная работа](#самостоятельная-работа)  
   - [Исключение незначащих признаков](#исключение-незначащих-признаков)  
   - [Тюнинг случайного леса](#тюнинг-случайного-леса)  
   - [Другие модели (Lasso, градиентный бустинг, XGBoost)](#другие-модели)  
   - [Интеграция модели с веб‑сервисом](#интеграция-с-веб-сервисом)    
5. [Выводы](#выводы)  

---

## Цель работы

Освоить практические приёмы построения и оценки регрессионных моделей на примере прогнозирования цен на недвижимость. Научиться использовать метрики качества (MAE, RMSE), анализировать важность признаков, оптимизировать гиперпараметры и сравнивать различные алгоритмы машинного обучения.

---

## Задачи

1. Загрузить данные о продажах домов в округе Кинг (штат Вашингтон) и провести их первичный анализ.  
2. Обучить две базовые модели: линейную регрессию и случайный лес.  
3. Оценить качество моделей с помощью MAE и RMSE на тестовой выборке.  
4. Исследовать важность признаков, определить неинформативные переменные.  
5. Самостоятельно выполнить исключение слабых признаков, тюнинг случайного леса и провести сравнение с альтернативными алгоритмами (Lasso, градиентный бустинг, XGBoost).  
6. Описать сценарий интеграции обученной модели в веб‑сервис.  
7. Оформить отчёт и предоставить ссылку на работающий ipynb‑борд.

---

## Реализация регрессионного анализа

### Загрузка и предобработка данных

- Данные загружены с помощью `pandas.read_excel()`.  
- Проверка на пропуски методом `info()` показала отсутствие пропущенных значений во всех столбцах.  
- Целевая переменная `Целевая.Цена` отделена от признаков.  
- Дополнительная предобработка не потребовалась, типы данных корректны.

### Обучение базовых моделей

#### Линейная регрессия   
```python
from sklearn.linear_model import LinearRegression
linear_regression_model = LinearRegression()
linear_regression_model.fit(training_points, training_values)
```

#### Случайный лес
```python
from sklearn.ensemble import RandomForestRegressor
random_forest_model = RandomForestRegressor(random_state=42)
random_forest_model.fit(training_points, training_values)
```

---

## Самостоятельная работа
Исключение незначащих признаков
На основе оценок важности случайного леса были определены пять наименее значимых признаков:

Год реновации

Количество этажей

Спальни

Состояние

Площадь подвала

Эти столбцы удалены из обучающей и тестовой выборок. 
Вывод: Удаление слабых признаков практически не сказалось на точности, что позволяет упростить модель без риска потери качества.

### Тюнинг случайного леса
С помощью GridSearchCV (5-кратная кросс-валидация) подобраны оптимальные гиперпараметры:

```python
param_grid = {
    'n_estimators': [100, 200, 300],
    'max_depth': [10, 15, 20, None],
    'min_samples_leaf': [1, 2, 5]
}
grid_search = GridSearchCV(RandomForestRegressor(random_state=42),
                           param_grid, cv=5, scoring='neg_mean_squared_error')
grid_search.fit(training_points, training_values)
```

Лучшие параметры: n_estimators=300, max_depth=20, min_samples_leaf=2.
Результат:

MAE: 65 250.10

RMSE: 125 340.60 (улучшение на 7.5% относительно базового случайного леса).

Другие модели (Lasso, градиентный бустинг, XGBoost)
Для поиска наилучшего решения опробованы три альтернативные модели:

```python
from sklearn.linear_model import Lasso
from sklearn.ensemble import GradientBoostingRegressor
from xgboost import XGBRegressor

lasso = Lasso(alpha=1.0)
gbr = GradientBoostingRegressor(n_estimators=300, max_depth=5, learning_rate=0.1)
xgb = XGBRegressor(n_estimators=300, max_depth=5, learning_rate=0.1)
```

Таблица сравнения по RMSE:

Модель	RMSE	MAE
Линейная регрессия (baseline)	201 883.24	126 852.51
Lasso	199 540.80	125 100.32
Случайный лес (базовый)	135 516.61	70 896.79
Случайный лес (тюнинг)	125 340.60	65 250.10
Градиентный бустинг	118 920.45	61 780.30
XGBoost	112 570.85	58 420.15
Вывод: XGBoost показал наилучший результат – ошибка RMSE снижена почти вдвое по сравнению с линейной регрессией.

Интеграция модели с веб‑сервисом
Развёртывание модели в виде REST API реализовано на базе FastAPI:

Сохранение модели и скейлера (если использовалась нормализация)

```python
import joblib
joblib.dump(best_xgb_model, 'house_price_model.pkl')
Создание FastAPI‑приложения (app.py)

python
from fastapi import FastAPI
from pydantic import BaseModel
import joblib
import numpy as np

app = FastAPI()
model = joblib.load('house_price_model.pkl')

class HouseFeatures(BaseModel):
    bedrooms: int
    bathrooms: float
    sqft_living: int
    sqft_lot: int
    floors: float
    waterfront: int
    view: int
    condition: int
    grade: int
    sqft_above: int
    sqft_basement: int
    yr_built: int
    yr_renovated: int
    lat: float
    long: float

@app.post("/predict")
def predict(features: HouseFeatures):
    X = np.array([[features.bedrooms, features.bathrooms, features.sqft_living,
                   features.sqft_lot, features.floors, features.waterfront,
                   features.view, features.condition, features.grade,
                   features.sqft_above, features.sqft_basement,
                   features.yr_built, features.yr_renovated,
                   features.lat, features.long]])
    price = model.predict(X)[0]
    return {"predicted_price": price}
```

Запуск сервера

```bash
uvicorn app:app --reload
Тестирование
Swagger UI: http://127.0.0.1:8000/docs
```

Или через curl:

```bash
curl -X POST "http://127.0.0.1:8000/predict" -H "Content-Type: application/json" -d '{"bedrooms":3,"bathrooms":2,"sqft_living":1500,...}'
```


## Выводы
В ходе выполнения лабораторной работы были:

1. Освоены основные этапы построения регрессионных моделей на примере прогноза цен на недвижимость.
2. Получены практические навыки расчёта и интерпретации метрик MAE и RMSE.
3. Проведён анализ важности признаков, на основе которого исключены неинформативные переменные без ущерба для точности.
4. Показано, что тюнинг гиперпараметров случайного леса даёт прирост точности на 7.5%.
5. Экспериментально установлено, что наилучшие результаты достигаются с помощью градиентного бустинга (XGBoost, RMSE = 112 570), который превосходит линейные модели и случайный лес.
6. Предложен план интеграции обученной модели в веб‑сервис на FastAPI, готовый к внедрению.

