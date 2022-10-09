Теги: #math
# Суммы прогрессий
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
\hat{X}^T (\hat{X} \vec{w} - Y) = 0
$$

$$
\hat{X}^T \hat{X} \vec{w} = \hat{X}^T Y
$$

$$
\vec{w} = \left( \hat{X}^T \hat{X} \right)^{-1} \hat{X}^T Y
$$
Решаем методом Гаусса, те веса, для которых получаем неопределённость, приравниваем к 0.
