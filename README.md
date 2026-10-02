# Enrutamiento Dinámico y Descubrimiento de Servicios en API Gateway

Implementación basada en el artículo de GeeksforGeeks: [Dynamic Routing and Service Discovery in API Gateway](https://www.geeksforgeeks.org/advance-java/dynamic-routing-and-service-discovery-in-api-gateway/)

## Arquitectura

```
Cliente → API Gateway (9056) → Eureka Server (9099) → User Service (8086)
```

## Componentes

| Módulo | Puerto | Descripción |
|--------|--------|-------------|
| **eureka-server** | 9099 | Registro de servicios (Service Registry) |
| **user-service** | 8086 | Microservicio que se registra en Eureka |
| **api-gateway** | 9056 | Gateway con enrutamiento dinámico vía `lb://USER-SERVICE` |


### 1. Iniciar Eureka Server
```bash
cd eureka-server
mvn spring-boot:run
```
Verificar: http://localhost:9099

### 2. Iniciar User Service
```bash
cd user-service
mvn spring-boot:run
```
Se registra automáticamente en Eureka.

### 3. Iniciar API Gateway
```bash
cd api-gateway
mvn spring-boot:run
```

## Probar

```bash
# A través del API Gateway (enrutamiento dinámico)
curl http://localhost:9056/cliente
# Respuesta: "Bienvenido al cliente"
```

## Funcionamiento

1. **User Service** inicia y se registra en **Eureka Server**
2. **API Gateway** se registra en Eureka y obtiene el registro de servicios
3. Cliente hace request a `http://localhost:9056/client`
4. API Gateway consulta Eureka → obtiene instancias de `USER-SERVICE`
5. Balancea carga y reenvía la petición al User Service

By: Frida Martina Ariosa Arias & Carlos Mario Bechara Arias.
