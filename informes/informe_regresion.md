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
    
