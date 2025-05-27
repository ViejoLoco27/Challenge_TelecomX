Descripción de los datos TelecomX_Data:
Filas: 7,267
Columnas: 21

|index|customerID|Churn|gender|SeniorCitizen|Partner|Dependents|tenure|PhoneService|MultipleLines|InternetService|OnlineSecurity|OnlineBackup|DeviceProtection|TechSupport|StreamingTV|StreamingMovies|Contract|PaperlessBilling|PaymentMethod|Charges\.Monthly|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|0002-ORFBO|No|Female|0|Yes|Yes|9|Yes|No|DSL|No|Yes|No|Yes|Yes|No|One year|Yes|Mailed check|65\.6|
|1|0003-MKNFE|No|Male|0|No|No|9|Yes|Yes|DSL|No|No|No|No|No|Yes|Month-to-month|No|Mailed check|59\.9|
|2|0004-TLHLJ|Yes|Male|0|No|No|4|Yes|No|Fiber optic|No|No|Yes|No|No|No|Month-to-month|Yes|Electronic check|73\.9|
 
Descripción de los datos TelecomX_servicios
Filas: 4832
Columnas: 12

|index|cancelacion|permanencia_mensual|servicio\_telefonico|multiples\_lineas|servicio\_internet|seguridad\_online|servicio\_nube|proteccion\_dispositivos|soporte\_tecnico|tv\_satelital|streaming\_peliculas|pago\_mensual|
|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|0|9|1|0|0|0|1|0|1|1|0|65\.6|
|1|0|9|1|1|0|0|0|0|0|0|1|59\.9|
|2|1|4|1|0|1|0|0|1|0|0|0|73\.9|

Estadísticos de Telecom_servicios
|index|pago\_mensual|
|---|---|
|count|4832\.0|
|mean|81\.7612065397351|
|std|18\.306133537175757|
|min|42\.9|
|25%|69\.7875|
|50%|82\.5|
|75%|95\.7|
|max|118\.75|

Última modificación a los datos

|index|cancelacion|permanencia\_mensual|servicio\_telefonico|multiples\_lineas|servicio\_internet|seguridad\_online|servicio\_nube|proteccion\_dispositivos|soporte\_tecnico|tv\_satelital|streaming\_peliculas|pago\_mensual|total\_servicios|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|0|9|1|0|0|0|1|0|1|1|0|65\.6|4|
|1|0|9|1|1|0|0|0|0|0|0|1|59\.9|3|
|2|1|4|1|0|1|0|0|1|0|0|0|73\.9|3|
|3|1|13|1|0|1|0|1|1|0|1|1|98\.0|6|
|4|1|3|1|0|1|0|0|0|1|1|0|83\.9|4|

Estratificación por consumo
Se tomó como referencia la suma de los valores de las columnas de todos los servicios de cada cliente. 

|Niveles de consumo: | condición|
|---|---|
|Bajo| 0= total_servicios >=3|
|Medio| 4=< total_servicios >=6|
|Alto| 7=< total_servicios >=9|

Resultados de la estratificación por nivel de consumo
|consumo\_cliente|total_clientes|
|---|---|
|medio|2399|
|bajo|1400|
|alto|1033|

![Image](https://github.com/user-attachments/assets/f31e8489-21c3-4f7b-b21f-c76d16403770)

Estratificación aplicada a la categoría permanencia

|consumo\_cliente|permanencia\_promedio|total\_meses\_permanencia|ponderacion\_permanencia|gasto\_mensual\_promedio|
|---|---|---|---|---|
|bajo|13\.77|19281|12\.07|62\.42|
|medio|33\.78|81038|50\.73|84\.2|
|alto|57\.51|59411|37\.19|102\.3|

Estratificación aplicada a la categoría cancelación
|consumo\_cliente|total_cancelaciones|porcentaje\_cancelacion|gasto\_mensual|
|---|---|---|---|
|bajo|604|38\.08|66\.02|
|medio|783|49\.37|88\.9|
|alto|199|12\.55|105\.33|
