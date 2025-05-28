# Informe Final


## 📋Introducción 


## Instrucciones del Challenge TelecomX
La empresa Telecom X enfrenta una alta tasa de cancelaciones y necesita comprender los factores que llevan a la pérdida de clientes.

Tu desafío será recopilar, procesar y analizar los datos, utilizando Python y sus principales bibliotecas para extraer información valiosa. A partir de tu análisis, el equipo de Data Science podrá avanzar en modelos predictivos y desarrollar estrategias para reducir la evasión.

## 🎯Normalización de datos

Al inicio del proyecto, la empresa entregó una base de datos con distintas descripciones de los perfiles de los usuarios. El archivo se encontraba en formato json, por lo que, al importarlo, nos encontramos con columnas anidadas, es decir, columnas que contenían estructuras de datos como listas y diccionarios. En efecto, antes de trabajar la limpieza y transformación de datos, se llevó a cabo la normalización de las columnas anidadas.

|index|customerID|Churn|customer|phone|internet|account|
|---|---|---|---|---|---|---|
|0|0002-ORFBO|No|\{'gender': 'Female', 'SeniorCitizen': 0, 'Partner': 'Yes', 'Dependents': 'Yes', 'tenure': 9\}|\{'PhoneService': 'Yes', 'MultipleLines': 'No'\}|\{'InternetService': 'DSL', 'OnlineSecurity': 'No', 'OnlineBackup': 'Yes', 'DeviceProtection': 'No', 'TechSupport': 'Yes', 'StreamingTV': 'Yes', 'StreamingMovies': 'No'\}|\{'Contract': 'One year', 'PaperlessBilling': 'Yes', 'PaymentMethod': 'Mailed check', 'Charges': \{'Monthly': 65\.6, 'Total': '593\.3'\}\}|
|1|0003-MKNFE|No|\{'gender': 'Male', 'SeniorCitizen': 0, 'Partner': 'No', 'Dependents': 'No', 'tenure': 9\}|\{'PhoneService': 'Yes', 'MultipleLines': 'Yes'\}|\{'InternetService': 'DSL', 'OnlineSecurity': 'No', 'OnlineBackup': 'No', 'DeviceProtection': 'No', 'TechSupport': 'No', 'StreamingTV': 'No', 'StreamingMovies': 'Yes'\}|\{'Contract': 'Month-to-month', 'PaperlessBilling': 'No', 'PaymentMethod': 'Mailed check', 'Charges': \{'Monthly': 59\.9, 'Total': '542\.4'\}\}|
|2|0004-TLHLJ|Yes|\{'gender': 'Male', 'SeniorCitizen': 0, 'Partner': 'No', 'Dependents': 'No', 'tenure': 4\}|\{'PhoneService': 'Yes', 'MultipleLines': 'No'\}|\{'InternetService': 'Fiber optic', 'OnlineSecurity': 'No', 'OnlineBackup': 'No', 'DeviceProtection': 'Yes', 'TechSupport': 'No', 'StreamingTV': 'No', 'StreamingMovies': 'No'\}|\{'Contract': 'Month-to-month', 'PaperlessBilling': 'Yes', 'PaymentMethod': 'Electronic check', 'Charges': \{'Monthly': 73\.9, 'Total': '280\.85'\}\}|

### 📅Tabla normalizada de datos
```
#Script para normalizar columna por columna
data_nomalized = pd.concat([pd.json_normalize(datos['customer']), pd.json_normalize(datos['phone']), pd.json_normalize(datos['internet']), pd.json_normalize(datos['account'])], axis=1)
```
**Descripción de los datos TelecomX_Data:**
**Filas: 7,267**
**Columnas: 21**
|index|customer\_id|churn|gender|senior\_citizen|partner|dependents|tenure|phone\_service|multiple\_lines|internet\_service|online\_security|online\_backup|device\_protection|tech\_support|streaming\_tv|streaming\_movies|contract|paperless\_billing|payment\_method|charges\_monthly|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|0002-ORFBO|No|Female|0|Yes|Yes|9|Yes|No|DSL|No|Yes|No|Yes|Yes|No|One year|Yes|Mailed check|65\.6|
|1|0003-MKNFE|No|Male|0|No|No|9|Yes|Yes|DSL|No|No|No|No|No|Yes|Month-to-month|No|Mailed check|59\.9|
|2|0004-TLHLJ|Yes|Male|0|No|No|4|Yes|No|Fiber optic|No|No|Yes|No|No|No|Month-to-month|Yes|Electronic check|73\.9|

## 🎯Limpieza y tratamiento de datos
Una vez normalizados los datos, se llevó a cabo la limpieza de datos, procedimiento que consistió en:
- ☑️**Modificar los encabezados de las columnas (primer cambio de encabezados)**
 ```
#Los encabezados compuestos por dos o más plabras deben separarse con un guión bajo.
datos_normalizados = datos_normalizados.rename(columns={'customerid':'customer_id', 'seniorcitizen':'senior_citizen',
                                                               'phoneservice':'phone_service', 'multiplelines':'multiple_lines',
                                                               'internetservice':'internet_service', 'onlinesecurity':'online_security',
                                                               'onlinebackup':'online_backup', 'deviceprotection':'device_protection',
                                                               'techsupport':'tech_support', 'streamingtv':'streaming_tv','streamingmovies':
                                                               'streaming_movies','paperlessbilling':'paperless_billing', 'paymentmethod':'payment_method',
                                                               'charges.monthly':'charges_monthly','charges.total':'charges_total'})
```
- ☑️**Mediante consultas se detectaron valores nulos; por lo que, se aplicó el siguiente script para retirar estos valores y categorías que no servirán para el análisis**
```
columnas_to_modified = ['online_security','online_backup','device_protection','tech_support','streaming_tv','streaming_movies'] # Nombrando las columnas con valores NaN

# Remplazando valores de filas que hace referencia a características nulas por valores NaN

datos_normalizados[columnas_to_modified] = datos_normalizados[columnas_to_modified].replace('no internet service', np.nan)

datos_normalizados['multiple_lines']= datos_normalizados['multiple_lines'].replace('no phone service',np.nan)

datos_normalizados['internet_service']= datos_normalizados['internet_service'].replace('no',np.nan)

datos_normalizados['churn']= datos_normalizados['churn'].replace('',np.nan)

datos_normalizados['charges_total']=datos_normalizados['charges_total'].replace(' ',np.nan)

# Retirando valores NaN

datos_normalizados= datos_normalizados.dropna() # Eliminar valores NaN
```
- ☑️**En la columna ['charges_total'] se cambió el tipo de dato a float64**
```
datos_normalizados['charges_total']=datos_normalizados['charges_total'].astype(np.float64) # Transformar 'charges_total' a float64
```
- ☑️**Se reinició el índice del DataFrame para que se ajuste con las modificaciones realizadas**
```
datos_normalizados.reset_index(drop=True)
```
- ☑️**Las columnas con valores 'Yes' 'No' se cambiaron por valores 0, 1** 
```
col_to_change =['churn','partner','dependents','phone_service','multiple_lines','online_backup','device_protection','tech_support','streaming_tv','streaming_movies','paperless_billing']

datos_normalizados[col_to_change] = datos_normalizados[col_to_change].replace({'yes':1,'no':0})
```
- ☑️**Los valores de la columna ['internet_service'] 'fiber optic' 'DSL' se cambiaron por valores 0, 1.** El fundamento de este cambio es que el servicio de fibra óptica es considerado como una mejora en el servicio, por lo que hay un mayor número de usuarios que instalan este servicio. 
```
datos_servicios['internet_service']=datos_servicios['internet_service'].replace({'fiber optic':1,'dsl':0})
```
- ☑️**Modificar los encabezados de las columnas (segundo cambio de encabezados)**
```
datos_servicios=datos_servicios.rename(columns={'internet_service':'fibra_optica','phone_service':'servicio_telefonico','multiple_lines':'multiples_lineas',

'internet_service':'servicio_internet','online_security':'seguridad_online','online_backup':'servicio_nube',

'device_protection':'proteccion_dispositivos','tech_support':'soporte_tecnico','streaming_tv':'tv_satelital',

'streaming_movies':'streaming_peliculas','charges_monthly':'pago_mensual'

})
```
- ☑️**Por último, se retiraron columnas que no servirían para el análisis exploratorio de los datos.** El criterio que se aplicó para la depuración de datos se fundamentó en el hecho de que en el DataFrame se sirve de datos que pueden servir para dos tipos distintos de análisis: (análisis por estatus de los usuarios y un análisis por el tipo de servicio que contrataron los usuarios). Para nuestro ejercicio se planteó la posibilidad de explorar ambos casos, pero por cuestiones de tiempo, se procedió por el segundo; por lo que, se retiraron las categorías que hagan referencia al estatus de los usuarios.
```
datos_servicios.drop(['customer_id','gender','senior_citizen','partner',

'dependents','contract','paperless_billing','payment_method','charges_total','cuentas_diarias'],axis=1,inplace=True)
```
- ☑️**El notebook Colab concluye con el respaldo.** Anteriormente, se señaló que, las cateogrías del DataFrame original pueden ser útiles para realizar dos tipos de análisis por lo que se guardaron dos archivos para recuperar ambos según se vaya a necesitar en el futuro.
 
 - **TelecomX_estatus_usuarios.json**
 - **TelecomX_servicios.json**
 
 Al término de la limpieza y transformación de datos, se obtuvo el siguiente DataFrame.

### 📅Tabla Telecom_servicios

**Descripción de los datos TelecomX_servicios**
- **Filas: 4,832** 
- **Columnas: 12**

|index|churn|tenure|servicio\_telefonico|multiples\_lineas|servicio\_internet|seguridad\_online|servicio\_nube|proteccion\_dispositivos|soporte\_tecnico|tv\_satelital|streaming\_peliculas|pago\_mensual|
|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|0|9|1|0|0|0|1|0|1|1|0|65\.6|
|1|0|9|1|1|0|0|0|0|0|0|1|59\.9|
|2|1|4|1|0|1|0|0|1|0|0|0|73\.9|

## 🎯Análisis exploratorio de datos
 
- **Renombrar columnas ['churn'], ['tenure']: ** al inicio del análisis, se encontraron dos columnas que aún tenían nombres no muy asequibles, por lo que se procedió a modificarlos. 
- 
- Se realizó 
### **📊1. Análisis descriptivo de las columnas ... **
|index|pago\_mensual|
|---|---|
|count|4832\.0|
|mean|81\.76|
|std|18\.30|
|min|42\.9|
|25%|69\.7875|
|50%|82\.5|
|75%|95\.7|
|max|118\.75|

### **📊2. Análisis gráfico** 
![Image](https://github.com/user-attachments/assets/ef68bc75-b5c3-4902-91eb-f16fe3f166b0)



Dist Subsc
![Image](https://github.com/user-attachments/assets/d5b84e6a-e214-4867-ad3b-74c32886d8f0)

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

Permanencia por consumo servicios
![Image](https://github.com/user-attachments/assets/7a4cee9e-ff5e-4741-a1ba-653648c7f45d)

Estratificación aplicada a la categoría permanencia

|consumo\_cliente|permanencia\_promedio|total\_meses\_permanencia|ponderacion\_permanencia|gasto\_mensual\_promedio|
|---|---|---|---|---|
|bajo|13\.77|19281|12\.07|62\.42|
|medio|33\.78|81038|50\.73|84\.2|
|alto|57\.51|59411|37\.19|102\.3|

Gasto mensual por consumo de servicios
![Image](https://github.com/user-attachments/assets/7eac6c50-30a7-417a-a5e4-d5c9261d7d57)

![Image](https://github.com/user-attachments/assets/7183e1ce-9e06-4632-848b-761582ba4c7b)




Estratificación aplicada a la categoría cancelación
|consumo\_cliente|total_cancelaciones|porcentaje\_cancelacion|gasto\_mensual|
|---|---|---|---|
|bajo|604|38\.08|66\.02|
|medio|783|49\.37|88\.9|
|alto|199|12\.55|105\.33|

Gasto mensual de los usuarios que cancelaron por tipo de consumo
![Image](https://github.com/user-attachments/assets/75c465d2-b3a2-47bf-ae03-aeb3cfe9d153)


![Image](https://github.com/user-attachments/assets/43de7983-00b8-4066-8435-a3a44673acac)

## 📋**Observaciones**

#### [**insertar tabla**]
#### [**Insertar código**]


