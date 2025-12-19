# Tesloshop - App Administrativa

Aplicación completa de e-commerce con backend en **NestJS** y frontend en **Angular**. Esta es la versión base que incluye la funcionalidad administrativa completa.

## Contenido del Proyecto

### Backend (NestJS)

**Tecnologías:**
- NestJS framework
- TypeORM para base de datos
- PostgreSQL
- JWT para autenticación
- Swagger para documentación API
- Seed de datos iniciales

**Características:**
- CRUD completo de productos
- Sistema de autenticación
- Gestión de usuarios
- Upload de imágenes
- Relaciones entre entidades (Product, User, ProductImage)

### Frontend (Angular)

**Características:**
- Store front (tienda pública)
- Catálogo de productos por género
- Páginas de detalle de producto
- Navegación y routing
- Integración con API backend

**Estructura:**
- `store-front/`: Módulo de tienda pública
- Componentes de navegación
- Páginas de productos
- Servicios para comunicación con API

## Desarrollo

### Backend

```bash
cd "Tesloshop - Backend"
npm install
npm run start:dev
```

### Frontend

```bash
cd "Tesloshop - frontend"
npm install
ng serve
```

## Estructura

```
Tesloshop - Backend/
├── src/
│   ├── products/        # Módulo de productos
│   ├── auth/            # Módulo de autenticación
│   ├── seed/            # Datos iniciales
│   └── main.ts          # Punto de entrada

Tesloshop - frontend/
├── src/app/
│   ├── store-front/     # Módulo de tienda
│   └── services/        # Servicios HTTP
```

Esta es la base de una aplicación e-commerce completa que se extiende en las siguientes secciones con paginación, autenticación, y panel administrativo.

