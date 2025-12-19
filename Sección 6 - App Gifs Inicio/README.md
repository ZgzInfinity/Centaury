# GifsApp - Inicio

Esta es la versión inicial de la aplicación de GIFs, que establece la estructura básica y el layout de la aplicación sin funcionalidad de API todavía.

## Contenido del Proyecto

### Estructura de la Aplicación

Esta versión se enfoca en:
- **Layout y estructura de componentes**
- **Routing con lazy loading**
- **Componentes básicos sin lógica de negocio**

### Componentes Principales

#### 1. **DashboardPageComponent** (`pages/dashboard-page/`)
- Página principal que actúa como contenedor
- Incluye el menú lateral (`SideMenuComponent`) y el router outlet para rutas hijas
- Estructura base del layout

#### 2. **SideMenuComponent** (`components/side-menu/`)
- Menú lateral de navegación
- Compuesto por:
  - `SideMenuHeaderComponent`: Encabezado del menú
  - `SideMenuOptionsComponent`: Opciones de navegación

#### 3. **TrendingPageComponent** (`pages/trending-page/`)
- Página de tendencias
- **Estado actual**: Muestra imágenes estáticas de ejemplo (placeholders)
- Usa `GifListComponent` para mostrar la lista
- Implementa señales básicas:
  ```typescript
  gifs = signal(imageUrls); // URLs estáticas de ejemplo
  ```

#### 4. **SearchPageComponent** (`pages/search-page/`)
- Página de búsqueda
- **Estado actual**: Componente vacío (solo estructura)
- Usa `ChangeDetectionStrategy.OnPush` para optimización

#### 5. **GifListComponent** (`components/gif-list/`)
- Componente para mostrar lista de GIFs
- Compuesto por `GifListItemComponent` para cada item
- Estructura preparada para recibir datos reales

### Routing

Implementa routing con **lazy loading** de componentes:

```typescript
{
  path: 'dashboard',
  loadComponent: () => import('./gifs/pages/dashboard-page/dashboard-page.component'),
  children: [
    { path: 'trending', loadComponent: () => import(...) },
    { path: 'search', loadComponent: () => import(...) },
  ]
}
```

**Conceptos clave:**
- `loadComponent()`: Carga perezosa de componentes (lazy loading)
- Rutas hijas con `children`
- Redirección con `**` path

## Conceptos Aprendidos

### 1. **Lazy Loading de Componentes**
Angular permite cargar componentes de forma perezosa:
```typescript
loadComponent: () => import('./path/to/component')
```

### 2. **Arquitectura de Componentes**
- Separación entre páginas y componentes reutilizables
- Componentes compuestos (SideMenu con sub-componentes)
- Estructura escalable

### 3. **Routing Anidado**
- Rutas padre con hijos
- Router outlet para renderizar componentes hijos

### 4. **Change Detection Strategy**
```typescript
changeDetection: ChangeDetectionStrategy.OnPush
```
Optimiza el rendimiento limitando la detección de cambios.

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve
```

Navegar a `http://localhost:4200/` que redirige a `/dashboard/trending`.

## Estado del Proyecto

Esta versión es principalmente una **estructura base** sin funcionalidad real:
- ✅ Layout y navegación
- ✅ Routing configurado
- ✅ Componentes básicos
- ❌ Sin integración con API
- ❌ Sin búsqueda funcional
- ❌ Sin persistencia de datos

**Próximos pasos**: En las siguientes secciones se añadirá la funcionalidad completa con servicios, HTTP, y gestión de estado.
