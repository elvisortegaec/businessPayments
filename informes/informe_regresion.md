### Informe de predicción utilizando regresión

Este informe tiene como objetivo analizar y predecir el comportamiento de las transacciones en la plataforma Business Payments, utilizando modelos de regresión lineal y explorando diferentes variables predictoras. La finalidad es mejorar la capacidad de la plataforma para anticipar incidencias y optimizar la gestión de transacciones.

**Variables Predictoras**:

##### Variables Numéricas:

    'id': Identificador único de las transacciones.
    'amount': Monto asociado a las transacciones.
    'user_id': Identificador de los usuarios.
    'deleted_account_id': ID de cuentas eliminadas.
    'id_fee': Identificador de tarifas/comisiones.
    'cash_request_id': ID relacionado con solicitudes de efectivo.
    'total_amount_fee': Total de tarifas/comisiones aplicadas.

##### Variables Categóricas (dtype object):

    'created_at': Fecha de creación de la transacción.
    'updated_at': Fecha de última actualización de la transacción.
    'moderated_at': Fecha en la que la transacción fue moderada (si aplica).
    'reimbursement_date': Fecha de reembolso.
    'cash_request_received_date': Fecha de recepción de la solicitud de efectivo.
    'money_back_date': Fecha en que se devolvió el dinero.
    'transfer_type': Tipo de transferencia (e.g., transferencia bancaria, débito directo).
    'send_at': Fecha en que se envió el dinero.
    'type_fee': Tipo de tarifa aplicada.
    'status_fee': Estado de la tarifa (e.g., pagada, pendiente).
    'category_fee': Categoría de la tarifa (si aplica).
    'reason_fee': Razón asociada a la tarifa.
    'created_at_fee': Fecha de creación de la tarifa.
    'updated_at_fee': Fecha de última actualización de la tarifa.
    'paid_at_fee': Fecha en la que se pagó la tarifa.
    'from_date_fee': Fecha de inicio de la aplicación de la tarifa.
    'to_date_fee': Fecha de finalización de la aplicación de la tarifa.
    'charge_moment_fee': Momento de cobro de la tarifa.

**Variables a predecir**:

##### "recovery_status"

Buscaremos predecir esta variable para anticipar y controlar posibles incidencias futuras relacionadas con devoluciones o recuperación de fondos.

##### "status"

El análisis se centrará en prever el estado futuro de las transacciones. Los posibles estados incluyen:

    0: 'approved', 
    1: 'money_sent', 
    2: 'rejected', 
    3: 'pending', 
    4: 'transaction_declined', 
    5: 'waiting_user_confirmation'

**Matriz de correlación variables numéricas**:

![Matriz de correlación de las variables numéricas](https://github.com/arboldeku/businessPayments/blob/regresion/graficos_y_salidas/matriz_correlacion_numericas.png?raw=true)

Podemos ver que los campos que están relacionados son los de ID, variables que a nivel predictorio nos aportan poca capacidad predictiva.

**Comportamiento recovery_status**:

recovery_status	recovery_status_encoded

    98	0
    pending	3
    completed	2
    pending_direct_debit	4
    cancelled	1
    
##### Explicación:

    recovery_status: Esta es la variable original, que contiene los estados de la transacción, como "98", "pending", "completed", "pending_direct_debit", y "cancelled".

    recovery_status_encoded: Es el valor numérico asignado a cada categoría, como resultado del Label Encoding. Este proceso asigna un número único a cada categoría en función de su orden en los datos.

##### Mapeo entre las categorías y sus valores numéricos:

    "98": Ha sido codificado como 0.
    "pending": Ha sido codificado como 3.
    "completed": Ha sido codificado como 2.
    "pending_direct_debit": Ha sido codificado como 4.
    "cancelled": Ha sido codificado como 1.

##### Consideraciones:

    - El proceso de Label Encoding asigna un número entero a cada categoría. Sin embargo, el valor numérico no tiene un significado inherente más allá de representar una categoría.
    
    - En este caso, los valores numéricos simplemente son representaciones para que las categorías puedan ser procesadas por algoritmos de machine learning, que no entienden valores categóricos.
    
##### Modelo de Mínimos Cuadrados Ordinarios:
    
![Salida del modelo de MCO](https://github.com/arboldeku/businessPayments/blob/regresion/graficos_y_salidas/modelo_mco.PNG?raw=true)

**R-squared**: 0.401 Este valor indica que el 40.1% de la variabilidad en la variable dependiente (recovery_status_encoded) está explicada por las variables independientes en el modelo. Aunque no es un valor extremadamente alto, muestra que algunas de las variables que estás utilizando tienen una relación significativa con el estado de recuperación.

El R² ajustado penaliza el R² por el número de variables predictoras en el modelo. Es útil cuando se tiene un modelo con muchas variables. En este caso, el valor es casi igual al R², lo que indica que las variables adicionales no están introduciendo sobreajuste en el modelo.

Si el **p-valor** es menor que 0.05, podemos decir que el coeficiente es significativo.

Las variables: 

    amount 
    type_fee_instant_payment
    status_fee_cancelled
    status_fee_rejected
    category_fee_month_delay_on_payment
    category_fee_rejected_direct_debit 

Tienen p-valores muy bajos (0.000), lo que indica que son estadísticamente significativas.
La variable **status_fee_confirmed** tiene un p-valor de 0.974, lo que sugiere que status_fee_confirmed no tiene un impacto significativo en la variable dependiente.

##### Regresión lineal y datos de prueba / entrenamiento:

    Error Cuadrático Medio (MSE): 0.7876582240511442
    R2: 0.35304526580937523
    Coeficientes: [-0.00164849 -0.0155265   0.61151403  0.81330906]
    Intercepto: 0.5015476024153426
    
En este caso, un MSE de 0.7877 sugiere que, en promedio, las predicciones del modelo tienen un error de 0.7877 unidades respecto a los valores observados. Cuanto menor sea el MSE, mejor será el modelo.

Aunque no es un valor extremadamente alto, el R² de 0.3530 sugiere que el modelo tiene un poder explicativo moderado, pero hay un 65% de la variabilidad que el modelo no captura. Esto sugiere que el modelo podría no estar aprovechando completamente la relación entre las variables.

El intercepto (o constante) del modelo es 0.5015, lo que significa que, cuando todas las variables predictoras son cero, el valor predicho de recovery_status_encoded será de 0.5015. Este valor es esencialmente el punto de partida del modelo, antes de que se tenga en cuenta cualquier influencia de las variables predictoras.

Vemos también que category_fee (0,813) y status_fee (0,611) són las variables que más impacto tienen en el modelo. 

##### Correlación respecto recovery status:

![Salida de correlacion recovery status](https://github.com/arboldeku/businessPayments/blob/regresion/graficos_y_salidas/correlacion_recovery_status.PNG?raw=true)

Las variables que tienen correlaciones moderadas positivas con recovery_status_encoded incluyen principalmente tipos y categorías de tarifas y el estado de las tarifas (especialmente las rechazadas). Estas variables podrían tener una mayor influencia en los resultados de recuperación.

Las variables que tienen correlaciones negativas moderadas o débiles, como user_id, cash_request_id, y type_fee_instant_payment, pueden sugerir que ciertos tipos de transacciones o usuarios tienen una probabilidad más baja de tener un buen estado de recuperación.

#### Uso de regresión logística

Hemos utilizado una regresión logística multinomial para predecir la variable categórica status de un conjunto de datos. En este caso hemos implementado una función **sigmoide**. 

    def funcion_logistica(x, L=1, k=0.1, x0=0):
        return L / (1 + np.exp(-k * (x - x0)))

Donde:

    L es la amplitud de la curva.
    k es la pendiente de la curva, que controla cuán empinada es la transición entre 0 y 1.
    x0 es el punto de inflexión, donde la probabilidad es 0.5.
    
Hemos utilizado la función de pérdida log-loss dónde se mide el error entre las predicciones y las etiquetas verdaderas.

El modelo ha logrado una precisión del **89.53%** en el conjunto de prueba. Esto significa que aproximadamente el 89.5% de las predicciones realizadas por el modelo fueron correctas.

Este es un resultado bastante bueno para un modelo de regresión logística, indicando que el modelo tiene un buen rendimiento en la clasificación de las transacciones en sus diferentes estados (status).

El modelo de regresión logística multinomial ha sido capaz de predecir de manera eficiente los diferentes estados de las transacciones, alcanzando una alta precisión en la clasificación. Este resultado es prometedor y podría ser útil para predecir el estado de futuras transacciones en la plataforma.

