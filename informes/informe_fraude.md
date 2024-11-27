### Informe de usuarios potencialmente fraudulentos

Las plataformas de pago y las fintech enfrentan de manera constante el desafío de detectar y prevenir fraudes transaccionales, un problema que no solo pone en riesgo la seguridad financiera de los usuarios, sino que también puede comprometer la integridad y el cumplimiento normativo de la plataforma. A pesar de que las transacciones fraudulentas representan generalmente menos del 0.3% del total de las transacciones, su impacto potencial es significativo, tanto en términos financieros como en la confianza del cliente.

En este análisis, se han identificado una serie de características clave que permiten detectar patrones sospechosos asociados a actividades fraudulentas en las transacciones. Estas características incluyen transacciones rechazadas o declinadas, montos inusuales o comportamientos atípicos de usuarios inactivos, así como patrones temporales inusuales y secuencias sospechosas de transacciones. Al enfocarse en estos factores, es posible identificar transacciones potencialmente fraudulentas y actuar de manera proactiva para mitigar riesgos.

**Métricas para detectar usuarios fraudulentos**:

##### 1: Filtrar los usuarios con status "transaction_declined" o "rejected"

Estas transacciones deben ser aisladas ya que pueden representar intentos de fraude, especialmente cuando ocurren en grandes cantidades o con comportamientos inusuales asociados a ellas. Al enfocarnos en estos dos estatus, podemos comenzar a identificar transacciones que merecen un análisis más detallado, ayudando a detectar patrones potenciales de fraude en etapas tempranas del proceso.

##### 2: Identificación de usuarios con transacciones de monto máximo (200)

El segundo paso en la detección de posibles fraudes es analizar los montos de las transacciones realizadas. Sabemos que, en esta plataforma, el monto máximo permitido para una transacción es de 200. Por lo tanto, cualquier usuario que haya realizado una transacción exactamente por este valor debe ser considerado un caso sospechoso.

El filtrado de usuarios con transacciones de 200 como monto máximo permitirá identificar de manera temprana posibles comportamientos fraudulentos relacionados con el uso de grandes sumas en una sola operación sin justificación aparente.

##### 3: Calcular el diferencial de días entre created_at y updated_at

En situaciones normales, la diferencia entre estos dos timestamps debería ser relativamente pequeña, ya que una vez que una transacción es creada, las actualizaciones (si ocurren) suelen ser rápidas y dentro de un plazo razonable. Sin embargo, cuando esta diferencia es muy pequeña (o incluso nula), podría indicar lo siguiente:

   a. Manipulación rápida de la transacción: Un tiempo de diferencia extremadamente corto entre la creación y la actualización puede ser una señal de manipulación por parte de un usuario o de un intento de evadir los controles del sistema. Por ejemplo, si una transacción es creada y luego actualizada casi instantáneamente, podría estar relacionado con un intento de fraudulento para cambiar detalles de la transacción o intentar realizar una operación de forma errónea para pasar desapercibida.

   b. Transacciones fraudulentas: En algunos casos, los atacantes pueden intentar realizar transacciones fraudulentas que sean rápidamente corregidas o modificadas para evitar la detección. Una diferencia temporal muy corta puede ser un indicativo de tales manipulaciones.
   
 ##### 4: Filtrar los user_id con transacciones realizadas entre la 1 AM y las 4 AM
 
Las transacciones a altas horas de la madrugada, especialmente cuando no hay una justificación clara, pueden indicar un comportamiento fraudulento, ya que los delincuentes a menudo operan durante horarios en los que las plataformas tienen menor supervisión.

##### 5: Filtrar los user_id con status "direct_debit_rejected"

En este paso, el objetivo es identificar los user_id asociados a transacciones que han sido rechazadas o declinadas debido a diferentes razones. Estas transacciones son señales tempranas de que algo no está funcionando correctamente en el proceso de pago y podrían ser indicativos de intentos de fraude.

Este estatus se refiere a las transacciones de débito directo que han sido rechazadas, lo cual puede suceder por razones similares (por ejemplo, falta de fondos o inconsistencias en los datos de la cuenta bancaria).

**Usuarios detectados**:

Una vez hemos aplicado todos estos filtros para todas las transacciones, nos va a devolver los user_id que coinciden con cada filtro y el conteo de cuántas veces aparecen en los distintos filtros. Nos encontramos que el número máximo de filtros al que aparecen un grupo de usuarios es **3/5**, esto nos indica que serían los candidatos a ser usuarios potencialmente fraudulentos. Ahora que los tenemos identificados, los capturamos añadiendo una columna sólo de estos user_id en el datframe, para a partir de ahí determinar si estamos hablando de verdaderos usuarios fraudulentos o simplemente se trata de una coincidencia. 

#### Uso de Random Forest para predecir verdaderos positivos y falsos positivos:
![salida random forest](https://github.com/arboldeku/businessPayments/blob/regresion/graficos_y_salidas/randomforest_output.PNG?raw=true)

El análisis de los resultados de la matriz de confusión y del reporte de clasificación generado por el modelo de Random Forest para predecir el fraude transaccional muestra cómo el modelo ha clasificado a los usuarios potencialmente peligrosos, y cómo se desempeña en términos de precisión, recall, f1-score y accuracy.

##### 1. Matriz de Confusión

La matriz de confusión muestra cómo el modelo clasificó las transacciones, dividiendo las predicciones en las siguientes categorías:

    Predicción: No fraude (0)	Predicción: Fraude (1)

    Real: No fraude (0)	4181 (verdaderos negativos)	5 (falsos positivos)
    Real: Fraude (1)	17 (falsos negativos)	9 (verdaderos positivos)
    
Verdaderos Negativos (VN): 4181 transacciones que eran no fraudulentas y fueron correctamente identificadas como no fraudulentas.
Falsos Positivos (FP): 5 transacciones que no eran fraudulentas pero fueron clasificadas erróneamente como fraudulentas.
Falsos Negativos (FN): 17 transacciones fraudulentas que fueron clasificadas erróneamente como no fraudulentas.
Verdaderos Positivos (VP): 9 transacciones fraudulentas que fueron correctamente identificadas como fraudulentas.

##### 2. Precisión:

Para el fraude (1): 0.64. Esto significa que, de todas las transacciones que el modelo identificó como fraudulentas, el 64% fueron realmente fraudulentas.

Para el no fraude (0): 1.00. Esto indica que, de todas las transacciones que el modelo clasificó como no fraudulentas, el 100% eran realmente no fraudulentas.

##### 3. Recall:

Para el fraude (1): 0.35. Este valor indica que el modelo identificó correctamente solo el 35% de las transacciones realmente fraudulentas.
Esto es relativamente bajo y sugiere que el modelo no está capturando todos los fraudes.

Para el no fraude (0): 1.00. Esto muestra que el modelo detectó correctamente el 100% de las transacciones no fraudulentas.

##### 4. F1-Score:

Para el fraude (1): 0.45. El F1-score es una medida balanceada de la precisión y el recall. Un valor de 0.45 sugiere que el modelo tiene un desempeño moderado en cuanto a la detección de fraudes, ya que no está capturando una gran parte de los fraudes reales, aunque cuando predice fraude, suele ser correcto.

Para el no fraude (0): 1.00. La excelente puntuación en esta categoría muestra que el modelo tiene un rendimiento casi perfecto al identificar transacciones no fraudulentas.

**Conclusión usando Random Forest**:

Bajo desempeño en la detección de fraudes: A pesar de una precisión general alta (99%), el modelo tiene un bajo recall para fraudes (0.35). Esto significa que solo una fracción de las transacciones fraudulentas es identificada correctamente, lo que indica que el modelo necesita mejorar en cuanto a la detección de fraudes.

Desbalance de clases: El modelo está muy sesgado hacia la clase mayoritaria (no fraude), lo que explica su alta precisión general. Para abordar esto, podrían explorarse técnicas como submuestreo, sobremuestreo, o ajustes en el umbral de decisión para mejorar la detección de fraudes sin afectar tanto la precisión.

