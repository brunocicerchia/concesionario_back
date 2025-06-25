# Concesionaria API - Documentación de Endpoints

Sistema de gestión para concesionaria de vehículos desarrollado con Spring Boot.

## Información General

- **Puerto**: 8080
- **Base URL**: `http://localhost:8080`
- **Base de datos**: SQL Server
- **Framework**: Spring Boot 3.5.0
- **Java**: 17

## Endpoints de la API

### 🔐 Autenticación

**Base URL**: `/api/auth`

| Método | Endpoint             | Descripción             | Request Body |
| ------ | -------------------- | ----------------------- | ------------ |
| `POST` | `/api/auth/register` | Registrar nuevo usuario | `Usuario`    |
| `POST` | `/api/auth/login`    | Iniciar sesión          | `Usuario`    |

#### Ejemplo Usuario

```json
{
  "username": "admin",
  "password": "password123",
  "role": "ADMIN"
}
```

---

### 👥 Clientes

**Base URL**: `/api/clientes`

| Método | Endpoint             | Descripción               | Parámetros              |
| ------ | -------------------- | ------------------------- | ----------------------- |
| `POST` | `/api/clientes`      | Crear nuevo cliente       | Request Body: `Cliente` |
| `GET`  | `/api/clientes`      | Listar todos los clientes | -                       |
| `GET`  | `/api/clientes/{id}` | Obtener cliente por ID    | `id`: Long              |

#### Ejemplo Cliente

```json
{
  "nombre": "Juan",
  "apellido": "Pérez",
  "documento": "12345678",
  "email": "juan.perez@email.com",
  "telefono": "+54911234567"
}
```

---

### 🚗 Vehículos

**Base URL**: `/api/vehiculos`

| Método | Endpoint                     | Descripción                  | Parámetros               |
| ------ | ---------------------------- | ---------------------------- | ------------------------ |
| `POST` | `/api/vehiculos`             | Crear nuevo vehículo         | Request Body: `Vehiculo` |
| `GET`  | `/api/vehiculos`             | Listar todos los vehículos   | -                        |
| `GET`  | `/api/vehiculos/disponibles` | Listar vehículos disponibles | -                        |
| `GET`  | `/api/vehiculos/{id}`        | Obtener vehículo por ID      | `id`: Long               |

#### Ejemplo Vehículo

```json
{
  "marca": "Toyota",
  "modelo": "Corolla",
  "color": "Blanco",
  "chasis": "CHASIS123456",
  "motor": "MOTOR789012",
  "precioBase": 25000.0,
  "tipo": "AUTO"
}
```

#### Tipos de Vehículo

- `AUTO`
- `CAMIONETA`
- `MOTO`
- `CAMION`

---

### 📋 Pedidos

**Base URL**: `/api/pedidos`

| Método | Endpoint                      | Descripción                  | Parámetros                                      |
| ------ | ----------------------------- | ---------------------------- | ----------------------------------------------- |
| `POST` | `/api/pedidos`                | Crear nuevo pedido           | Request Body: `Pedido`                          |
| `GET`  | `/api/pedidos`                | Listar todos los pedidos     | -                                               |
| `GET`  | `/api/pedidos/{id}`           | Obtener pedido por ID        | `id`: Long                                      |
| `GET`  | `/api/pedidos/{id}/historial` | Obtener historial de estados | `id`: Long                                      |
| `PUT`  | `/api/pedidos/{id}/estado`    | Avanzar estado del pedido    | `id`: Long, `nuevoEstado`: String               |
| `GET`  | `/api/pedidos/reportes`       | Reporte de pedidos filtrado  | `desde`: LocalDate, `estado`: String (opcional) |

#### Ejemplo Pedido

```json
{
  "numeroPedido": "PED-001",
  "configuracionExtra": "Llantas deportivas",
  "formaPago": "CONTADO",
  "cliente": {
    "id": 1
  },
  "vehiculo": {
    "id": 1
  }
}
```

#### Estados de Pedido

- `VENTAS`
- `COBRANZAS`
- `IMPUESTOS`
- `EMBARQUE`
- `LOGISTICA`
- `ENTREGA`

#### Formas de Pago

- `CONTADO`
- `TRANSFERENCIA`
- `TARJETA`

---

### 💳 Pagos

**Base URL**: `/api/pedidos/{pedidoId}/pagos`

| Método | Endpoint                                      | Descripción                      | Parámetros                                          |
| ------ | --------------------------------------------- | -------------------------------- | --------------------------------------------------- |
| `POST` | `/api/pedidos/{pedidoId}/pagos/contado`       | Registrar pago al contado        | `pedidoId`: Long, Request Body: `PagoContado`       |
| `POST` | `/api/pedidos/{pedidoId}/pagos/transferencia` | Registrar pago por transferencia | `pedidoId`: Long, Request Body: `PagoTransferencia` |
| `POST` | `/api/pedidos/{pedidoId}/pagos/tarjeta`       | Registrar pago con tarjeta       | `pedidoId`: Long, Request Body: `PagoTarjeta`       |
| `GET`  | `/api/pedidos/{pedidoId}/pagos`               | Listar pagos de un pedido        | `pedidoId`: Long                                    |

#### Ejemplo Pago Contado

```json
{
  "descuento": 1000.0
}
```

#### Ejemplo Pago Transferencia

```json
{
  "banco": "Banco Nación",
  "cbu": "1234567890123456789012"
}
```

#### Ejemplo Pago Tarjeta

```json
{
  "numeroTarjeta": "1234567812345678",
  "titular": "Juan Pérez",
  "fechaExpiracion": "2025-12-31",
  "cvv": "123"
}
```

---

### 📊 Reportes

**Base URL**: `/api/reportes`

| Método | Endpoint                    | Descripción               | Parámetros                                                          |
| ------ | --------------------------- | ------------------------- | ------------------------------------------------------------------- |
| `GET`  | `/api/reportes/pedidos`     | Reporte de pedidos JSON   | `desde`: LocalDate, `estado`: String (opcional)                     |
| `GET`  | `/api/reportes/totales`     | Totales por forma de pago | `desde`: LocalDate, `hasta`: LocalDate, `incluirImpuestos`: boolean |
| `GET`  | `/api/reportes/pedidos/csv` | Exportar pedidos a CSV    | `desde`: LocalDate, `hasta`: LocalDate, `estado`: String (opcional) |

#### Parámetros de Fecha

Formato: `YYYY-MM-DD` (ISO 8601)

Ejemplo: `2024-01-01`

---

## Códigos de Respuesta HTTP

| Código | Descripción                                              |
| ------ | -------------------------------------------------------- |
| `200`  | OK - Operación exitosa                                   |
| `201`  | Created - Recurso creado exitosamente                    |
| `400`  | Bad Request - Parámetros inválidos                       |
| `404`  | Not Found - Recurso no encontrado                        |
| `409`  | Conflict - Recurso duplicado o restricción de integridad |
| `500`  | Internal Server Error - Error interno del servidor       |

## Configuración CORS

La API está configurada para permitir requests desde:

- `http://localhost:3000`

Métodos permitidos: `GET`, `POST`, `PUT`, `DELETE`, `OPTIONS`

## Patrones de Diseño Implementados

### Strategy Pattern

Para el cálculo de impuestos según el tipo de vehículo:

- **AutoImpuestoStrategy**: 21% nacional + 5% provincial + 1% adicional
- **CamionetaImpuestoStrategy**: 10% nacional + 5% provincial + 2% adicional
- **MotoImpuestoStrategy**: 5% provincial
- **CamionImpuestoStrategy**: 5% provincial

### Facade Pattern

Implementado en los servicios para simplificar las operaciones complejas y coordinar múltiples componentes.

## Ejecutar la Aplicación

```bash
# Con Maven Wrapper
./mvnw spring-boot:run

# O en Windows
mvnw.cmd spring-boot:run
```

La aplicación estará disponible en `http://localhost:8080`

## Tecnologías Utilizadas

- **Spring Boot 3.5.0**
- **Java 17**
- **SQL Server**
- **Spring Data JPA**
- **Spring Security**
- **Maven**

## Estructura del Proyecto

```
src/
├── main/
│   ├── java/
│   │   └── com/concesionaria/
│   │       ├── controller/     # Controladores REST
│   │       ├── model/          # Entidades JPA
│   │       ├── service/        # Lógica de negocio
│   │       ├── repository/     # Repositorios JPA
│   │       └── config/         # Configuraciones
│   └── resources/
│       └── application.properties
```

