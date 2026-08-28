# TPF - Desarrollo de Software

Aplicación Full Stack desarrollada como Trabajo Práctico Final utilizando **NestJS** para el backend y **Angular** para el frontend.

## Tecnologías utilizadas

### Backend

* NestJS
* TypeScript
* TypeORM
* SQLite
* JWT (JSON Web Token)
* Passport
* bcrypt
* Resend (envío de correos electrónicos)

### Frontend

* Angular
* TypeScript
* HTML
* CSS

---

## Base de datos

El proyecto utiliza **SQLite** como sistema de base de datos y **TypeORM** como ORM para la gestión de entidades, relaciones y consultas, permitiendo interactuar con la base de datos mediante clases y repositorios.

---

## Funcionalidades

* Registro de usuarios.
* Inicio de sesión mediante JWT.
* Encriptación de contraseñas con bcrypt.
* Verificación de correo electrónico mediante Resend.
* Protección de rutas utilizando Guards.
* Autorización basada en roles.
* Gestión de usuarios.
* Comunicación entre frontend y backend mediante API REST.

---

## Reglas de negocio

* Todo **producto debe tener una categoría** (obligatorio). Se valida tanto en el backend (`CreateProductInput`) como en la base de datos (columna `categoryId` NOT NULL).
* **No se puede eliminar una categoría que tenga productos asociados**. El backend responde `409 Conflict` con un mensaje descriptivo.
* El **primer usuario registrado** asume el rol `admin`; los siguientes se crean como `user`.
* La creación, edición y eliminación de productos y categorías requiere rol `admin`.


## Instalación / Puesta en marcha

### Backend

```bash
cd back
npm install
```

* `npm install` descarga automáticamente el binario nativo de `better-sqlite3`. Si la instalación falla o no se genera, ejecutar manualmente:

```bash
cd node_modules/better-sqlite3
npx prebuild-install
```

* Copiar `.env.example` a `.env` y completar los valores:

```bash
cp .env.example .env
```

| Variable | Descripción |
| --- | --- |
| `JWT_SECRET` | Clave secreta para firmar los tokens JWT (obligatoria). |
| `RESEND_API_KEY` | API key de Resend para el envío de correos. |
| `RESEND_FROM_EMAIL` | Remitente de los correos. |

* La base de datos `database.sqlite` se crea automáticamente al iniciar (TypeORM con `synchronize: true`), no requiere configuración adicional.
* Iniciar el servidor en modo desarrollo:

```bash
npm run start:dev
```

El backend queda disponible en `http://localhost:3000`.

### Frontend

```bash
cd front
npm install
npm start
```

La aplicación apunta a la API en `http://localhost:3000` (configurado en `front/src/environments/environment.ts`). Ajustar ese valor si el backend corre en otra URL o puerto.

---

## Autenticación

El backend implementa autenticación mediante **JWT (JSON Web Token)**. Tras iniciar sesión, el servidor genera un token que debe enviarse en las solicitudes a los endpoints protegidos para identificar y autorizar al usuario.

---

## Verificación de correo electrónico

Durante el registro se genera un token de verificación y se envía un correo electrónico utilizando **Resend**. Una vez que el usuario accede al enlace recibido, su cuenta queda marcada como verificada y puede acceder a las funcionalidades que requieren autenticación.

### Nota sobre Resend

* Si `RESEND_FROM_EMAIL` está **vacío**, el backend usa automáticamente el remitente de prueba `onboarding@resend.dev` (no requiere dominio verificado).
* Resend solo permite enviar correos desde **dominios verificados** en su panel o desde `onboarding@resend.dev`. Usar un remitente no verificado (por ejemplo un correo de Gmail) devuelve un error `403` y el envío falla.
* Una API key inválida o revocada también impide el envío: la respuesta de la API de Resend devuelve `400 API key is invalid`.

---

## Control de acceso por roles

La aplicación implementa autorización basada en roles mediante:

* Decoradores personalizados (`@Roles`).
* `JwtAuthGuard`.
* `RolesGuard`.

Esto permite restringir el acceso a determinados recursos según el rol asignado al usuario.

---

## Autor

**Mateo Cardonnet**

Trabajo Práctico Final – Desarrollo de Software.