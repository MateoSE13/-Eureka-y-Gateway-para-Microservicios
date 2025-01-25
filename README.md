## 1. Título
**Implementación de Eureka y Spring Cloud Gateway para Microservicios**

## 2. Tiempo de duración
**20 minutos**

## 3. Fundamentos
En arquitecturas de microservicios, es esencial gestionar eficientemente la comunicación y el descubrimiento de servicios.  
- **Eureka** actúa como un servidor de descubrimiento, permitiendo que los servicios se registren y descubran dinámicamente.  
- **Spring Cloud Gateway** funciona como un punto de entrada único, manejando el enrutamiento de solicitudes hacia los diferentes microservicios registrados en Eureka.  
- Esta combinación facilita la escalabilidad y mantenibilidad de aplicaciones distribuidas.  

## 4. Conocimientos previos
- Conceptos básicos de microservicios.
- Familiaridad con Spring Boot y Spring Cloud.
- Conocimientos básicos de Maven y configuración de archivos YAML.

## 5. Objetivos a alcanzar
- Configurar un servidor Eureka para el descubrimiento de servicios.
- Implementar Spring Cloud Gateway como punto de entrada para los microservicios.
- Registrar microservicios en Eureka y configurar rutas en el Gateway.
- Verificar el correcto enrutamiento y comunicación entre los componentes.

## 6. Equipo necesario
- Computadora con **Java 17** o superior instalado.
- IDE compatible con proyectos Spring Boot (por ejemplo, **IntelliJ IDEA, Eclipse**).
- Conexión a internet para descargar dependencias y herramientas necesarias.

## 7. Material de apoyo
- [Documentación oficial de Spring Cloud Netflix Eureka](https://cloud.spring.io/spring-cloud-netflix/)
- [Documentación oficial de Spring Cloud Gateway](https://spring.io/projects/spring-cloud-gateway)
- [Guía de inicio rápido de Spring Boot](https://spring.io/quickstart)

## 8. Procedimiento

### **Paso 1: Crear el servidor Eureka**
1.1 Utilizar **Spring Initializr** para generar un nuevo proyecto Spring Boot con las siguientes dependencias:
   - Eureka Server
  ![image](https://github.com/user-attachments/assets/d1632d31-52df-45b8-804d-18d0305cdc9a)

1.2 Configurar el archivo `application.yml` del proyecto Eureka:
![image](https://github.com/user-attachments/assets/6635cce2-7313-4b29-854e-fed61f4bbf24)

1.3 Anotar la clase principal del proyecto con @EnableEurekaServer para habilitar las funcionalidades de servidor Eureka.
![image](https://github.com/user-attachments/assets/e47345cc-9971-4c6f-8c19-b77d832542e0)

1.4 Al ejecutar el proyecto localhost:8761 se mostrara un pagina indicando que el servidor de eureka esta funcionando.En este caso no existe ningún microservicio funcionando entonces mostrara vacío.
![image](https://github.com/user-attachments/assets/0b09356b-53b6-4d46-8335-10bba2f80661)

### **Paso 2: Crear el proyecto Gateway
![image](https://github.com/user-attachments/assets/a826d736-81d1-446d-b706-901d0d7ffc73)

2.1 Configurar el archivo application.yml del Gateway:
![image](https://github.com/user-attachments/assets/d85bb5b3-5111-4473-a326-5ee3b1c4e8e4)
Este archivo application.yml configura un Spring Cloud Gateway, que actúa como un API Gateway para enrutar solicitudes a diferentes microservicios.
✅ Expone el API Gateway en el puerto 8080
✅ Registra el Gateway en Eureka.
✅ Balancea la carga entre los microservicios students-ms y grades-ms.
✅ Redirige rutas /students/** y /grades/** a sus respectivos microservicios.

### ** Paso 3  Configuración de APIs Students y Grades

3.1 Crear las 2 APIs una para  Students y otra para Grades
![image](https://github.com/user-attachments/assets/a0b42e44-d6a2-4ac6-9795-8df192bf96bc)
![image](https://github.com/user-attachments/assets/114d6bea-1ad9-48bf-900c-43810a59cdba)

3.2 Agregar dependencia para Eureka Client en el archivo pom.xml en la sección dependecias
![image](https://github.com/user-attachments/assets/d1879887-0a9d-4f58-8c55-b9ee5072f96a)

3.3 Configuración de application.yml
![image](https://github.com/user-attachments/assets/095981ce-691f-4b31-b6d2-112b0243d024)
Los microservicios que se ejecutaran como clientes de eureka. Se debe configurar que el puerto sea 0 inicialmente debido a que el puerto debe ser aleatorio.

3.4 Agregamos Un Controller a Students 
![image](https://github.com/user-attachments/assets/d3ddc102-73b7-4e45-8715-8e1ed855c436)
¿Qué hace este StudentController?
✅ Expone un endpoint GET en /students.
✅ Devuelve "estudiantes" como respuesta cuando se accede a la ruta.
✅ Es un controlador REST en Spring Boot con Kotlin.

3.5  Hacemos las mismas configuraciones de estudents pero con grades. Agregar dependencia para Eureka Client en el archivo pom.xml en la sección dependecias
![image](https://github.com/user-attachments/assets/e8394c3a-92e0-4f33-af3d-c575a085dc85)

3.6 Configuración de application.yml 
![image](https://github.com/user-attachments/assets/89f1bb83-80e7-4f77-a172-3f1cc8a293f6)
Los microservicios que se ejecutaran como clientes de eureka. Se debe configurar que el puerto sea 0 inicialmente debido a que el puerto debe ser aleatorio.

3.7 Agregamos Un Controller a Grades 
![image](https://github.com/user-attachments/assets/a9c6ab19-3ed5-442f-83b6-d9340ce0f13a)
¿Qué hace este GradesController?
✅ Expone un endpoint GET en /grades.
✅ Devuelve "listado de calificaciones" como respuesta cuando se accede a la ruta.
✅ Es un controlador REST en Spring Boot con Kotlin.

### **Paso 4 Comprobar registro en Eureka server

4.1 Actualizar la url de eureka server localhost:8761 donde se mostrar el nuevo registro, mas bien los registros STUDENTS-MS,GRADES-MS y GATEWAY
![image](https://github.com/user-attachments/assets/e57dfc73-8f40-49ff-a7f6-64418f347973)

4.2 Probar que funcione correctame las APIS de Students-Grades 

STUDENTS-MS
![image](https://github.com/user-attachments/assets/5e21753a-69a1-4c9d-ae54-c87548de3b4e)

GRADES
![image](https://github.com/user-attachments/assets/2b1c47dc-58e6-4f0e-92e5-a6752b0847f0)

4.3 Ahora que funcione Con GATEWAY con el pueto 8080

STUDENTS
![image](https://github.com/user-attachments/assets/6e503694-798f-4322-972f-6961ca489afd)

Grades
![image](https://github.com/user-attachments/assets/305d9d69-daa7-46f0-b79c-6beb1e89ed78)

### ** Paso 9. Concluciones
La integración de Spring Cloud Gateway, Eureka y microservicios en Kotlin permite construir una arquitectura altamente flexible, escalable y modular.
La centralización de rutas mediante el Gateway optimiza la gestión del tráfico y mejora la seguridad, mientras que Eureka facilita el descubrimiento dinámico de servicios, eliminando configuraciones manuales.
Esta combinación no solo simplifica la administración del sistema, sino que también mejora la eficiencia en el desarrollo, despliegue y mantenimiento de aplicaciones distribuidas, garantizando mayor resiliencia y disponibilidad.

### ** Paso 10 Bibliografia 
Fehling, C., Leymann, F., Retter, R., Schupeck, W., & Arbitter, P. (2014). Cloud Computing Patterns: Fundamentals to Design, Build, and Manage Cloud Applications. Springer.
Kane, A. (2024). Docker in Action, Second Edition. Manning Publications.
Merkel, D. (2014). Docker: Lightweight Linux Containers for Consistent Development and Deployment. Linux Journal, 2014(239), 2.
Pahl, C., & Brogi, A. (2016). Cloud Container Technologies: A State-of-the-Art Review. IEEE Transactions on Cloud Computing, 5(4), 667-678.
Richardson, C. (2019). Microservices Patterns: With Examples in Java. Manning Publications.
Walls, C. (2021). Spring in Action, Sixth Edition. Manning Publications.





































