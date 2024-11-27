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

