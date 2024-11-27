### Informe del Análisis de Calidad de Datos

Resultados del análisis de calidad de los datos, identificando problemas encontrados (como valores faltantes o inconsistencias) y detallando las soluciones implementadas para garantizar la confiabilidad del análisis posterior.

A continuación se presenta el análisis de cada una de las bases de datos, enfocándose en los valores faltantes. Como se puede observar, no hay registros duplicados, pero sí hay datos faltantes en ambas bases de datos.

**Basic Information extract - cash request - data analyst.csv**:

El DataFrame tiene 23970 entradas, con índices que van de 0 a 23969. Contiene un total de 16 columnas con los siguientes tipos de datos:

- 3 columnas de tipo `float64`
- 1 columna de tipo `int64`
- 12 columnas de tipo `object`

A continuación se muestra el número de valores no nulos por columna:

- `id`: 23970 no nulos
- `amount`: 23970 no nulos
- `status`: 23970 no nulos
- `created_at`: 23970 no nulos
- `updated_at`: 23970 no nulos
- `user_id`: 21867 no nulos
- `moderated_at`: 16035 no nulos
- `deleted_account_id`: 2104 no nulos
- `reimbursement_date`: 23970 no nulos
- `cash_request_received_date`: 16289 no nulos
- `money_back_date`: 16543 no nulos
- `transfer_type`: 23970 no nulos
- `send_at`: 16641 no nulos
- `recovery_status`: 3330 no nulos
- `reco_creation`: 3330 no nulos
- `reco_last_update`: 3330 no nulos

El uso de memoria del DataFrame es de aproximadamente 2.9 MB.

**Evaluación de la calidad de los datos para extract - cash request - data analyst.csv**

Valores faltantes en extract - cash request - data analyst.csv:
- `id`: 0
- `amount`: 0
- `status`: 0
- `created_at`: 0
- `updated_at`: 0
- `user_id`: 2103
- `moderated_at`: 7935
- `deleted_account_id`: 21866
- `reimbursement_date`: 0
- `cash_request_received_date`: 7681
- `money_back_date`: 7427
- `transfer_type`: 0
- `send_at`: 7329
- `recovery_status`: 20640
- `reco_creation`: 20640
- `reco_last_update`: 20640

Filas duplicadas en extract - cash request - data analyst.csv:
- 0

Graficamente los valores faltantes en extract - cash request - data analyst se puden observar de la siguiente manera: 
**Basic Information extract - fees - data analyst - .csv**:

El DataFrame tiene 21061 entradas, con índices que van de 0 a 21060. Contiene un total de 13 columnas con los siguientes tipos de datos:

- 2 columnas de tipo `float64`
- 1 columna de tipo `int64`
- 10 columnas de tipo `object`

A continuación se muestra el número de valores no nulos por columna:

- `id`: 21061 no nulos
- `cash_request_id`: 21057 no nulos
- `type`: 21061 no nulos
- `status`: 21061 no nulos
- `category`: 2196 no nulos
- `total_amount`: 21061 no nulos
- `reason`: 21061 no nulos
- `created_at`: 21061 no nulos
- `updated_at`: 21061 no nulos
- `paid_at`: 15531 no nulos
- `from_date`: 7766 no nulos
- `to_date`: 7766 no nulos
- `charge_moment`: 21061 no nulos

El uso de memoria del DataFrame es de aproximadamente 2.1 MB.

**Evaluación de la calidad de los datos para extract - fees - data analyst - .csv**

Valores faltantes en extract - fees - data analyst - .csv:
- `id`: 0
- `cash_request_id`: 4
- `type`: 0
- `status`: 0
- `category`: 18865
- `total_amount`: 0
- `reason`: 0
- `created_at`: 0
- `updated_at`: 0
- `paid_at`: 5530
- `from_date`: 13295
- `to_date`: 13295
- `charge_moment`: 0

Filas duplicadas en extract - fees - data analyst - .csv:
- 0

Se comprobó que la columna `id` de la primera base de datos corresponde a la columna `cash_request_id` de la segunda base de datos. En este sentido, se decidió fusionar las dos bases de datos considerando estos campos como equivalentes. 2913 registros de cash request no tienen un valor fee asociado. Dado que los datos no asociados no son de interés para el análisis de las métricas, se decidió tomar en cuenta solo los registros de cash request que tienen un registro de fee asociado. Por otro lado, dado que las columnas `id`, `created_at` y `updated_at` tienen el mismo nombre en ambas bases de datos, se decidió añadir el sufijo `_fee` a todas las columnas de la segunda base de datos, excepto `cash_request_id`.

**Resultado de agregar '_fee' a cada columna de extract - fees - data analyst - .csv**:

El DataFrame tiene 21061 entradas, con índices que van de 0 a 21060. Contiene un total de 13 columnas con los siguientes tipos de datos:

- 2 columnas de tipo `float64`
- 1 columna de tipo `int64`
- 10 columnas de tipo `object`

A continuación se muestra el número de valores no nulos por columna:

- `id_fee`: 21061 no nulos
- `cash_request_id`: 21057 no nulos
- `type_fee`: 21061 no nulos
- `status_fee`: 21061 no nulos
- `category_fee`: 2196 no nulos
- `total_amount_fee`: 21061 no nulos
- `reason_fee`: 21061 no nulos
- `created_at_fee`: 21061 no nulos
- `updated_at_fee`: 21061 no nulos
- `paid_at_fee`: 15531 no nulos
- `from_date_fee`: 7766 no nulos
- `to_date_fee`: 7766 no nulos
- `charge_moment_fee`: 21061 no nulos

El uso de memoria del DataFrame es de aproximadamente 2.1 MB.

Posteriormente, se procedió a fusionar las bases de datos. A continuación, se presenta el análisis de la base de datos fusionada, enfocándose en los valores faltantes.

**Conjuntos de datos fusionados basados en las columnas 'id' (extract - cash request - data analyst) y 'cash_request_id' (extract - fees - data analyst)**:

El DataFrame fusionado tiene 21057 entradas, con índices que van de 0 a 21056. Contiene un total de 29 columnas con los siguientes tipos de datos:

- 5 columnas de tipo `float64`
- 2 columnas de tipo `int64`
- 22 columnas de tipo `object`

A continuación se muestra el número de valores no nulos por columna:

- `id`: 21057 no nulos
- `amount`: 21057 no nulos
- `status`: 21057 no nulos
- `created_at`: 21057 no nulos
- `updated_at`: 21057 no nulos
- `user_id`: 20151 no nulos
- `moderated_at`: 11284 no nulos
- `deleted_account_id`: 906 no nulos
- `reimbursement_date`: 21057 no nulos
- `cash_request_received_date`: 19763 no nulos
- `money_back_date`: 19701 no nulos
- `transfer_type`: 21057 no nulos
- `send_at`: 17570 no nulos
- `recovery_status`: 6894 no nulos
- `reco_creation`: 6894 no nulos
- `reco_last_update`: 6894 no nulos
- `id_fee`: 21057 no nulos
- `cash_request_id`: 21057 no nulos
- `type_fee`: 21057 no nulos
- `status_fee`: 21057 no nulos
- `category_fee`: 2196 no nulos
- `total_amount_fee`: 21057 no nulos
- `reason_fee`: 21057 no nulos
- `created_at_fee`: 21057 no nulos
- `updated_at_fee`: 21057 no nulos
- `paid_at_fee`: 15531 no nulos
- `from_date_fee`: 7766 no nulos
- `to_date_fee`: 7766 no nulos
- `charge_moment_fee`: 21057 no nulos

El uso de memoria del DataFrame es de aproximadamente 4.7 MB.

**Evaluación de la calidad de los datos para merged_cash_request_fees.csv**

Valores faltantes en merged_cash_request_fees.csv:
- `id`: 0
- `amount`: 0
- `status`: 0
- `created_at`: 0
- `updated_at`: 0
- `user_id`: 906
- `moderated_at`: 9773
- `deleted_account_id`: 20151
- `reimbursement_date`: 0
- `cash_request_received_date`: 1294
- `money_back_date`: 1356
- `transfer_type`: 0
- `send_at`: 3487
- `recovery_status`: 14163
- `reco_creation`: 14163
- `reco_last_update`: 14163
- `id_fee`: 0
- `cash_request_id`: 0
- `type_fee`: 0
- `status_fee`: 0
- `category_fee`: 18861
- `total_amount_fee`: 0
- `reason_fee`: 0
- `created_at_fee`: 0
- `updated_at_fee`: 0
- `paid_at_fee`: 5526
- `from_date_fee`: 13291
- `to_date_fee`: 13291
- `charge_moment_fee`: 0

Filas duplicadas en merged_cash_request_fees.csv:
- 0

Seguidamente, se procedió a documentar el proceso para completar los valores faltantes. A continuación, se presentan los códigos que se usaron para completar los valores faltantes en las columnas mencionadas, seguido de una explicación. La explicación se tomó de Lexique - Data Analyst.xlsx. En los casos en que aparece un signo de interrogación en la explicación, es porque no se ha encontrado una justificación para los datos faltantes; consecuentemente, se completaron los datos con "Null". Dado que esos campos no son de utilidad en los futuros análisis, no fue necesario implementar otra estrategia.

**Códigos para información faltante basados en Lexique - Data Analyst.xlsx**:

| Nombre de la Columna           | Código   | Explicación                                      |
|--------------------------------|----------|--------------------------------------------------|
| user_id                        | Null     | ?                                                |
| moderated_at                   | 98       | No necesita una revisión manual                  |
| deleted_account_id             | 98888888 | No hay cuenta eliminada                          |
| cash_request_received_date     | 98       | No se envió un débito directo SEPA               |
| money_back_date                | Null     | ?                                                |
| send_at                        | Null     | ?                                                |
| recovery_status                | 98       | La solicitud de efectivo nunca tuvo un incidente de pago |
| reco_creation                  | Null     | ?                                                |
| reco_last_update               | Null     | ?                                                |
| category_fee                   | Null     | ?                                                |
| paid_at_fee                    | Null     | ?                                                |
| from_date_fee                  | 98       | No hay tarifas pospuestas                        |
| to_date_fee                    | 98       | No hay tarifas pospuestas                        |

**Evaluación de la calidad de los datos para cleaned_merged_cash_request_fees.csv**

Valores faltantes en cleaned_merged_cash_request_fees.csv:
- `id`: 0
- `amount`: 0
- `status`: 0
- `created_at`: 0
- `updated_at`: 0
- `user_id`: 0
- `moderated_at`: 0
- `deleted_account_id`: 0
- `reimbursement_date`: 0
- `cash_request_received_date`: 0
- `money_back_date`: 0
- `transfer_type`: 0
- `send_at`: 0
- `recovery_status`: 0
- `reco_creation`: 0
- `reco_last_update`: 0
- `id_fee`: 0
- `cash_request_id`: 0
- `type_fee`: 0
- `status_fee`: 0
- `category_fee`: 0
- `total_amount_fee`: 0
- `reason_fee`: 0
- `created_at_fee`: 0
- `updated_at_fee`: 0
- `paid_at_fee`: 0
- `from_date_fee`: 0
- `to_date_fee`: 0
- `charge_moment_fee`: 0

Filas duplicadas en cleaned_merged_cash_request_fees.csv:
- 0

Consecuentemente, fue necesario convertir el tipo de datos de las columnas de interés a tipos que faciliten el manejo de los datos en las métricas. Las columnas `created_at` y `created_at_fee` se cambiaron de tipo de datos de objeto a tipo de fecha.

**Conversión de las columnas 'created_at' and 'created_at_fee' a formato de fecha**:

El DataFrame tiene 21057 entradas, con índices que van de 0 a 21056. Contiene un total de 29 columnas con los siguientes tipos de datos:

- 4 columnas de tipo `float64`
- 2 columnas de tipo `int64`
- 21 columnas de tipo `object`
- 2 columnas de tipo `datetime64[ns, UTC]`

A continuación se muestra el número de valores no nulos por columna:

- `id`: 21057 no nulos
- `amount`: 21057 no nulos
- `status`: 21057 no nulos
- `created_at`: 21057 no nulos
- `updated_at`: 21057 no nulos
- `user_id`: 21057 no nulos
- `moderated_at`: 21057 no nulos
- `deleted_account_id`: 21057 no nulos
- `reimbursement_date`: 21057 no nulos
- `cash_request_received_date`: 21057 no nulos
- `money_back_date`: 21057 no nulos
- `transfer_type`: 21057 no nulos
- `send_at`: 21057 no nulos
- `recovery_status`: 21057 no nulos
- `reco_creation`: 21057 no nulos
- `reco_last_update`: 21057 no nulos
- `id_fee`: 21057 no nulos
- `cash_request_id`: 21057 no nulos
- `type_fee`: 21057 no nulos
- `status_fee`: 21057 no nulos
- `category_fee`: 21057 no nulos
- `total_amount_fee`: 21057 no nulos
- `reason_fee`: 21057 no nulos
- `created_at_fee`: 21057 no nulos
- `updated_at_fee`: 21057 no nulos
- `paid_at_fee`: 21057 no nulos
- `from_date_fee`: 21057 no nulos
- `to_date_fee`: 21057 no nulos
- `charge_moment_fee`: 21057 no nulos

El uso de memoria del DataFrame es de aproximadamente 4.7 MB.

Por último, a continuación se presentan las posibles columnas útiles para los objetivos del proyecto, las cuales se verificó que no tienen valores faltantes o datos que podrían representar un riesgo para las métricas.

**Calidad de los datos necesarios para las métricas**:
1. **Frecuencia de Uso del Servicio:** Analizar con qué frecuencia los usuarios de cada cohorte utilizan los servicios de adelanto de efectivo de Business Payments a lo largo del tiempo. (“created_at” “transfer_type”)

Evaluación de la calidad de los datos para la columna: `created_at`
- Valores faltantes en `created_at`: 0
- Fechas inválidas en `created_at`: 0

Evaluación de la calidad de los datos para la columna: `transfer_type`
- Valores faltantes en `transfer_type`: 0
- Valores únicos en `transfer_type`: ['instant', 'regular']

2. **Tasa de Incidentes:** Determinar la tasa de incidentes, especialmente aquellos relacionados con problemas de pago, en cada cohorte. Identificar variaciones significativas entre cohortes. (“recovery_status: pending, pending_direct_debit”)

Evaluación de la calidad de los datos para la columna: `recovery_status`
- Valores faltantes en `recovery_status`: 0
- Conteo de 'pending' en `recovery_status`: 1914
- Conteo de 'pending_direct_debit' en `recovery_status`: 34
- Valores únicos en `recovery_status`: [98, 'pending', 'completed', 'pending_direct_debit', 'cancelled']

3. **Ingresos Generados por Cohorte:** Calcular el total de ingresos generados por cada cohorte a lo largo del tiempo para evaluar el impacto financiero del comportamiento de los usuarios. (“total_amount” “created_at” “created_at_fee”)

Evaluación de la calidad de los datos para la columna: `total_amount_fee`
- Valores faltantes en `total_amount_fee`: 0
- Valores negativos en `total_amount_fee`: 0

Evaluación de la calidad de los datos para la columna: `created_at`
- Valores faltantes en `created_at`: 0
- Fechas inválidas en `created_at`: 0

Evaluación de la calidad de los datos para la columna: `created_at_fee`
- Valores faltantes en `created_at_fee`: 0
- Fechas inválidas en `created_at_fee`: 0

4. **Métricas Acumuladas por Cohorte:** Proponer y calcular métricas acumuladas que proporcionen perspectivas adicionales para la extracción de insights accionables.

To be defined
