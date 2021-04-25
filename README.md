### Escuela Colombiana de Ingeniería
### Arquitecturas de Software - ARSW

## Escalamiento en Azure con Maquinas Virtuales, Sacale Sets y Service Plans

### Dependencias
* Cree una cuenta gratuita dentro de Azure. Para hacerlo puede guiarse de esta [documentación](https://azure.microsoft.com/en-us/free/search/?&ef_id=Cj0KCQiA2ITuBRDkARIsAMK9Q7MuvuTqIfK15LWfaM7bLL_QsBbC5XhJJezUbcfx-qAnfPjH568chTMaAkAsEALw_wcB:G:s&OCID=AID2000068_SEM_alOkB9ZE&MarinID=alOkB9ZE_368060503322_%2Bazure_b_c__79187603991_kwd-23159435208&lnkd=Google_Azure_Brand&dclid=CjgKEAiA2ITuBRDchty8lqPlzS4SJAC3x4k1mAxU7XNhWdOSESfffUnMNjLWcAIuikQnj3C4U8xRG_D_BwE). Al hacerlo usted contará con $200 USD para gastar durante 1 mes.

### Parte 0 - Entendiendo el escenario de calidad

Adjunto a este laboratorio usted podrá encontrar una aplicación totalmente desarrollada que tiene como objetivo calcular el enésimo valor de la secuencia de Fibonnaci.

**Escalabilidad**
Cuando un conjunto de usuarios consulta un enésimo número (superior a 1000000) de la secuencia de Fibonacci de forma concurrente y el sistema se encuentra bajo condiciones normales de operación, todas las peticiones deben ser respondidas y el consumo de CPU del sistema no puede superar el 70%.

### Escalabilidad Serverless (Functions)

1. Cree una Function App tal cual como se muestra en las  imagenes.

![](images/part3/part3-function-config.png)

![](images/part3/part3-function-configii.png)

2. Instale la extensión de **Azure Functions** para Visual Studio Code.

![](images/part3/part3-install-extension.png)

3. Despliegue la Function de Fibonacci a Azure usando Visual Studio Code. La primera vez que lo haga se le va a pedir autenticarse, siga las instrucciones.

![](images/part3/part3-deploy-function-1.png)

![](images/part3/part3-deploy-function-2.png)

4. Dirijase al portal de Azure y pruebe la function.

![](images/part3/part3-test-function.png)

5. Modifique la coleción de POSTMAN con NEWMAN de tal forma que pueda enviar 10 peticiones concurrentes. Verifique los resultados y presente un informe.
# Informe
Se creó un environment donde se guardó la dirección de Azure, y por otro lado Se modificó la colección de postman para poder enviar peticiones concurrentemente.
Este fue el resultado de las 10 peticiones:
![Imagenes](https://github.com/JoseGutierrezMairn/ARSW-Lab11/blob/master/images/punto5.jpg?raw=true)  
Además cuando se revisó el monitoreo en la plataforma de azure se pudo ver el siguiente resultado:  
![Imagenes](https://github.com/JoseGutierrezMairn/ARSW-Lab11/blob/master/images/monitoring5.jpg?raw=true)  
Que como se ve en la imagen, las pruebas tuvieron un resultado exitoso. 
tener en cuenta que para ejecutar la coleccion de postman se usa el comando
~~~
newman run Pos.postman_collection.json -n 10
~~~
6. Cree una nueva Function que resuleva el problema de Fibonacci pero esta vez utilice un enfoque recursivo con memoization. Pruebe la función varias veces, después no haga nada por al menos 5 minutos. Pruebe la función de nuevo con los valores anteriores. ¿Cuál es el comportamiento?.

**Preguntas**

* ¿Qué es un Azure Function?
    *  es un servicio serverless, una solución que nos permite ejecutar fácilmente pequeños fragmentos de código o funciones en la nube, esto hace que solamente nos tengamos que preocupar por desarrollar la funcionalidad que necesitamos, sin importar la aplicación o la infraestructura para ejecutarlo. Permite desarrollar con más eficacia una plataforma informática sin servidor basada en eventos, permite compilar y depurar a nivel local sin configuracines edicionales.
* ¿Qué es serverless?
    * es un tipo de arquitectura en el que no se utilizan servidores (físicos o en la nube), sino que se asigna la responsabilidad de ejecutar un fragmento de código a un proovedor de la nube (AWS, Azure, Google Cloud, etc.), este se encarga de realizar una asignación dinámica de recursos, es decir, los escala automáticamente si crece la demanda y los libera cuando no son utilizados.
* ¿Qué es el runtime y que implica seleccionarlo al momento de crear el Function App?
    * Se relaciona principalmente con la version de .NET en la que se basa su tiempo de ejecución. Al seleccionar el plan Consumption y la versión de runtime 2, implica que  su "timeout duration" es de 5 minutos.
* ¿Por qué es necesario crear un Storage Account de la mano de un Function App?
    * Debido a que Azure Functions se basa en Azure Storage para operaciones de almacenamiento y administración como son Manejo de triggers y logs. Azure Storage account nos proporciona un espacio de nombres unico para el almacenamiento.
* ¿Cuáles son los tipos de planes para un Function App?, ¿En qué se diferencias?, mencione ventajas y desventajas de cada uno de ellos.  
    * Existen 3 tipos de planes: Consumption, Premium y App Service. Sus características, ventajas y desventajas de pueden ver en la siguiente imagen:  
	![Imagenes](https://github.com/JoseGutierrezMairn/ARSW-Lab11/blob/master/images/planes.jpg?raw=true)  
* ¿Por qué la memoization falla o no funciona de forma correcta?  
    * Después de los 5 minutos, functionTimeout llega al límite de espera y la siguiente petición que se haga es como si fuera la primera, es decir, la estructura de memorización usada anteriormente queda vacía.
* ¿Cómo funciona el sistema de facturación de las Function App?  
    * el consumo de los recursos y las ejecuciones por segundo, donde los precios del plan de consumo incluyen 1 millones de solicitudes y 400.000 GB-segundos de consumo de recursos
	   gratuitos por mes. Functions hace la facturación según el consumo de recursos medido en GB-s, donde el consumo de recursos se calcula multiplicando el tamaño medio de memoria
	   en GB por el tiempo en milisegundos que dura la ejecución de la función. Y la memoria que una función utiliza se mide redondeando a los 128 MB más cercanos hasta un tamaño de
	   memoria máximo de 1.536 MB, y el tiempo de ejecución se redondea a los 1 ms más cercanos y para la ejecución de una única función, el tiempo de ejecución mínimo es de 100 ms
	   y la memoria mínima es de 128 MB, respectivamente.
* Informe
