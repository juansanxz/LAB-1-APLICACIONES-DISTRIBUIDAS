# TALLER 1: APLICACIONES DISTRIBUIDAS (HTTP, SOCKETS, HTML, JS,MAVEN, GIT)

A continuación, se elaborará un laboratorio en el que se deberá implementar la arquitectura sugerida más adelante, para consumir una API externa a partir de lo que un cliente solicite. En este caso, el cliente consultará la informacíon de una película a partir de su título.

## Instalación y ejecución
### Prerequisitos

* Versión 1.8 de Java.
* Si no cuenta con tal versión para ejecutarla desde la consola, dirijase a un IDE como IntelliJ en donde puede descargar y usar una versión específica de java para el proyecto.

### Instalación
1. Clonar el repositorio en la carpeta que desee. Para ello use el siguiente comando:
```
git clone 
```



## Autor

* **Juan Felipe Sánchez Pérez**

## Enunciado  

Debe construir una aplicación para consultar la información de películas de cine.  La aplicación recibirá una frase de búsqueda del título, por ejemplo “Guardians of the galaxy”  y deberá mostrar el los datos de la película correspondiente. Para esto utilice el API gratuito de https://www.omdbapi.com/ (Puede crear obtener una llave gratuita para realizar consultas). Se le pide que su implementación sea eficiente en cuanto a recursos así que debe implementar un Caché que permita evitar hacer consultas repetidas al API externo.

La arquitectura debe tener las siguientes características:  

1. El cliente Web debe ser un cliente asíncrono que corra en el browser  y use Json como formato para los mensajes.
2. El servidor de servirá como un gateway para encapsular llamadas a otros servicios Web externos.
3. La aplicación debe ser multiusuario.
4. Todos los protocolos de comunicación serán sobre HTTP.
5. Los formatos de los mensajes de intercambio serán siempre JSON.
6. La interfaz gráfica del cliente debe ser los más limpia y agradableolo HTML y JS (Evite usar librerías complejas). Para invocar métodos REST desde el cliente usted puede utilizar la tecnología que desee.
7. Debe construir un cliente Java que permita probar las funciones del servidor fachada. El cliente utiliza simples conexiones http para conectarse a los servicios. Este cliente debe hacer pruebas de concurrencia en su servidor de backend. (Docente especificó que no se debe realizar este punto).
8. La fachada de servicios tendrá un caché que permitirá que llamados que ya se han realizado a las implementaciones concretas con parámetros específicos no se realicen nuevamente. Puede almacenar el llamado como un String con su respectiva respuesta, y comparar el string respectivo. Recuerde que el caché es una simple estructura de datos.
9. Se debe poder extender fácilmente, por ejemplo, es fácil agregar nuevas funcionalidades, o es fácil cambiar el proveedor de una funcionalidad.
10. Debe utilizar maven para gestionar el ciclo de vida, git y github para almacenar al código fuente y heroku como plataforma de producción.
11. En el backend debe utilizar solo Java. No puede utilizar frameworks como SPRING.

<img width="965" alt="image" src="https://github.com/juansanxz/TALLER-1-APLICACIONES-DISTRIBUIDAS/assets/123812331/dfd3fec2-73e3-4dd3-8bb4-5a2266748320">


## Versión  

Se usó java 8 en la solución del laboratorio.

## Solución  

Siguiendo la arquitectura especificada anteriormente, se procede a configurar el servidor fachada para que cumpla con las características esperadas.  
* En primer lugar, se utiliza el código proporcionado dentro del método `connectionWithExternalRestApi(String movieName)` para invocar al servicio externo que brindará la información de las películas. 
* Luego, se configura lo que se enviará a través del flujo de salida hacia el cliente, para que sea formato JSON como se indica en el enunciado, y se ajusta para que cuando este realice una petición GET, se haga al recurso específico, por ejemplo: ` GET /movie?t=dora`. 
* Además, al recibirse la solicitud, se redirige a la URL con el path asociado que contendrá una tabla con la respuesta. Por ejemplo, para el caso anteriormente mencionado, la redirección sería hacia `http://localhost:35000/movie?t=dora`, mostrando como resultado:  

![img.png](img/img.png)  


* Para mostrar el resultado de esta forma, se implementó el método `movieDataService(String resource)`, el cuál recibirá de parámetro el recurso solicitado. Por ejemplo, para el caso anterior sería `/movie?t=dora`. 
* Se apoya además del método `buildTable(String movieData)` para la construcción de la tabla.  
* Ahora, si se deseara extender la funcionalidad, para filtrar la búsqueda junto con otro parámetro, podría seguirse la lógica implementada para separar los elementos que hacen parte de la búsqueda.  
* Por otro lado, si se desea usar un proveedor distinto para alguna función, se le podría enviar de parámetro la url al método `connectionWithExternalRestApi(String movieName, String url)`, para que use la que necesite en un momento determinado.  


## Tests  

Para las pruebas unitarias, se buscó comprobar que se estuviera realizando la solicitud a la API externa para obtener la información de la película, y además se comprobó el funcionamiento del caché.
A continuación se observa que las pruebas se realizaron de forma exitosa.  
![img_1.png](img/img_1.png)  
La implementación de las pruebas se encuentra en el código.  







