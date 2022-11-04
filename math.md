Теги: #math
# Суммы прогрессий
## Сумма чисел от 1 до n
Очевидно, что

$$
\sum_{k=1}^1 k = 1 = \frac{1 \cdot (1 + 1)}{2}
$$

Пусть доказано для $n$, что

$$
\sum_{k=1}^n k = \frac{n (n + 1)}{2}
$$

Докажем для $n + 1$:

$$
\sum_{k=1}^{n + 1} k = (n + 1) + \sum_{k=1}^n k =
$$

$$
= (n + 1) + \frac{n(n + 1)}{2} =
$$

$$
= \frac{(n + 1)(n + 2)}{2}
$$

Тогда по индукции

$$
\forall n \in \mathbb{N} : \sum_{k=1}^n k = \frac{n (n + 1)}{2}
$$

## Сумма квадратов от 1 до n
Теги: #math #infiniteSum
По индукции. Очевидно, что

$$
\sum_{k=1}^1 k^2 = 1 = \frac{2n+1}{3} \frac{n(n + 1)}{2}
$$

Пусть для $n$ доказано, что

$$
\sum_{k=1}^n k^2 = \frac{n(n+1)(2n+1)}{6}
$$

Тогда для $n+1$:

$$
\sum_{k=1}^{n+1} k^2 = (n+1)^2 + \frac{n(n+1)(2n+1)}{6} =
$$

$$
= \frac{(n + 1)(n(2n + 1) + 6(n + 1))}{6} =
$$

$$
= \frac{(n + 1)(2n^2 + 7n + 6)}{6} =
$$

$$
= \frac{(n + 1)(n + 2)(2n + 3)}{6}
$$

Тогда по индукции

$$
\forall n \in \mathbb{N} : \sum_{k=1}^n k^2 = \frac{n(n + 1)(2n + 1)}{6}
$$

## Сумма кубов от 1 до n
Очевидно, что

$$
\sum_{k=1}^1 k^3 = 1 = \left( \frac{1^2 \cdot (1 + 1)^2}{4} \right)
$$

Пусть доказано для $n$, что

$$
\sum_{k=1}^n k^3 = \frac{n^2 (n + 1)^2}{4}
$$

Докажем для $n + 1$:

$$
\sum_{k=1}^{n + 1} k^3 = (n + 1)^3 + \frac{n^2 (n + 1)^2}{4} =
$$

$$
= \frac{(n + 1)^2 (n^2 + 4 (n + 1))}{4} =
$$

$$
= \frac{(n + 1)^2 (n^2 + 4n + 4)}{4} =
$$

$$
= \frac{(n + 1)^2 (n + 2)^2}{4}
$$

Тогда по индукции

$$
\forall n \in \mathbb{N} : \sum_{k=1}^n k^3 = \frac{n^2 (n + 1)^2}{4}
$$

## Сумма бесконечной геометрической прогрессии
Теги: #math #infiniteSum

$$
\sum_{k=0}^{\infty} q^k = 1 + \sum_{k=1}^{\infty} q^k = 1 + q \cdot \sum_{k=0}^{\infty} q^k
$$

$$
(1 - q) \cdot \sum_{k=0}^{\infty} q^k = 1
$$

$$
\sum_{k=0}^{\infty} q^k = \frac{1}{1 - q}
$$

## Сумма конечной геометрической прогрессии
Теги: #math #finiteSum

$$
\sum_{k=0}^n q^k = \sum_{k=0}^{\infty} q^k - q^{n + 1} \cdot \sum_{k=0}^{\infty} q^k =
$$

$$
= \frac{1}{1 - q} - \frac{q^{n + 1}}{1 - q} = \frac{1 - q^{n + 1}}{1 - q}
$$

$$
\sum_{k=1}^n q^k = q \cdot \frac{1 - q^n}{1 - q}
$$

## Сумма бесконечной (k q^k)
Теги: #math #infiniteSum

$$
\sum_{k=1}^{\infty} k \cdot q^k = \sum_{k=1}^{\infty} q^k + \sum_{k=2}^{\infty} (k - 1) \cdot q^k =
$$

$$
= \frac{q}{1 - q} + q \cdot \sum_{k=1}^{\infty} k \cdot q^k
$$

$$
(1 - q) \cdot \sum_{k=1}^{\infty} k \cdot q^k = \frac{q}{1 - q}
$$

$$
\sum_{k=1}^{\infty} k \cdot q^k = \frac{q}{(1 - q)^2}
$$

## Сумма конечной (k q^k)
Теги: #math #finiteSum

$$
\sum_{k=1}^n k \cdot q^k = \sum_{k=1}^{\infty} k \cdot q^k - \sum_{k=n+1}^{\infty} k \cdot q^k =
$$

$$
= \sum_{k=1}^{\infty} k \cdot q^k - \sum_{k=1}^{\infty} (k + n) \cdot q^k =
$$

$$
= \sum_{k=1}^{\infty} k \cdot q^k - \sum_{k=1}^{\infty} (k + n) \cdot q^{k + n} =
$$

$$
= \sum_{k=1}^{\infty} k \cdot q^k - q^n \cdot \sum_{k=1}^{\infty} k \cdot q^k - q^n \cdot \sum_{k=1}^{\infty} n \cdot q^k =
$$

$$
= \frac{q}{(1 - q)^2} - q^n \cdot \frac{q}{(1 - q)^2} - q^n \cdot \frac{nq}{1 - q} =
$$

$$
= q \cdot \frac{1 - q^n - n \cdot q^n \cdot (1 - q)}{(1 - q)^2} =
$$

$$
= q \cdot \frac{1 - (1 + n) \cdot q^n + n \cdot q^{n + 1}}{(1 - q)^2}
$$

# Линейная регрессия
Теги: #math #ml #linearRegression

$$
\hat{y}_k = x_k^T \vec{w'} + b
$$

$$
\hat{X} =
    \begin{bmatrix}
        x_{1 1} & x_{1 2} & \dots  & x_{1 m} & 1      \\
        x_{2 1} & x_{2 2} & \dots  & x_{2 m} & 1      \\
        \vdots  & \vdots  & \ddots & \vdots  & \vdots \\
        x_{n 1} & x_{n 2} & \dots  & x_{n m} & 1      \\
    \end{bmatrix}
$$

$$
\hat{Y} = \hat{X} \cdot \vec{w}
$$

## MSE

Теги: #math #ml #metric #mse

$$
\text{MSE} = \frac{1}{n} \sum_{k=1}^n (y_k - \hat{y}_k)^2
$$

## R^2
Теги: #math #ml #metric #r2

$$
\overline{y} = \frac{1}{n} \sum_{k=1}^n y_k
$$

$$
SS_\text{res} = \sum_k (y_k - \hat{y}_k)^2
$$

$$
SS_\text{tot} = \sum_k (y_k - \overline{y})
$$

$$
R^2 = 1 - \frac{SS_\text{res}}{SS_\text{tot}}
$$

## Решение MSE
Теги: #math #ml #linearRegression #mse

$$
\text{MSE} = \min
$$

$$
\frac{1}{n} \sum_{k=1}^n (\hat{y}_k - y_k)^2 = \min
$$

$$
\sum_k (\hat{y}_k - y_k)^2 = \min
$$

$$
E = \hat{Y} - Y = \hat{X} \vec{w} - Y
$$

$$
E^T E = \min
$$

$$
\nabla \left( E^T E \right) = \vec{0}
$$

$$
\forall j : \frac{d}{dw_j} \left( E^T E \right) = 0
$$

$$
\forall j : \frac{d}{dw_j} \left( \left( \hat{X} \vec{w} - Y \right)^2 \right) = 0
$$

$$
\forall j : \frac{d}{dw_j} \left( \left( \sum_k \hat{X}_k \cdot \vec{w} - y_k \right)^2 \right) = 0
$$

$$
\forall j : \sum_k \frac{d}{dw_j} \left( \left( \hat{X}_k \cdot \vec{w} - y_k \right)^2 \right) = 0
$$

$$
\forall j : \sum_k 2 (\hat{X}_k \cdot \vec{w} - y_k) \hat{X}_{kj} = 0
$$

$$
\forall j : \sum_k (\hat{X} \vec{w} - Y)_k \hat{X}_{kj} = 0
$$

$$
\hat{X}^T (\hat{X} \vec{w} - Y) = \vec{0}
$$

$$
\hat{X}^T \hat{X} \vec{w} = \hat{X}^T Y
$$

$$
\vec{w} = \left( \hat{X}^T \hat{X} \right)^{-1} \hat{X}^T Y
$$

Решаем методом Гаусса, те веса, для которых получаем неопределённость, приравниваем к 0.

# Градиент кроссэнтропии
Теги: #math #ml #gradient #NeuralNetworks #CrossEntropy #SoftMax

Рассматриваем один объект с производными фичами $x_k$. $p_c$ — вероятность того, что этот объект попадает в класс $c$, $y$ — реальный класс объекта, $L$ — функция потерь всего, $L_c$ — функция потерь по классу $c$ (то есть $L$, если $y = c$, и $0$ в противном случае).

$$
p_c = \frac{\exp(x_c)}{\sum_k \exp(x_k)}
$$

$$
L = \sum_c L_c = -\sum_c (y = c) \ln p_c
$$

$$
\frac{dL}{dp_c} = \frac{dL_c}{dp_c} = \frac{-(y = c)}{p_c}
$$

$$
\frac{dp_c}{dx_j} = \frac{d}{dx_j} \frac{\exp(x_c)}{\sum_k \exp(x_k)} =
$$

$$
= \frac{\sum_k \exp(x_k) \cdot \frac{d}{dx_j} \exp(x_c) - \exp(x_c) \cdot \frac{d}{dx_j} \sum_k \exp(x_k)}{\left(\sum_k \exp(x_k) \right)^2} =
$$

$$
= \frac{(j = c) \exp(x_c)}{\sum_k \exp(x_k)} - \frac{\exp(x_c)}{\sum_k \exp(x_k)} \cdot \frac{\exp(x_j)}{\sum_k \exp(x_k)} =
$$

$$
= (j = c) p_c - p_c p_j = p_c ((j = c) - p_j)
$$

$$
\frac{dL}{dx_j} = \frac{dL_y}{dx_j} = \frac{dL_y}{dp_y} \cdot \frac{dp_y}{dx_j} =
$$

$$
= \frac{-1}{p_y} \cdot p_y \cdot ((j = y) - p_j) = p_j - (j = y)
$$
