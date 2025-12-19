# Angular App Países - Estructura Inicial

Esta es la versión inicial de la aplicación de países, que establece la estructura básica, layout y componentes sin funcionalidad de API todavía.

## Contenido del Proyecto

### Estructura de la Aplicación

Esta versión se enfoca en:
- **Layout y estructura de componentes**
- **Routing con layouts anidados**
- **Componentes básicos sin lógica de negocio**
- **Arquitectura modular con feature modules**

### Componentes Principales

#### 1. **CountryLayoutComponent** (`layouts/CountryLayout/`)
- Layout compartido para todas las páginas de países
- Incluye menú superior y router outlet para rutas hijas
- Estructura base del módulo de países

#### 2. **TopMenuComponent** (`components/top-menu/`)
- Menú de navegación superior
- Navegación entre diferentes vistas de búsqueda

#### 3. **SearchInputComponent** (`components/search-input/`)
- Componente reutilizable para búsqueda
- Emite eventos cuando el usuario busca
- Preparado para recibir funcionalidad

#### 4. **CountryListComponent** (`components/country-list/`)
- Componente para mostrar lista de países
- Estructura preparada para recibir datos reales

#### 5. **Páginas de Búsqueda**

**ByCapitalPageComponent** (`pages/by-capital-page/`):
- Búsqueda por capital
- Estado actual: Solo estructura, `onSearch()` hace `console.log`

**ByCountryPageComponent** (`pages/by-country-page/`):
- Búsqueda por nombre de país
- Estructura básica

**ByRegionPageComponent** (`pages/by-region-page/`):
- Búsqueda por región
- Estructura básica

**CountryPageComponent** (`pages/country-page/`):
- Detalle de un país específico
- Ruta dinámica: `/by/:code`

### Routing

Implementa routing modular con layouts:

```typescript
{
  path: '',
  component: CountryLayoutComponent,
  children: [
    { path: 'by-capital', component: ByCapitalPageComponent },
    { path: 'by-country', component: ByCountryPageComponent },
    { path: 'by-region', component: ByRegionPageComponent },
    { path: 'by/:code', component: CountryPageComponent },
  ]
}
```

**Conceptos clave:**
- Rutas hijas con layout compartido
- Rutas dinámicas con parámetros (`:code`)
- Redirección con `**` path

### Estructura de Carpetas

```
src/app/
├── country/
│   ├── components/          # Componentes reutilizables
│   ├── layouts/             # Layouts compartidos
│   ├── pages/              # Páginas de la feature
│   └── country.routes.ts   # Rutas del módulo
└── shared/
    ├── components/         # Componentes compartidos globales
    └── pages/              # Páginas compartidas
```

## Conceptos Aprendidos

### 1. **Feature Modules Pattern**
Organización del código por features (países):
- Todo lo relacionado con países en una carpeta
- Rutas agrupadas en `country.routes.ts`
- Layout específico para la feature

### 2. **Layout Components**
Componentes que actúan como contenedores:
- Layout compartido para múltiples páginas
- Router outlet para renderizar contenido
- Menús y navegación compartidos

### 3. **Routing Anidado con Layouts**
```typescript
{
  path: '',
  component: LayoutComponent,
  children: [/* rutas hijas */]
}
```

### 4. **Rutas Dinámicas**
```typescript
{ path: 'by/:code', component: CountryPageComponent }
```
Parámetros de ruta accesibles con `ActivatedRoute`.

### 5. **Arquitectura Escalable**
- Separación por features
- Componentes reutilizables
- Estructura mantenible

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve
```

Navegar a `http://localhost:4200/country` que muestra el layout con las opciones de búsqueda.

## Estado del Proyecto

Esta versión es principalmente una **estructura base** sin funcionalidad real:
- ✅ Layout y navegación
- ✅ Routing configurado con layouts
- ✅ Componentes básicos
- ✅ Arquitectura modular
- ❌ Sin integración con API
- ❌ Sin búsqueda funcional
- ❌ Sin servicios

**Próximos pasos**: En las siguientes secciones se añadirá la funcionalidad completa con servicios HTTP, recursos reactivos (`rxResource`), y gestión de estado.
