# Sistema Estacionamiento

Gestión de estacionamientos con control de ingresos/egresos, cuentas corrientes, abonos mensuales y recarga de saldo.

## Tecnologías

| Backend | Frontend | Infra |
|---------|----------|-------|
| Java 21 + Spring Boot 4.1 | Next.js 16 (App Router) | Docker Compose |
| Spring Data JPA + Hibernate 7 | React 19 + TypeScript | PostgreSQL 16 |
|  PostgreSQL / H2 | Tailwind CSS 4 + shadcn/ui | H2 (desarrollo local) |
| MapStruct 1.6 + Lombok | React Hook Form + Zod | pnpm 10 |
|  | Recharts + date-fns | |

## Inicio rápido

```bash
docker-compose up --build
```

- Frontend: `http://localhost:3000`
- Backend API: `http://localhost:8080`
- Base de datos: PostgreSQL en `localhost:5432` (user/pass: `postgres`)

## Desarrollo local

### Backend

```bash
cd estacionamiento-backend
./mvnw spring-boot:run    # usa H2 en memoria por defecto
./mvnw test               # tests con H2
```

Para usar PostgreSQL local, descomentar las líneas correspondientes en `application.properties` y tener PostgreSQL corriendo.

### Frontend

```bash
cd front
pnpm install
pnpm dev                 # http://localhost:3000
pnpm test                # vitest
```

El frontend en dev redirige `/api/*` a `http://localhost:8080` (configurable vía `BACKEND_URL`).

## API

| Método | Ruta | Descripción |
|--------|------|-------------|
| `GET` | `/api/estacionamientos` | Lista estacionamientos con disponibilidad |
| `GET` | `/api/estacionamientos/{id}/plazas` | Plazas de un estacionamiento |
| `POST` | `/api/estacionamientos` | Crear estacionamiento |
| `POST` | `/api/usos-estacionamiento/ingreso` | Registrar ingreso de vehículo |
| `POST` | `/api/usos-estacionamiento/salida` | Registrar salida y calcular cobro |
| `GET` | `/api/vehiculos/buscar?patente=` | Buscar vehículo por patente |
| `POST` | `/api/cuentas/registro-completo` | Registrar usuario + cuenta + vehículo |
| `POST` | `/api/cuentas/{id}/recargar` | Recargar saldo |
| `POST` | `/api/cuentas/{id}/pagar-abono` | Pagar abono mensual |
| `GET` | `/api/cuentas/{id}/abono-info` | Info del abono vigente |

## Seed data

Al iniciar, `DataInitializer` carga:

- **3 roles**: ESTUDIANTE (100% desc.), PROFESOR (50%), ADMINISTRATIVO (50%)
- **4 usuarios** con cuentas y vehículos (Juan, María, Carlos, Lucía)
- **2 estacionamientos**: Central (30 plazas, 12 ocupadas) y Norte (20 plazas, 8 ocupadas)
- **Usos activos** con vehículos invitados en plazas ocupadas

## Estructura

```
estacionamiento-backend/
├── src/main/java/estacionamiento/
│   ├── controller/     # REST controllers
│   ├── service/        # Lógica de negocio
│   ├── repository/     # Spring Data JPA
│   ├── model/          # Entidades JPA
│   ├── dto/            # DTOs de request/response
│   ├── mapper/         # MapStruct mappers
│   ├── exception/      # Excepciones + handler global
│   ├── enums/          # Enums compartidos
│   └── config/         # DataInitializer
front/
├── app/                # Next.js App Router (páginas)
├── components/         # UI components (shadcn/ui)
├── hooks/              # Custom hooks (API calls)
├── lib/                # Utilidades
└── styles/             # Estilos globales
```

## Docker

```bash
docker-compose up --build     # construir e iniciar
docker-compose down           # detener
docker-compose down -v        # detener y borrar volúmenes (reinicia datos)
```