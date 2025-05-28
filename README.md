# Informe Final

![Static Badge](https://img.shields.io/badge/Oracle_Next_Education-blue?style=plastic&logo=python&logoColor=green&logoSize=auto&label=Alura%20Latam)
![Static Badge](https://img.shields.io/badge/Proyecto-blue?style=plastic&logo=pandas&logoColor=green&logoSize=auto&label=Pandas%20ETL)
![Static Badge](https://img.shields.io/badge/Certificado_del_curso-Alura?logo=pandas&color=%231e4181&link=https%3A%2F%2Fapp.aluracursos.com%2Fcertificate%2Fguz27-unameconomia%2Fpandas-transformacion-manipulacion-datos)

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
 
- ☑️**Renombrar columnas ['churn'], ['tenure']: ** al inicio del análisis, se encontraron dos columnas que aún tenían nombres no muy asequibles, por lo que se procedió a modificarlos. 
```
datos_origen_servicios = datos_origen_servicios.rename(columns={'churn':'cancelacion','tenure':'permanencia_mensual'})
```
- ☑️**Se agregó una nueva columna ['total_servicios'] la cual suma los servicios contratados por usuarios**.
### 📅Tabla Servicios para la exploración del análisis de datos
**Descripción de los datos Servicios**
- **Filas: 4,832** 
- **Columnas: 13**

|index|cancelacion|permanencia\_mensual|servicio\_telefonico|multiples\_lineas|servicio\_internet|seguridad\_online|servicio\_nube|proteccion\_dispositivos|soporte\_tecnico|tv\_satelital|streaming\_peliculas|pago\_mensual|total\_servicios|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|0|9|1|0|0|0|1|0|1|1|0|65\.6|4|
|1|0|9|1|1|0|0|0|0|0|0|1|59\.9|3|
|2|1|4|1|0|1|0|0|1|0|0|0|73\.9|3|

### 📊1. Análisis descriptivo de las columnas ['permanencia_mensual', 'pago_mensual', 'total_servicios']
- ☑️A partir del nuevo DataFrame se puede obtener información muy valiosa de las medidas de tendencia central, por ello, se aplicó el siguiente script:
```
cols=['permanencia_mensual','pago_mensual','total_servicios']
pd.DataFrame(datos_origen_servicios[cols].describe()).round(2)
```

|index|permanencia\_mensual|pago\_mensual|total\_servicios|
|---|---|---|---|
|count|4832\.0|4832\.0|4832\.0|
|mean|33\.06|81\.76|4\.79|
|std|24\.64|18\.31|1\.97|
|min|1\.0|42\.9|1\.0|
|25%|9\.0|69\.79|3\.0|
|50%|30\.0|82\.5|5\.0|
|75%|56\.0|95\.7|6\.0|
|max|72\.0|118\.75|9\.0|
|index|pago\_mensual|

- ☑️**Mean (media):** en términos del número de meses que llevan los usuarios con el servicio de telecomunicaciones provisto por TelecomX es de un total de 33 meses; respecto al pago mensual promedio es de 81.76 y en promedio los usuarios contratan 4 de los 9 servicios que provee la compañía.
- ☑️**SDT (Desviación estándar):** en los tres casos se observa que la dispersión de datos es muy diversa, por lo que su comportamiento no es cercano a la media. En efecto, basar un análisis en el valor promedio puede llevarnos a conclusiones erróneas.

### **📊2. Análisis gráfico** 

☑️El siguiente gráfico tiene por objetivo mostrar la popularidad de los servicios que adquieren los usuarios. Una observación adicional, es que, el servicio telefónico no debería ser un indicador que requiera especial atención ya que para poder contar con los demás servicios se requiere de la suscripción de al menos una línea telefónica.

En términos de popularidad, los servicios de internet, líneas múltiples, streaming de películas y tv satelital, son los cuatro principales. Esto podría dar lugar a múltiples estrategias que busquen dar a conocer los otros cuatro servicios restantes. Podrían darse paquetes armados con periodos de prueba. Periodos de prueba atractivos. 

![Image](https://github.com/user-attachments/assets/ef68bc75-b5c3-4902-91eb-f16fe3f166b0)

☑️La pregunta que abre el estudio es ¿por qué los suscriptores están abandonando sus subscripciones? Por ello, partimos de la pregunta ¿cuántos usuarios abandonaron su subscripción? 
De acuerdo con la información obtenida, se observa que 1586 de los 4832 subscriptores dieron por cancelada la subscripción.

![Image](https://github.com/user-attachments/assets/d5b84e6a-e214-4867-ad3b-74c32886d8f0)

☑️Un paso adicional que se dio durante el análisis de datos fue la estratificación de datos por el nivel de consumo. Esto se llevó a cabo para simplificar la lectura de los datos. En lo que resta del análisis se empleó este método para obtener información del DataFrame.
Estratificación por consumo
Se tomó como referencia la suma de los valores de las columnas de todos los servicios de cada cliente. 

|Niveles de consumo: | condición|
|---|---|
|Bajo| 0= total_servicios >=3|
|Medio| 4=< total_servicios >=6|
|Alto| 7=< total_servicios >=9|

```
estratos = [0,3,6,9]  # Estratos: <=3, 4-6, 7-9

etiquetas = ['bajo','medio','alto']  # Etiquetas

servicios['consumo_cliente'] = pd.cut(servicios['total_servicios'], bins=estratos, labels=etiquetas)#pd.cut -> asigna la categoría correspondiente a cada columna
```
En su mayoría, los usuarios han contrato de entre cuatro a seis servicios de la compañía. Por otro lado, son sólo 1033 usuarios que cuentan con más de 6 servicios.

![Image](https://github.com/user-attachments/assets/7a4cee9e-ff5e-4741-a1ba-653648c7f45d)

☑️**Estratificación aplicada a la categoría permanencia**

|consumo\_cliente|permanencia\_promedio|total\_meses\_permanencia|ponderacion\_permanencia|gasto\_mensual\_promedio|
|---|---|---|---|---|
|bajo|13\.77|19281|12\.07|62\.42|
|medio|33\.78|81038|50\.73|84\.2|
|alto|57\.51|59411|37\.19|102\.3|

El 50.7% de las personas que mantuvieron subscripción realiza un consumo medio de los servicios de la compañía. Además, el gráfico siguiente muestra que las personas que tienen un consumo elevado han decidido permanecer con sus servicios.
![Image](https://github.com/user-attachments/assets/7183e1ce-9e06-4632-848b-761582ba4c7b)
En términos del gasto que realizan los usuarios según su nivel de consumo, se observa que las personas con consumo elevado, es decir el 37.2% de las personas que permanecieron con su subscripción gastan en promedio gasta un 39% más que los usuarios de consumo bajo.

![Image](https://github.com/user-attachments/assets/7eac6c50-30a7-417a-a5e4-d5c9261d7d57)

☑️**Estratificación aplicada a la categoría cancelación**
|consumo\_cliente|total_cancelaciones|porcentaje\_cancelacion|gasto\_mensual|
|---|---|---|---|
|bajo|604|38\.08|66\.02|
|medio|783|49\.37|88\.9|
|alto|199|12\.55|105\.33|

Respecto al gasto mensual de las personas que cancelaron su subscripción no resulta tan distinto a los que decidieron continuar con los servicios provistos por la compañía.

![Image](https://github.com/user-attachments/assets/75c465d2-b3a2-47bf-ae03-aeb3cfe9d153)
El siguiente gráfico muestra que los usuarios que más abandonaron sus suscripciones, según su perfil de consumo, fueron aquellas que tienen contratados menos de siete servicios.
![Image](https://github.com/user-attachments/assets/43de7983-00b8-4066-8435-a3a44673acac)

