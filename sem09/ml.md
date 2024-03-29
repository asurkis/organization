Лектор: Алексей Александрович Шпильман
Практик: Олег Свидченко

# Разная теория
## Общие понятия
**Обучение с учителем (supervised learning)** — на каждый вход из обучающей выборки известен правильный ответ. Задача: аппроксимировать целевую функцию.

**Обучение без учителя (unsupervised learning)** — известны входные данные, но не выходные. Цели: извлечение информации, выделение зависимостей, сжатие данных.

**Train-test split** — выделяем часть выборки для _проверки_, насколько хорошо обучилась модель. Тестовая выборка исключается из обучающей.

**Cross-validation** — проверка метрик на разных разбиениях на обучающую и тестовую выборки.

**One Hot Encoding** — каждый категориальный признак кодируется как вектор из нулей и одной единицы (количество измерений — количество возможных классов).

**Утечка данных** — протекание тестовой выборки в обучающую. Опасно тем, что метрика модели будет оцениваться неправильно, потому что может быть переобучение на _тестовых_ данных.

**R<sup>2</sup>-score** — "доля объяснённой дисперсии":

$$
R^2 = 1 - \frac{\sum (h(X_i) - y_i)^2}{\sum (\overline{y} - y_i)^2} < 1
$$

## Scalers
Масштабирование нужно для того, чтобы модель учитывала все фичи, а не только большие по модулю. Также может быть полезно центрировать фичу вокруг нуля.

MinMax scaler:

$$
x' = \frac{x - \min(x)}{\max(x) - \min(x)} \cdot (\text{max}' - \text{min}') + \text{min}'
$$

MaxAbs scaler:

$$
x' = \frac{x}{\max(|x|)}
$$

Standard scaler:

$$
x' = \frac{x - \text{mean}(x)}{\text{std}(x)} = \frac{x - E(x)}{\sigma(x)}
$$

Robust scaler:

$$
x' = \frac{x - \text{median}(x)}{\text{quartile}_3(x) - \text{quartile}_1(x)}
$$


## Другое
**Autoencoder** — нейросеть сжимает изображение до маленького вектора, затем генерирует из него исходное изображение. U-Net — НЕ автоэнкодер.

**GAN** — Generative-Adversarial Networks. Функция потерь генерирующей нейросети — это нейросеть-детектор, которая учится отличать сгенерированные изображения от настоящих.

# Экзамен
## Билет 1
### Регрессия, борьба с выбросами. RANSAC. Theil-Sen. Huber.
**Theil-Sen** — обучить несколько моделей на подмножествах обучающей выборки, в качестве ответа брать медиану ответов нескольких моделей.

**RANSAC (RANdom SAmple Consensus)** — обучить модели на подмножествах обучающей выборки, выбрать лучшую по количеству попаданий (полоса), обучить новую модель на этих попаданиях.

**Huber regressor** — квадратичная ошибка для попаданий, линейная — для выбросов. Так выбросы влияют на результат значительно меньше.

$$
\begin{gather*}
\min_{w, \sigma} \sum_{i=1}^N \left( \sigma + H_\varepsilon \left( \frac{X_i W - y_i}{\sigma} \right) \sigma \right) + \alpha ||W||_2^2 \\
H_m(z) =
\begin{cases}
    z^2 & |z| < \varepsilon \\
    2 \varepsilon |z| - \varepsilon^2 & |z| \geq \varepsilon \\
\end{cases} \\
\end{gather*}
$$

$\sigma$ — константа масштабирования.

Рекомендуется устанавливать $\varepsilon = 1.35$ для достижения 95% статистической эффективности.

### Кластеризация. kMeans, kMeans++, MeanShift, DBSCAN.
**k-means:** количество кластеров предопределено. Кластер задаётся его **центроидом** (точкой). Плоскость (пространство) делится диаграммой Вороного (т.е. точка относится к тому кластеру, центроид которого к ней ближайший). Задача — минимизировать

$$
\sum_{X_j \in X} \min_{\mu_i} \Bigl| |X_j - \mu_i| \Bigr|_2^2
$$

То есть функция ошибки это сумма квадратов расстояний от точек до центроидов назначенных им кластеров.
На каждом шаге вычисляются кластеры, затем центроиды перемещаются в центры масс своих кластеров.
Инициализируются центроиды случайным образом. Цикл работает до сходимости.

Преимущества:
+ Быстрая сходимость

Недостатки:
- Инициализация кластеров
- См. картинки к лекции 3

**k-means++:**
1. Выбрать центроиды случайным образом из точек
2. Для каждой точки **x** посчитать расстояние до ближайшего центроида $M(X)$
3. Выбрать новый центроид с вероятностью, пропорциональной $M^2(X)$
4. Повторить (k - 1) раз шаги 2-3
5. Запустить k-means
Тогда кластеры инициализируются более удалёнными друг от друга.

**Mean Shift:** количество кластеров не предопределено.

$$
\begin{aligned}
\mu_i^0 & = X_i \\
\mu_i^{t + 1} & = \frac{\sum_{X_j \in N(\mu_i^t)} \text{RBF}(X_j - \mu_i^t) X_j}{\sum_{X_j \in N(\mu_i^t)} \text{RBF}(X_j - \mu_i^t)}
\end{aligned}
$$

Где $N(\mu_j^t)$ — окрестность $\mu_j^t$.
Radial Basis Function:

$$
\text{RBF}(d) = \exp\left( -c \Bigl| |d| \Bigr|_2^2 \right)
$$

**DBSCAN (Density-Based Spatical Clustering Applications with Noise):** количество кластеров не предопределено. В качестве начальных ядер кластеров точки с хотя бы $m$ других точек на расстоянии не более заданного $\varepsilon$. Затем кластеры объединяются, если их ядра находятся не дальше $\varepsilon$ друг от друга.

Преимущества:
+ Можно работать с не сферическими кластерами

Недостатки:
- Надо подбирать $\varepsilon$ и $m$
- Медленно работает

## Билет 2
### Смещение и дисперсия, понятие средней гипотезы.
**Смещение и дисперсия (bias & variance)**:

$$
\begin{gather*}
E_\text{out}(h^D) = \mathbb{E}_{X} \left[ \left( h^D(X) - f(X) \right)^2 \right] \\
\begin{aligned}
\mathbb{E}_D \left[ E_\text{out}(h^D) \right]
& = \mathbb{E}_D \left[ \mathbb{E}_{X} \left[ \left( h^D(X) - f(X) \right)^2 \right] \right] = \\
& = \mathbb{E}_X \left[ \mathbb{E}_D \left[ \left( h^D(X) - f(X) \right)^2 \right] \right] = \\
\end{aligned} \\
\overline{h}(X) = \mathbb{E}_D \left[ h^D(X) \right] \\
\begin{aligned}
& \mathbb{E}_D \left[ \left( h^D(X) - f(X) \right)^2 \right]
= \mathbb{E}_D \left[ \left( h^D(X) - \overline{h}(X) + \overline{h}(X) - f(X) \right)^2 \right] = \\
& = \mathbb{E}_D \left[ \left( h^D(X) - \overline{h}(X) \right)^2 + \left( \overline{h}(X) - f(X) \right)^2 + 2 \left( h^D(X) - \overline{h}(X) \right) \left( \overline{h}(X) - f(X) \right) \right] = \\
& = \mathbb{E}_D \left[ \left( h^D(X) - \overline{h}(X) \right)^2 \right] + \left( \overline{h}(X) - f(X) \right)^2 \\
\end{aligned} \\
\begin{aligned}
\mathbb{E}_D \left[ E_\text{out}(h^D) \right]
& = \mathbb{E}_X \left[ \mathbb{E}_D \left[ \left( h^D(X) - f(X) \right)^2 \right] \right] = \\
& = \mathbb{E}_X \left[ \mathbb{E}_D \left[ \left( h^D(X) - \overline{h}(X) \right)^2 \right] + \left( \overline{h}(X) - f(X) \right)^2 \right] = \\
& = \mathbb{E}_X \left[ \text{bias}(X) + \text{variance}(X) \right] = \\
& = \text{bias} + \text{variance}
\end{aligned} \\
\end{gather*}
$$

**Средняя гипотеза:** $\overline{h}(X) = \mathbb{E}_D \left[ h^D(X) \right]$ — средняя по всем возможным гипотезам на датасете.

### Ансамбли. Soft and Hard Voting. Bagging. Случайный лес. AdaBoost.
**Ансамбли** — объединения нескольких моделей с голосованием.

**Жёсткое голосование** — одна модель — один голос. **Мягкое** — вес голоса зависит от уверенности модели.

**Случайный лес** — несколько деревьев обучаются на разных подвыборках (возможно, пересекающихся).

**Bagging (bootstrap aggregating)** — случайная выборка с повторениями такого же размера, как и оригинальный датасет. На практике вместо новых точек просто старым присваивается вес больше 1.

**Random subspaces** — случайная выборка фичей, а не точек.

**Random patches** — случайная выборка и фичей, и точек.

**AdaBoost (Adaptive Boosing)** — лес с деревьями глубины 1 ("пеньками"). $D_1(i) = 1/N$.

$$
\begin{gather*}
E_t = \sum_{i=1}^N D_t(i) E(h_t(x_i), y_i) \\
\text{вес гипотезы}~h_t : \alpha_t  = \frac{1}{2} \ln \left( \frac{1 - E_t}{E_t} \right) \\
\text{для неправильной классификации}~D_{t+1}(i) = D_t(i) \exp(\alpha_t) \\
\text{для правильной классификации}~D_{t+1}(i) = D_t(i) \exp(-\alpha_t) \\
\text{затем нормализовать} \\
\end{gather*}
$$

## Билет 3
### Линейная регрессия. LASSO, LARS. CART. SVR.
**Регуляризация линейной регрессии**
**L2** — к ошибке добавляется $\alpha W^T W$, т.е.

$$
L = (XW - Y)^T (XW - Y) + \alpha W^T W
$$

Тогда

$$
W = (X^T X + \alpha I)^{-1} X^T Y
$$

**L1**, она же **LASSO (Least Absolute Shrinkage and Selection Operator)** — к ошибке добавляется $\alpha ||W||$. Тогда искать решение надо численными методами, например, градиентным спуском или _LARS_.

**LARS (Least Angle Regression):**
1. Взять фичу $x_i$, максимально коррелирующую с $y$
2. Ввести $\beta_1$ как множитель для $x_i$ и увеличивать/уменьшать его, пока корреляция $x_i$ с $r = y - \hat{y}$ максимальна
3. Найти $x_j$ — новую фичу, максимально коррелирующую с $y$
4. Ввести $\beta_2$ как множитель для $(x_i \pm x_j)$
5. Повторять с шага 2, пока $\alpha \cdot \sum_i \beta_i < -\Delta \text{Error}$

**Elastic Net:**

$$
L = ||XW - Y||_2^2 + \alpha ||W||_2^2 + \beta ||W||_1
$$

**CART** — регрессия решающими деревьями. Deviance for regression:

$$
I_V(X) = \sum_{x_i \in X} \sum_{x_j \in X} \frac{1}{2} (y_i - y_j)^2
$$

**SVR** — SVM для регрессии:

$$
\left\{
\begin{aligned}
& \frac{1}{2}||W||^2 + C \sum (\xi_i + \xi_i^*) \to \min \\
& y_i - W^T X_i - b \leq \varepsilon + \xi_i \\
& W^T X_i + b - y_i \leq \varepsilon + \xi_i^* \\
& \xi_i \xi_i^* \geq 0
\end{aligned} \right.
$$

### Деревья решений. Информационный выигрыш. Ошибка классификации, энтропия, критерий Джини.
**Решающие деревья:**
+ Интерпретируемость
+ Почти не нужен препроцессинг
+ И числовые, и категориальные признаки
+ Устойчивость

**Information gain:**

$$
\text{IG} = \frac{|X|_\text{node}}{|X|_\text{total}} I(X_\text{node}) - \frac{|X|_\text{right}}{|X|_\text{total}} I(X_\text{right}) - \frac{|X|_\text{left}}{|X|_\text{total}} I(X_\text{left})
$$

**Misclassification error:**

$$
I_E(X) = 1 - \max\{ p(y) \} = 1 - \max_y \left( \frac{|x_i : y_i = y|}{|X|} \right)
$$

**Entropy:**

$$
I_H(X) = -\sum_{y \in Y} p(y) \log_2 (p(y))
$$

**Gini impurity:**

$$
I_G(X) = \sum_{y \in Y} p(y) (1 - p(y))
$$

## Билет 4
### Глобальный поиск. Случайный поиск. Grid search. Случайное блуждание. Байесовская оптимизация. Кросс-энтропийный поиск.
**Глобальный поиск** — например, задача коммивояжёра.

**Случайный поиск** — случайным образом выбираем решение, до сходимости.

**Поиск по сетке** — поиск по сетке через равные промежутки параметров.

**Случайное блуждание** — случайный поиск, где каждая следующая точка — смещение от предыдущей.

**Байесовская оптимизация** — считаем, что целевая функция как-то распределена в пространстве. Задаём начальное значение среднего и дисперсии возможных значений функции для всех точек. При каждом измерении нам становится известна дисперсия (0) и среднее в конкретной точке. Далее каким-то образом выбираются другие точки измерений, например, по вероятности получить наибольшее значение. **Ядро** регулирует, как быстро меняется аппроксимация предсказания дисперсии и среднего.

$$
\begin{gather*}
P(f_{t + 1} | \mathcal{D}_{1:t}, X_{t + 1}) = \mathcal{N} (\mu_t(X_{t + 1}), \sigma_t^2(X_{t + 1})) \\
\mu_t(X_{t + 1}) = \mathbf{k}^T \mathbf{K}^{-1} f_{1:t} \\
\sigma_t^2(X_{t + 1}) = k(X_{t + 1}, X_{t + 1}) - \mathbf{k}^T \mathbf{K}^{-1} \mathbf{k} \\
\mathbf{K} =
\begin{bmatrix}
    k(X_1, X_1) & \cdots & k(X_1, X_t) \\
    \vdots & \ddots & \vdots \\
    k(X_t, X_1) & \cdots & k(X_t, X_t) \\
\end{bmatrix} + \sigma^2_\text{noise} I \\
\mathbf{k} =
\begin{bmatrix}
k(X_{t + 1}, X_1)
& k(X_{t + 1}, X_2)
& \ldots
& k(X_{t + 1}, X_t)
\end{bmatrix}
\end{gather*}
$$

Самая популярная функция ковариации:

$$
k(X_i, X_j) = \exp\left( -\frac{1}{2} ||X_i - X_j||^2 \right)
$$

Методы выбора следующего измерения:
- Expected Improvement:

$$
\text{EI}(X) = \mathbb{E} \max(f(X) - f(X_\text{best}), 0)
$$

- Upper confidence bound:

$$
\text{UCB}(X) = \mu(X) + \beta \sigma(X)
$$

- Probability of improvement:

$$
\text{PI}(X) = \mathbb{E}(f(X) > f(X_\text{best}))
$$

**Кросс-энтропийный поиск:**
1. На первом шаге выбираем случайные параметры $\theta_1$ распределения
2. Генерируем случайный набор $X_1, \ldots, X_N$ из распределения $p(X; \theta_t)$
3. Следующий набор параметров выбирается по критерию

$$
\theta_{t + 1} = \arg\max_\theta \sum_{X_i \in \text{best} k} y_i \frac{p(X_i; \theta)}{p(X_i; \theta_t)} \log p(X_i; \theta_t)
$$

### Линейная регрессия. Полиномиальная регрессия. Гребневая регрессия.
**Линейная регрессия:** минимизируем среднеквадратическое отклонение.
Можно дополнительно ввести $x_{i0} = 1$ и $w_0$ для удобства подсчёта смещения (bias). Тогда $\hat{M} = M + 1$.

$$
\begin{gather*}
\text{MSE} = \frac{1}{N} \sum_{i=1}^N (W^T X_i - y_i)^2 = \frac{1}{N} \Bigl| |X W - Y| \Bigr|_2^2 \\
X =
\begin{bmatrix}
    1 & x_{11} & \cdots & x_{1M} \\
    \vdots & \vdots & \ddots & \vdots \\
    1 & x_{N1} & \cdots & x_{NM} \\
\end{bmatrix} \\
Y =
\begin{bmatrix}
    y_1 \\
    \vdots \\
    y_N \\
\end{bmatrix} \\
W =
\begin{bmatrix}
    w_0 \\
    w_1 \\
    \vdots \\
    w_M \\
\end{bmatrix} \\
L_\text{linear}(W) = N \cdot \text{MSE} = \Bigl| |X W - Y| \Bigr|_2^2 \\
\end{gather*}
$$

Минимизируем $\text{MSE} \sim L_\text{linear}$:

$$
\begin{gather*}
\begin{aligned}
L_\text{linear}(W)
& = (X W - Y)^T (X W - Y) = \\
& = (W^T X^T - Y^T) (X W - Y)
\end{aligned} \\
\nabla L_\text{linear}(W) = 2 X^T (X W - Y) = \vec{0} \\
X^T X W = X^T Y \\
W = (X^T X)^{-1} X^T Y \\
\end{gather*}
$$

Можно перейти к полиномиальной регрессии, просто добавив признаки $x_{ij}^q$. Но слишком много признаков приведут к переобучению.

**Гребневая (Ridge) регрессия** — решение L2-регуляризации

$$
L = ||XW - Y||_2^2 + \alpha ||W||_2^2
$$

Тогда

$$
W = (X^T X + \alpha I)^{-1} X^T Y
$$

## Билет 5
### Градиентный бустинг решающих деревьев.

$$
\begin{gather*}
H_{t + 1}(X) = H_t(X) + h_{t + 1}(X) \to y \Rightarrow \\
\Rightarrow h_{t + 1}(X) \to y - H_t(X)
\end{gather*}
$$

Можно добавить скорость обучения: $H_{t + 1}(X) = H_t(X) + \alpha h_{t + 1}(X)$.

**XGBoost** (eXtreme Gradient Boosting): $H_t(X) = \sum_{i=1}^t h_i(X)$.

$$
E_t = \sum_{i=1}^N L(H_t(X_i), y_i) + \sum_{j=1}^t \Omega(h_j) \to \min
$$

Где $\Omega$ — регуляризация. В общем случае с разложением по Тейлору:

$$
\begin{gather*}
E_t = \sum_{i=1}^N \left( L(H_{t - 1}(X_i), y_i + u_i h_t(X_i) + \frac{1}{2} (v_i(h_t(X_i)))^2 \right) + \Omega(h_t) + \text{Const} \\
u_i = \delta_{H_{t - 1}(X_i)}(L(H_{t - 1}(X_i), y_i)) \\
u_i = \delta^2_{H_{t - 1}(X_i)}(L(H_{t - 1}(X_i), y_i)) \\
\end{gather*}
$$

На $t$-й гипотезе минимизируем

$$
\begin{gather*}
E_t = \sum_{i=1}^N \left( u_i h_i(X_i) + \frac{1}{2} (v_i(h_t(X_i)))^2 \right) + \Omega(h_t) \to \min \\
\Omega(f) = \gamma M + \frac{1}{2} \lambda \sum_{j=1}^M w_j^2 \\
\end{gather*}
$$

$M$ — количество листьев, $w_j$ — выход листа $j$.

$$
E_t = \sum_{i=1}^N \left( u_i w_{q(X_i)} + \frac{1}{2} (v_i(w_{q(X_i)}))^2 \right) + \gamma M + \frac{1}{2} \lambda \sum_{j=1}^M w_j^2 \to \min
$$

$q(X_i)$ --- лист, соответствующий $X_i$. Группируем по листьям:

$$
\begin{gather*}
E_t = \sum_{j=1}^M \left( \sum_{q(X_i) = j} u_i w_j + \frac{1}{2} \left( \sum_{q(X_i) = j} v_i + \lambda \right) w_j^2 \right) + \gamma M \\
U_j = \sum_{q(X_i) = j} u_i \quad
V_j = \sum_{q(X_i) = j} v_i \\
E_t = \sum_{j=1}^M \left( U_j w_j + \frac{1}{2} (V_j + \lambda) w_j^2 \right) + \gamma M \\
\end{gather*}
$$

$$
\begin{gather*}
w_j^\text{opt} = \frac{-U_j}{V_j + \lambda} \\
E_t^\text{opt} = -\frac{1}{2} \sum_{j=1}^M \frac{U_j^2}{V_j + \lambda} + \gamma M \\
\text{Gain} = \frac{1}{2} \left[ \frac{U_L^2}{V_L + \lambda} + \frac{U_R^2}{V_R + \lambda} - \frac{(U_L + U_R)^2}{V_L + V_R + \lambda} \right] \\
\end{gather*}
$$

### Кластеризация. Agglomerative Clustering. Метрики кластеризации.
**Agglomerative clustering:** изначально каждая точка в своём кластере. Затем объединяются кластеры с наименьшим значением одной из метрик (обозначим кластеры как множества точек $A$ и $B$):
- Average — $\text{mean}(\rho(x, y)) \mid (x, y) \in A \times B$, среднее расстояние между точками в двух кластерах
- Single — $\min(\rho(x, y)) \mid (x, y) \in A \times B$, минимальное расстояние между точками в двух кластерах
- Complete — $\max(\rho(x, y)) \mid (x, y) \in A \times B$, максимальное расстояние между точками в двух кластерах
- Ward — дисперсия объединяемых кластеров
Цикл повторяется до достижения нужного количества кластеров.

Работает _очень_ медленно.

**Метрики кластеризации:**
- Внешние метрики
- **Homogenity score:**

$$
\frac{1}{|D|} \sum_i \max_y |X_j \in C_i, y_j = y|
$$

- **Silhouette coefficient:** больше — лучше

$$
s = \frac{b - a}{\max(a, b)}
$$

$a$ — среднее расстояние между точкой и всеми остальными точками в её кластере

$b$ — среднее расстояние между точкой и всеми точками в ближайшем другом кластере

- **Dunn index:** больше — лучше

$$
D = \frac{\min_{i \neq j} \rho(\mu_i, \mu_j)}{\max_{X_i, X_j \in \mu} \rho(X_i, X_j)}
$$

- **Davies-Bouldin index:** меньше — лучше. $\overline{\rho(\mu_i, X^i)}$ — среднее расстояние от центра до точек в $i$-м кластере

$$
\text{DB} = \frac{1}{k} \sum_{i=1}^k \max_{j \neq i} \left( \frac{\overline{\rho(\mu_i, X^i)} + \overline{\rho(\mu_j, X^j)}}{\rho(\mu_i, \mu_j)} \right)
$$

## Билет 6
### Оценка классификации. Эффективность по Парето. Precision-Recall и ROC кривые. AUC.
См. лекцию 2.

|             | Правильный ответ — True         | Правильный ответ — False |                                           |
|-------------|---------------------------------|--------------------------|-------------------------------------------|
| Ответ True  | True Positive                   | False Positive           | **Precision** = Positive Predictive Value |
| Ответ False | False Negative                  | True Negative            |                                           |
|             | **Recall** = True Positive Rate | False Positive Rate      | **Accuracy**                              |

$$
\begin{aligned}
\text{Precision} & = \frac{TP}{TP + FP} \\
\text{Recall} & = \frac{TP}{TP + FN} \\
\end{aligned}
$$

Precision (точность) и Recall (отклик) считаются отдельно для каждого класса, Accuracy (доля попаданий) — можно для всех вместе:

$$
\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}
$$

**F1-score** — среднее гармоническое между Precision и Recall

$$
\begin{aligned}
F_1 = \frac{2}{\text{Precision}^{-1} + \text{Recall}^{-1}} = \frac{2 \cdot TP}{2 \cdot TP + FN + FP}
\end{aligned}
$$

Очевидно, $0 \leq F_1 \leq 1$.

**Pareto efficiency**: классификатор эффективен по Парето, если не существует классификатора лучше одновременно и по Precision, и по Recall. Увеличение одного приводит к уменьшению другого.

При повышении границы (threshold) повышается Precision, понижается Recall. На графике Precision / Recall двигаемся к увеличению точности и уменьшению отклика, на Recall / False Positive Rate — к уменьшению обоих.

**ROC (Receiver Operating Characteristic) Curve** — график Recall / False Positive Rate. **AUC** — Area Under \[ROC\] Curve. AUC — метрика классификатора, 0.5 — случайный классификатор, 1 — идеальный. Если инвертировать классификатор, то его AUC отразится от 0.5.

### Векторное представление слов. Word2Vec. Transformer.
**word2vec** — преобразование слов в вектора.

$$
P(w_t | w_c) = \frac{\exp(f(w_t, w_c))}{\sum_{w_i \in \text{Dict}} \exp(f(w_i, w_c))}
$$

То есть SoftMax от какой-то функции. Функция потерь — по правдоподобию. Простейшее $f$ берётся таким:

$$
f(w_t, w_c) = u^T_{w_t} v_{w_c}
$$

где $u$ и $v$ — вектора для слова-таргета и слова-контекста соответственно. Изначально — случайны для всех слов словаря. Затем проходом по тексту для каждой пары считается эта вероятность (см. выше). Реальное событие "слово находится в контексте другого слова" считается по тому, как часто они встречаются рядом.

В результате после обучения $u$ и $v$ получаются векторные представления слов — **эмбеддинг.**

Negative sampling: вместо всего словаря берётся сколько-то случайных слов (уже в подсчёте градиента). По теории вероятности они должны встречаться в целом редко.

**CBOW (Continuous Bag Of Words)** — см. слайды лекции 8. Контекст — окружающие слова, а не центральные, предсказываются коэффициенты. Предсказание центрального слова по рядом стоящим.

**Skip-gram** — предсказание рядом стоящих слов по центральному.

Чудесным образом появляются геометрические отношения между смыслами, например, $f(\text{king}) - f(\text{queen}) \sim f(\text{man}) - f(\text{woman})$, или $f(\text{walking}) - f(\text{walked}) \sim f(\text{swimming}) - f(\text{swam})$.

**Fasttext** — использовать n-граммы вместо слов, например, where ⇒ wh + whe + her + ere + re.

**Рекуррентные нейросети:** выход предыдущего шага подаётся на вход следующему.

$$
\begin{gather*}
H_{t + 1} = W_\to \sigma(H_t) + W_{\uparrow} (X_t) \\
\widehat{y}_{t + 1} = W \sigma(H_t)
\end{gather*}
$$

Если $W$ слишком большое, то градиент взрывается, слишком маленькое — затухает. Решение -— **LSTM (Long-Short Term Memory):** часть предыдущего выхода "забывается" в соответствии с новым предыдущим выходом и новым входом. Новый выход зависит от запомненных предыдущих выходов и нового входа.

$$
\begin{aligned}
f_t & = \sigma \left( W_f \cdot \left[ Y_{t - 1}, X_t \right] + b_f \right) \\
i_t & = \sigma \left( W_i \cdot \left[ Y_{t - 1}, X_t \right] + b_i \right) \\
o_t & = \sigma \left( W_o \cdot \left[ Y_{t - 1}, X_t \right] + b_o \right) \\
\widetilde{C}_t & = \tanh \left( W_c \cdot \left[ Y_{t - 1}, X_t \right] + b_C \right) \\
C_t & = f_t C_{t - 1} + i_t \widetilde{C}_{t - 1} \\
y_t & = o_t \cdot \tanh (C_t) \\
\end{aligned}
$$

**Attention** — векторам входа присваивается вес, сумма весов — 1. Вес распределяется как-то. Похоже на "важность слов", на самом деле просто нейросеть работает как-то. Attention улучшает практически все нейросети. Визуальное внимание тоже существует.

**Transformer** — преобразует один поток векторов в другой поток векторов с помощью рекуррентной нейросети.

**Self-attention** — расширение Attention до матрицы. Считается важность каждого слова для каждого слова. $W^Q$ — queries, $W^K$ — keys, $W^V$ — values. Матрицы обучаются через backpropagation. $Q = X \cdot W^Q$, $K = X \cdot W^K$, $V = X \cdot W^V$.

$$
\text{SoftMax}\left( \frac{Q \cdot K^T}{\sqrt{d_k}} \right) \cdot V = Z
$$

$Z$ — результат Self-Attention.

**Multi-headed attention** — применяется self-attention с несколькими разными матрицами.

**Positional encoding**
- Уникальное кодирование для каждой позиции
- Расстояние между кодами должно соответствовать расстоянию между словами
- Обобщение на предложения любой длины
- Детерминированное

$$
\begin{aligned}
\text{PE}(\text{pos}, 2i) & = \sin(\text{pos} / 10000^{2i / d_\text{model}}) \\
\text{PE}(\text{pos}, 2i + 1) & = \cos(\text{pos} / 10000^{2i / d_\text{model}}) \\
\end{aligned}
$$

Обучение трансформера: скрыть слово в настоящем переводе и пытаться его предсказать.

## Билет 7
### Локальный поиск. Hill Climb и его разновидности. Отжиг. Генетический алгоритм.
**Hill Climb** — аналог градиентного спуска, но есть только неизвестная недиффиренцируемая целевая функция, которую можно вычислить в конкретной точке. Маленькими шажками можно дойти до локального максимума.
- Стохастический: шаг с вероятностью, пропорциональной увеличению метрики (т.е. делаем шаги во всех направлениях, а потом с помощью, например, SoftMax, выбрать один)
    - **Отжиг** (annealing) — постепенно "нагреваем" и "охлаждаем" систему, добавляя температуру T. При высокой температуре вероятности, которые выдаёт SoftMax, стремятся к равномерным, при низкой — увеличивается вероятность максимума:

$$
P(s_i) = \frac{\exp\left(\frac{\Delta E(s_i)}{T}\right)}{\sum\exp\left(\frac{\Delta E(s_i)}{T}\right)}
$$

- Табу: нельзя возвращаться в предыдущую точку ни при каких обстоятельствах
- Рой частиц: множество частиц, взбирающихся параллельно, и обменивающихся информацией

**Генетический алгоритм** — ответ разбивается на "гены", затем "эволюционирует в популяции" по метрике.

### Метод опорных векторов. Прямая и двойственная задача. Решение двойственной задачи. Типы опорных векторов. Ядра.
**SVM** (Support Vector Machine, метод опорных векторов).

$$
\min |W^T X_i - b| = \min \left( y_i (W^T X_i - b) \right) = 1
$$

Тогда ширина полосы:

$$
\frac{W^T (X_i - X_j)}{||W||} = \frac{2}{||W||}
$$

Задача — максимизировать $2 / ||W||$, т.е. минимизировать $||W||_2^2 = W^T W$. Задача оптимизации:

$$
\left\{
\begin{aligned}
    & \frac{1}{2} W^T W \to \min \\
    & y_i (W^T X_i - b) \geq 1 \\
\end{aligned} \right.
$$

Можно преобразовать к двойственной задаче:

$$
\begin{gather*}
\left\{
\begin{aligned}
    & \frac{1}{2} W^T W \to \min \\
    & 1 - y_i (W^T X_i - b) \leq 0 \\
\end{aligned} \right. \\
\mathcal{L}(W, b, \alpha) = \frac{1}{2} W^T W - \sum_{i=1}^N \alpha_i \left( y_i (W^T X_i - b) - 1 \right) \\
\alpha_i \geq 0; \alpha_i \left( y_i (W^T X_i - b) - 1 \right) = 0 \\
\end{gather*}
$$

Решение двойственной задачи:

$$
\begin{gather*}
\nabla_W \mathcal{L}(W, b, \alpha) = W - \sum_{i=1}^N \alpha_i y_i X_i = \vec{0} \\
W = \sum_{i=1}^N \alpha_i y_i X_i \\
\frac{\delta}{\delta b} \mathcal{L}(W, b, \alpha) = \sum_{i=1}^N \alpha_i y_i = 0 \\
\mathcal{L}(W, b, \alpha) = \frac{1}{2} W^T W - \sum_{i=1}^N \alpha_i (y_i (W^T X_i) - b) - 1) = \\
= \frac{1}{2} \sum \sum y_i y_j \alpha_i \alpha_j X_i^T X_j - \\
- \sum \sum y_i y_j \alpha_i \alpha_j X_i^T X_j + \sum \alpha_i = \\
= \mathcal{L}(W, b, \alpha) = \sum_{i=1}^N \alpha_i - \frac{1}{2} \sum_{i,j=1}^N y_i y_j \alpha_i \alpha_j X_i^T X_j
\end{gather*}
$$

Решается квадратичным программированием. Ход решения в целом: начать с $\alpha_i = 0$, взять $\alpha_i$ и $\alpha_j$ такие, что $y_i y_j < 0$, и начинаем их обе увеличивать. Потом можно брать альфы одного знака и их двигать в разные стороны (альфы не могут быть меньше нуля). Условие для CVXOPT:

$$
\begin{gather*}
\left\{
\begin{aligned}
& \frac{1}{2} \alpha^T P \alpha + q^T \alpha \to \min \\
& G \alpha \leq h \\
& A \alpha = b \\
\end{aligned}
\right. \quad
\begin{array}{cc}
P = \left[ y_i y_j X_i^T X_j \right] & q = \left[ -1 \right] \\
G = -I & h = \left[ 0 \right] \\
A = Y^T & b = 0 \\
\end{array}
\end{gather*}
$$

Это работает при линейно разделимых классах. Классы могут быть линейно неразделимы. Тогда используют **ядра.** Ядерный трюк (kernel trick): можно не искать $\psi(X)$, а сразу искать $K(X, X') = \psi(X)^T \psi(X')$. Например:

$$
\begin{gather*}
K(X, X') = (1 + X^T X')^2 = 1 + x_1^2 x_1'^2 + x_2^2 x_2'^2 + 2x_1 x_1' + 2x_2 x_2' + 2x_1 x_1' x_2 x_2' \\
\psi(X) = (1, x_1^2, x_2^2, \sqrt{2} x_1, \sqrt{2} x_2, \sqrt{2} x_1 x_2) \\
\end{gather*}
$$

Но тогда пропадает $W$. Но, поскольку веса — это линейная комбинация опорных векторов, а номера опорных векторов известны, то по-прежнему можно посчитать $W^T X_i = \sum_j \alpha_j y_j K(X_j, X_i)$.

Иногда датасет линейно неразделим. Берём soft margin:

$$
\begin{gather*}
\left\{
\begin{aligned}
& \frac{1}{2} W^T W + C \sum_{i=1}^N \xi_i \to \min \\
& y_i (W^T X_i - b) \geq 1 - \xi_i \\
& \xi_i \geq 0 \\
\end{aligned}
\right. \\
\mathcal{L}(\alpha) = \sum \alpha_i - \frac{1}{2} \sum \sum y_i y_j \alpha_i \alpha_j X_i^T X_j \\
\left\{
\begin{aligned}
& \max_\alpha \mathcal{L}(\alpha) = \sum \alpha_i - \frac{1}{2} \sum \sum y_i y_j \alpha_i \alpha_j X_i^T X_j \\
& 0 \leq \alpha_i \leq C \\
& \sum \alpha_i y_i = 0 \\
\end{aligned}
\right.
\end{gather*}
$$

**Типы опорных векторов:**
- $\alpha_i = 0; \xi_i = 0; y_i (W^T X_i - b) \geq 1$ — внутренние вектора
- $0 < \alpha_i < C; \xi_i = 0; y_i (W^T X_i - b) = 1$ — "хорошие" опорные вектора, на полосе
- $\alpha_i = C; \xi_i > 0; y_i (W^T X_i - b) \leq 1$ — "плохие" опорные вектора, внутри полосы

## Билет 8
### Гипотезы и дихотомии. Функция роста. Точка поломки. Доказательство полиномиальности функции роста в присутствии точки поломки.
$E_\text{in}$ — ошибка в выборке. $E_\text{out}$ — ошибка вне выборки.

$$
\begin{aligned}
E_\text{in}(h) & = \frac{1}{N} \sum_{i=1}{N} e(h(X_i), f(X_i)) \\
E_\text{out}(h) & = \mathbb{E}_{X} \left[ e(h(X), f(X)) \right] \\
\end{aligned}
$$

**Hoeffding's inequality:** вероятность того, что разница между ошибкой в выборке и вне выборки превысит $\varepsilon$.

$$
P\left[ |E_\text{in}(h) - E_\text{out}(h)| > \varepsilon \right] \leq 2 \exp(-\varepsilon^2 N)
$$

Но если тестируем $M$ гипотез:

$$
P\left[ |E_\text{in}(h) - E_\text{out}(h)| > \varepsilon \right] \leq M \exp(-\varepsilon^2 N)
$$

Очевидно, ситуация ухудшается при $M \to +\infty$

- Гипотеза: $h : X \to \{ -1; 1 \}$
- Дихотомия: $h : \{ X_1; \ldots; X_N\} \to \{ -1; 1 \}$
- Всего дихотомий — не более $2^N$

**Функция роста** $m_H(N)$ — максимальное количество дихотомий. $m_H(N) \leq 2^N$.

$$
m_H(N) = \max_{X_1; \ldots; X_N} |H(X_1, \ldots, X_N)|
$$

**Неравенство Вапника-Червоненкиса:**

$$
P\left[ |E_\text{in}(h) - E_\text{out}(h)| > \varepsilon \right] \leq 4 m_H(2N) \exp \left(-\frac{\varepsilon N}{8} \right)
$$

Например, для двухмерного перцептрона и датасета с $X = \{ 0; 1 \}^2$ количество дихотомий будет $m_H = 14 < 16 = 2^{|X|}$. То есть для _двухмерного перцептрона_ **точка поломки** (где впервые появляется датасет с $m_H < 2^N$) — $k = 4$.

**Доказательство полиномиальности функции роста в присутствии точки поломки:**
- Рассматриваем множество $X = \{ x_1; \ldots; x_N \}$
- Количество дихотомий всего — $2^N$, не все из них возможны
- Пусть $B(N, k) = m_H(N)$ с точкой поломки $k$ — максимально возможное количество дихотомий такое, что ни для каких $k$ точек нет всех возможных $2^k$ дихотомий
    - $B(N, k) = \alpha + 2\beta$:
    - $\alpha$ таких, что дихотомия по $\{ X_1; \ldots; X_{N - 1} \}$ встречается один раз (не только в $\alpha$, а вообще уникальна)
    - Две по $\beta$ таких, что дихотомии по $\{ X_1; \ldots; X_{N - 1} \}$ не уникальны, а строки из разных $\beta$ отличаются только $h(X_N)$
- Лемма: $\alpha + \beta \leq B(N - 1, k)$, т.к. в $\alpha + \beta$ содержатся все дихотомии на $\{ X_1; \ldots; X_{N - 1} \}$
- Лемма: $\beta \leq B(N - 1, k - 1)$. Пусть это не так. Тогда найдётся $2^{k - 1}$ дихотомий по $(k - 1)$ переменных в одной $\beta$. Тогда они же есть и во второй. Следовательно, добавив $x_N$, получаем набор с $2^k$ дихотомий по $k$ переменных в исходном $B(N, k)$, то есть противоречие.
- Следовательно, $B(N, k) \leq B(N - 1, k - 1) + B(N - 1, k)$.
- Следовательно, $B(N, k) \leq \sum_{i=0}^{k-1} \binom{N}{i}$:

$$
\begin{aligned}
B(N, 1) & = 1 \\
B(1, k) & = 2 \\
B(N, k)
& \leq B(N - 1, k) + B(N - 1, k - 1) \leq \\
& \leq \sum_{i=0}^{k-1} \binom{N - 1}{i} + \sum_{i=0}^{k-2} \binom{N - 1}{i} = \\
& = 1 + \sum_{i=1}^{k-1} \binom{N - 1}{i} + \sum_{i=1}^{k-1} \binom{N - 1}{i - 1} = \\
& = 1 + \sum_{i=1}^{k-1} \binom{N}{i} = \\
& = \sum_{i=0}^{k-1} \binom{N}{i}
\end{aligned}
$$

А $\sum_{i=0}^{k-1} \binom{N}{i}$ — полином.

**Размерность Вапника-Черваненкиса (VC-dimension)** — $d_\text{VC} = (k - 1)$, где $k$ — точка поломки. Например, для плоского перцептрона $d_\text{VC} = 3$. Тогда

$$
m_H(N) \leq \sum_{i=0}^{d_\text{VC}} \binom{N}{i} \leq N^{d_\text{VC}} + 1
$$

Для d-мерного перцептрона размерность Вапника-Черваненкиса — (d + 1). Доказательство — через обратимую матрицу:

$$
X =
\left.
\underbrace{
\begin{bmatrix}
1      &      0 & 0      & \cdots & 0      \\
1      &      1 & 0      & \cdots & 0      \\
1      &      0 & 1      & \cdots & 0      \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
1      &      0 & 0      & \cdots & 1      \\
\end{bmatrix}
}_{d + 1}
\right\} d + 1
$$

Затем возьмём

$$
\begin{gather*}
X_{d + 2} = \sum_{i=1}^{d + 1} \alpha_i X_i \\
y_i = \text{sign}(\alpha_i) \\
y_{d + 2} = -1 \\
\end{gather*}
$$

Тогда $W^T X_{d + 2} = \sum_{i=1}^{d + 1} (W^T X_i \alpha_i \geq 0) \neq -1 = y_{d + 2}$, поскольку $W^T X_i \cdot \text{sign}(\alpha_i) = 1$

Позволяет оценить, сколько данных будет достаточно:

$$
\begin{gather*}
4 m_H(2N) \exp \left(-\frac{\varepsilon N}{8} \right) \approx N^{d_\text{VC}} \exp(-N) \\
N \geq 10 d_\text{VC} \\
\end{gather*}
$$

### Деревья решений. Прунинг. Небрежные решающие деревья. Нечеткие решающие деревья.
Регуляризация:
+ Ограничение глубины
+ Pruning — обрезание веток
    + Minimum error — пока не ухудшается функция потерь на валидационном датасете
    + Cost complexity — пока функция потерь на валидационном датасете ухудшается не более чем на $\alpha$. Т.е. новая функция потерь — $\text{error} + \alpha * \text{tree size}$

**Небрежные деревья** — на каждом уровне рассматривается один признак (фича).

**Нечёткие деревья** — проход по ветке — не окончательное решение, а вероятность в зависимости от уверенности.

## Билет 9
### Байесовский классификатор. Оценка признаков (Gaussian, Bernoulli, Multinomial). EM алгоритм.
**Байесовский классификатор:**

$$
\begin{gather*}
P(y | X) = \frac{P(y) P(X | y)}{P(X)} \\
y_\text{map} = \arg\max_{y \in Y} P(y | X) = \arg\max_{y \in Y} P(y) P(X | y) = \\
= \arg\max_{y \in Y} P(x_1, x_2, \ldots, x_n | y) P(y) \\
\end{gather*}
$$

При независимых признаках

$$
P(x_1, \ldots, x_n | y) = P(x_1 | y) \cdot \ldots \cdot P(x_n | y)
$$

Байесовский классификатор работает, когда признаков _очень много_. Наивный классификатор:

$$
y_\text{NB} = \arg\max_{y \in Y} P(y) \prod_i P(x_i | y)
$$

Если вероятность какого-то признака 0, то произведение целиком обнуляется. Решение — использовать сглаживание:

$$
\hat{P}(x_i | y_j) = \frac{\text{count}(x_i, y_j) + 1}{\text{count}(y_j) + K}
$$

Бинарные признаки:

$$
P(x_i | y) = P(x_i = 1 | y) x_i + (1 - P(x_i = 1 | y)) (1 - x_i) \mid x_i \in \{ 0; 1 \}
$$

Непрерывные признаки — нормальное распределение:

$$
p(x_i | y) = \left( \sqrt{2\pi \sigma_y^2} \right)^{-1} \exp\left( -\frac{(x_i - \mu_y)^2}{2\sigma_y^2} \right)
$$

**Expectation-maximization** (EM): $\mu_k$ — вектор средних, $\Sigma_k$ — матрица ковариации. $\alpha_k$ — вес распределения $k$, $\sum \alpha_k = 1$. Близость объекта $X_i$ к распределению $k$:

$$
w_{ik} = P(\mu_k, \Sigma_k | X_i) = \frac{p(X_i | \mu_k, \Sigma_k) \cdot \alpha_k}{\Sigma_j p_j (X_i | \mu_j, \Sigma_j) \cdot \alpha_j}
$$

E-шаг: посчитать $w_{ik}$.
M-шаг: пересчитать

$$
\begin{gather*}
\alpha_k^\text{new} = \frac{\sum_{i=1}^N w_{ik}}{N} = \frac{N_k}{N} \\
\mu_k^\text{new} = \frac{1}{N_k} \sum_{i=1}^N w_{ik} \cdot X_i \\
\Sigma_k^\text{new} = \frac{1}{N_k} \sum_{i=1}^N w_{ik} \cdot (X_i - \mu_k^\text{new}) (X_i - \mu_k^\text{new})^T \\
\end{gather*}
$$

Наивный байес — хороший базовый алгоритм.

### Нейронные сети. Перцептрон Розенблатта. Обратное распространение градиента. Функции активации. Softmax.
**Перцептрон Френка Розенблатта:** класс — знак в линейной регрессии. Алгоритм поиска весов:
1. Начать со случайными $W$
2. Для каждого $X_i \mid h(X_i) \neq y_i$ выполнить $W \gets W + y_i X_i$
Алгоритм выше сходится, если множества линейно разделимы.

Если неразделимы, то можно попытаться добавить признаков.

**Многослойный перцептрон** — выход предыдущего слоя — вход следующего. Функция активации по-прежнему знак.

**Логистическая функция (сигмоида):**

$$
\begin{gather*}
\sigma(x) = \frac{1}{1 + \exp(-x)} \\
\sigma(-x) = -\sigma(x) \\
0 < \sigma(x) < 1 \\
\end{gather*}
$$

**Логистическая регрессия:** $P(y \mid X) = \sigma(y W^T X)$

**Правдоподобие (likelihood):**

$$
\begin{gather*}
\prod_{i=1}^N P(y_i \mid X_i) = \prod_{i=1}^N \sigma(y_i W^T X_i) \\
\begin{aligned}
L(W)
& = -\frac{1}{N} \ln \left( \prod_{i=1}^N \sigma(y_i W^T X_i) \right) = \\
& = \frac{1}{N} \sum_{i=1}^N \ln \left( \frac{1}{\sigma(y_i W^T X_i)} \right) = \\
& = \frac{1}{N} \sum_{i=1}^N \ln \left( 1 + \exp(-y_i W^T X_i) \right) \\
\end{aligned}
\end{gather*}
$$

Если использовать логистическую функцию активации, то можно использовать градиентный спуск, т.е. $w_k \gets w_k - \eta \cdot (\delta L / \delta w_k)$.

Обратное распространение градиента:

$$
\begin{aligned}
y & = f(x) \\
z & = g(y) \\
\frac{\delta z}{\delta y} & = g'(y) \\
\frac{\delta y}{\delta x} & = f'(x) \\
\frac{\delta z}{\delta x} & = \frac{\delta z}{\delta y} \cdot \frac{\delta y}{\delta x} = g'(y) \cdot f'(x) \\
\end{aligned}
$$

Другие функции активации:

$$
\begin{aligned}
\tanh(x) & = 2\sigma(x) - 1 \\
\text{ReLU}(x) & = \max(0, x) \\
\end{aligned}
$$

**SoftMax** полезен, когда пишем многоклассовый классификатор:

$$
\text{SoftMax}(x_j) = \frac{\exp(x_j)}{\sum_i \exp(x_i)}
$$

**Кросс-энтропия:**

$$
L(P) = -\sum_i [y = i] \ln(p_i)
$$

Градиент кросс-энтропии после SoftMax:

$$
\begin{gather*}
p_k = \text{SoftMax(X)}_k = \frac{\exp(x_k)}{\sum_i \exp(x_i)} \\
\begin{aligned}
\frac{\delta L}{\delta p_k}
& =
\begin{cases}
    -\ln'(p_k) & y = k \\
    0 & y \neq k \\
\end{cases}
& = \frac{-(y = k)}{p_k} \\
\end{aligned} \\
\begin{aligned}
\frac{\delta p_k}{\delta x_i}
& =
\begin{cases}
    \frac{\exp(x_k) \sum_j \exp(x_j) - \exp(x_k) \exp(x_i)}{\left( \sum_j \exp(x_j) \right)^2} & k = i \\
    \frac{-\exp(x_k) \exp(x_i)}{\left( \sum_j \exp(x_j) \right)^2} & k \neq i \\
\end{cases} \\
& = p_k ((k = i) - \frac{\exp(x_i)}{\sum_j \exp(x_j)}
\end{aligned} \\
\begin{aligned}
\frac{\delta L}{\delta x_i}
& = \sum_k \frac{\delta L}{\delta p_k} \cdot \frac{\delta p_k}{\delta x_i} = \\
& = \frac{\delta L}{\delta p_y} \cdot \frac{\delta p_y}{\delta x_i} = \\
& = \frac{-1}{p_y} \cdot p_y \cdot ((k = i) - p_i) = \\
& = p_i - (y = i)
\end{aligned}
\end{gather*}
$$

Регуляризации:
- L2
- Dropout — выкидывать часть нейронов во время обучения, обычно половину, не забывать масштабировать веса

## Билет 10
### Свёрточные нейронные сети. VGG. ResNet. Трансферное обучение.
**Свёртка** — слой нейросети, преобразующий "прямоугольный" участок входного изображения в "пиксель" выходного изображения (каналы — сразу все со всеми). Лучше полносвязного слоя тем, что меньше и быстрее учится. Гиперпараметры: количество каналов ввода и вывода, размер ядра (kernel size), шаг (stride), дополнительная толщина границы (padding).

Есть специальные свёртки — pooling — которые вычисляют результат не линейно, а по другой функции, например, извлечением минимума / максимума / медианы.

**AlexNet** — одна из первых свёрточных нейронных сетей:
- Масштабирует до 256 на 256, берёт случайные участки 224 на 224 и отражает их
- Вычитает из каждого пикселя среднее по всем пикселям
- ReLU
- Dropout 0.5
- Batch size 128
- SGD с массой 0.9
- L2 с коэффициентом 0.0005

**VGG** (2014) от лаборатории Visual Geometry Group — Very Deep Convolutional Networks
- Свёртки только 3 на 3 полностью заменяют все остальные
- MaxPool
- Ввод 224 на 224
- 11, 13, 16 или 19 весовых свёрток
- В конце FC-4096, FC-4096, FC-1000, soft-max

**ResNet** (2015)
- Использует skip connections — результаты свёрток отправляются не только в следующие свёртки, но и дальше
- Достиг ошибки в 3.57%, что ниже ошибки человека
- Дальше соревноваться на ImageNet бесполезно, потому что сам датасет содержит человеческую ошибку

**Трансферное обучение:**
- Берётся обученная нейросеть
- Часть слоёв (обычно последние) заменяются на более новые
- Проводится дообучение, старые слои замораживаются
- В результате — очень быстрое обучение
- Заморозка опциональна, можно "файн тюнить"

**U-Net:**
- Сегментация изображений
- Выходы слоёв пробрасываются помимо следующего слоя ещё в самый конец, к тому же разрешению
- Посередине — полносвязные слои на маленьких изображениях

### Метрические классификаторы. kNN. WkNN. Отбор эталонов. DROP5. KDtree.
**kNN (k Nearest Neighbors)** — ответ для точки определяется голосованием ответов её $k$ ближайших соседей из обучающей выборки. При этом для работы метода достаточно существования функции расстояния.

$$
\begin{gather*}
h(X, D, k) = \arg\max_{y \in Y} \sum_{X_i \in D} \left[ y_i = y \right] w(X_i, X, k) \\
w(X_i, x, k) = 1 \text{, если $X_i$ --- один из $k$ ближайших соседей x} \\
\text{или} \\
w(X_i, x, k) = 1 \text{, если $\rho(X_i, x) \leq \text{радиус $k$-го соседа}$ } \\
\end{gather*}
$$

Метрика ошибки kNN: **leave-one-out error** — для каждой точки определяется её ответ, если её убрать из обучающей выборки, после этого считается точность (accuracy) классификации (т.к. функция ошибки, то инвертированная точность). Выбирается такое k, при котором достигается наилучшая точность (т.е. наименьшая ошибка).

$$
\text{LOO}(k, D) = \frac{\sum_{X_i \in D}\left[ h(X_i, D \backslash X_i, k) \neq y_i \right]}{|D|}
$$

**WkNN (Weighted k Nearest Neighbors)** — вводится **ядро**, например

$$
\begin{aligned}
w_i & = \left[ \frac{r - \rho(X, X_i)}{r} \right]_+ \text{ --- треугольное ядро} \\
w_i & = q^{-\rho(X, X_i)} \text{ --- гауссово ядро} \\
\end{aligned}
$$

То есть убывающая неотрицательная функция от расстояния, в идеале — обращающаяся в 0 при достижении r. Ядро используется для вычисления весов в голосовании.

**Проклятие размерности:** рассмотрим распределённые в гиперкубе 0..1 точек. В 3-мерном кубе 0.1% точек попадает в куб со стороной 0.1, в то время как в 100-мерном — со стороной 0.93.
Решение: использовать пошаговый kNN, то есть отсортировать фичи и увеличивать количество рассматриваемых расстояний, пока точность улучшается.

**KDTree (k-dimensional Tree)** — для быстрого поиска ближайших соседей можно разбить пространство по медиане какой-нибудь координаты. Очевидно, что если расстояние от точки до "многомерного прямоугольника" больше, чем до найденного $k$-го ближайшего соседа, то улучшить результат какой-то точкой в этом прямоугольнике уже невозможно.
Такая эвристика позволяет многократно сократить время поиска ближайших соседей: сначала находим прямоугольник-лист, в который попадает точка, ищем соседей в нём, потом поднимаемся по дереву и ищем в нерассмотренных поддеревьях, сразу пропуская всё поддерево целиком, если расстояние слишком большое.
Разбиение ветки стоит делать по медиане координаты (для более сбалансированного дерева), и пока _наименьшее_ количество точек в поддереве больше заданного порога.

**Отбор эталонов (prototype selection):** иногда невозможно сохранить весь датасет (например, когда он большой), нужно сделать обучающую выборку по маленькой части. Есть разные способы выбрать, по какой именно.

**DROP5** — метод отбора эталонов:
1. Начать с полного датасета
2. Отсортировать точки по возрастанию близости до неправильного класса (по формуле kNN)
3. Пройти по отсортированному массиву. Точку **x** можно удалить, если это не приведёт к ухудшению **LOO** для тех точек, которые считают **x** одним из своих ближайших соседей