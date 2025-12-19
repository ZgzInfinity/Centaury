# Gif App - Funcionalidad Básica

Esta versión añade la funcionalidad completa de búsqueda y visualización de GIFs usando la API de Giphy, incluyendo servicios HTTP, mappers, y gestión de estado con señales.

## Contenido del Proyecto

### Funcionalidades Implementadas

- ✅ Búsqueda de GIFs mediante API de Giphy
- ✅ Visualización de GIFs en tendencia
- ✅ Historial de búsquedas persistido en LocalStorage
- ✅ Navegación a historial de búsquedas específicas
- ✅ Mappers para transformar datos de la API

### Componentes y Servicios

#### 1. **GifService** (`services/gifs.service.ts`)
Servicio principal que gestiona toda la lógica de negocio:

**Características:**
- **HTTP Client**: Integración con API de Giphy
- **Señales reactivas**:
  ```typescript
  trendingGifs = signal<Gif[]>([]);
  trendingGifsLoading = signal(true);
  searchHistory = signal<Record<string, Gif[]>>(loadFromLocalStorage());
  searchHistoryKeys = computed(() => Object.keys(this.searchHistory()));
  ```
- **Persistencia automática** con `effect()`:
  ```typescript
  saveGifsToLocalStorage = effect(() => {
    localStorage.setItem(GIF_KEY, JSON.stringify(this.searchHistory()));
  });
  ```
- **Métodos principales**:
  - `loadTrendingGifs()`: Carga GIFs en tendencia
  - `searchGifs(query: string)`: Busca GIFs y guarda en historial
  - `getHistoryGifs(query: string)`: Obtiene GIFs del historial

**Estructura del historial:**
```typescript
// Record<string, Gif[]>
{
  'goku': [gif1, gif2, gif3],
  'saitama': [gif1, gif2, gif3],
  'dragon ball': [gif1, gif2, gif3],
}
```

#### 2. **SearchPageComponent** (`pages/search-page/`)
- Búsqueda funcional de GIFs
- Integración con `GifService`
- Manejo de observables:
  ```typescript
  onSearch(query: string) {
    this.gifService.searchGifs(query).subscribe((resp) => {
      this.gifs.set(resp);
    });
  }
  ```

#### 3. **TrendingPageComponent** (`pages/trending-page/`)
- Muestra GIFs en tendencia
- Usa `GifService.trendingGifs()` directamente
- Estado de carga con `trendingGifsLoading`

#### 4. **GifHistoryComponent** (`pages/gif-history/`)
- **Conceptos avanzados**: `toSignal()`, `ActivatedRoute`, `computed()`
- Lee parámetros de ruta:
  ```typescript
  query = toSignal(
    inject(ActivatedRoute).params.pipe(map((params) => params['query']))
  );
  ```
- Computed signal que obtiene GIFs del historial:
  ```typescript
  gifsByKey = computed(() => this.gifService.getHistoryGifs(this.query()));
  ```

#### 5. **GifMapper** (`mapper/gif.mapper.ts`)
Transforma datos de la API de Giphy al modelo interno:
```typescript
static mapGiphyItemToGif(item: GiphyItem): Gif {
  return {
    id: item.id,
    title: item.title,
    url: item.images.original.url,
  };
}
```

### Interfaces

#### **Gif Interface** (`interfaces/gif.interface.ts`)
```typescript
export interface Gif {
  id: string;
  title: string;
  url: string;
}
```

#### **Giphy Interfaces** (`interfaces/giphy.interfaces.ts`)
Interfaces completas para la respuesta de la API de Giphy, incluyendo:
- `GiphyResponse`: Respuesta completa
- `GiphyItem`: Item individual de GIF
- `Images`: Diferentes tamaños de imágenes
- Y más...

### Routing

Rutas actualizadas con historial:
```typescript
{
  path: 'history/:query',
  loadComponent: () => import('./gifs/pages/gif-history/gif-history.component'),
}
```

## Conceptos Aprendidos

### 1. **HTTP Client y Observables**
```typescript
this.http.get<GiphyResponse>(url, { params }).subscribe(...)
```

### 2. **RxJS Operators**
- `map()`: Transforma datos
- `tap()`: Efectos secundarios (guardar en historial)

### 3. **toSignal() - Conversión de Observables a Señales**
```typescript
query = toSignal(
  inject(ActivatedRoute).params.pipe(map((params) => params['query']))
);
```

### 4. **ActivatedRoute - Parámetros de Ruta**
Lectura de parámetros dinámicos de la URL:
```typescript
inject(ActivatedRoute).params.pipe(map((params) => params['query']))
```

### 5. **Computed Signals con Dependencias Externas**
```typescript
gifsByKey = computed(() => this.gifService.getHistoryGifs(this.query()));
```

### 6. **Mappers Pattern**
Separación de responsabilidades: transformar datos de API a modelo interno.

### 7. **Record Type en TypeScript**
```typescript
Record<string, Gif[]> // Objeto con claves string y valores Gif[]
```

### 8. **Environment Variables**
Configuración centralizada:
```typescript
environment.giphyApiKey
environment.giphyUrl
```

## Configuración

### Environment (`environments/environment.ts`)
```typescript
export const environment = {
  giphyApiKey: 'tu-api-key',
  giphyUrl: 'https://api.giphy.com/v1',
};
```

**Nota**: Necesitas obtener tu propia API key de [Giphy Developers](https://developers.giphy.com/dashboard/).

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve
```

Navegar a:
- `http://localhost:4200/dashboard/trending` - GIFs en tendencia
- `http://localhost:4200/dashboard/search` - Búsqueda
- `http://localhost:4200/dashboard/history/:query` - Historial específico

## Estructura del Proyecto

```
src/app/gifs/
├── components/
│   ├── gif-list/              # Lista de GIFs
│   └── side-menu/             # Menú lateral
├── interfaces/
│   ├── gif.interface.ts        # Modelo interno
│   └── giphy.interfaces.ts     # Modelos de API
├── mapper/
│   └── gif.mapper.ts          # Transformación de datos
├── pages/
│   ├── dashboard-page/        # Contenedor principal
│   ├── trending-page/         # Tendencias
│   ├── search-page/           # Búsqueda
│   └── gif-history/           # Historial específico
└── services/
    └── gifs.service.ts        # Lógica de negocio
```

## Diferencias con la Versión Anterior

**Sección 6 (Inicio)**:
- Solo estructura y layout
- Sin funcionalidad real

**Sección 7 (Funcionalidad Básica)**:
- ✅ Integración completa con API
- ✅ Búsqueda funcional
- ✅ Historial persistente
- ✅ Routing con parámetros
- ✅ Gestión de estado con señales

Esta versión demuestra cómo construir una aplicación Angular funcional con servicios, HTTP, y gestión de estado reactivo.
