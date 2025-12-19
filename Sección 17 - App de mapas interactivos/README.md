# MapsApp - App de Mapas Interactivos

Esta aplicación demuestra la integración de **Mapbox GL JS** con Angular, incluyendo mapas interactivos, marcadores, y visualización de propiedades inmobiliarias.

## Contenido del Proyecto

### Funcionalidades

- ✅ Mapas interactivos con Mapbox
- ✅ Marcadores dinámicos
- ✅ Visualización de propiedades inmobiliarias
- ✅ Mini mapas reutilizables
- ✅ Controles de zoom y navegación
- ✅ Sincronización de estado con señales

### Componentes Principales

#### 1. **FullscreenMapPageComponent**
Mapa a pantalla completa con controles:

**Características:**
- Inicialización del mapa en `ngAfterViewInit`
- Uso de `viewChild()` para referencia al elemento del DOM
- Señales para zoom y coordenadas
- `effect()` para sincronizar zoom con el mapa
- Listeners de eventos del mapa:
  - `zoomend`: Actualiza señal de zoom
  - `moveend`: Actualiza coordenadas
  - `load`: Cuando el mapa carga

**Implementación:**
```typescript
divElement = viewChild<ElementRef>('map');
map = signal<mapboxgl.Map | null>(null);
zoom = signal(14);
coordinates = signal({ lng: -74.5, lat: 40 });

zoomEffect = effect(() => {
  if (!this.map()) return;
  this.map()?.setZoom(this.zoom());
});
```

#### 2. **MarkersPageComponent**
Demuestra cómo agregar y gestionar marcadores dinámicamente.

#### 3. **HousesPageComponent**
Visualización de propiedades inmobiliarias con mini mapas.

#### 4. **MiniMapComponent**
Componente reutilizable de mapa pequeño:

```typescript
export class MiniMapComponent implements AfterViewInit {
  divElement = viewChild<ElementRef>('map');
  lngLat = input.required<{ lng: number; lat: number }>();
  zoom = input<number>(14);
  
  // Mapa no interactivo con marcador
}
```

## Conceptos Aprendidos

### 1. **Integración de Librerías de Terceros**

**Mapbox GL JS:**
```typescript
import mapboxgl from 'mapbox-gl';
mapboxgl.accessToken = environment.mapboxKey;
```

### 2. **Inicialización en ngAfterViewInit**

```typescript
async ngAfterViewInit() {
  if (!this.divElement()?.nativeElement) return;
  await new Promise((resolve) => setTimeout(resolve, 80));
  // Inicializar mapa
}
```

### 3. **Sincronización Bidireccional**

- Mapa → Señales: Listeners de eventos actualizan señales
- Señales → Mapa: Effects actualizan el mapa

### 4. **Controles de Mapbox**

```typescript
map.addControl(new mapboxgl.FullscreenControl());
map.addControl(new mapboxgl.NavigationControl());
map.addControl(new mapboxgl.ScaleControl());
```

### 5. **Marcadores**

```typescript
new mapboxgl.Marker()
  .setLngLat([lng, lat])
  .addTo(map);
```

## Configuración

### Environment Variables

```typescript
export const environment = {
  mapboxKey: 'tu-api-key-de-mapbox'
};
```

**Nota**: Necesitas obtener una API key de [Mapbox](https://www.mapbox.com/).

### Scripts

```bash
npm run set-envs  # Configurar variables de entorno
```

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve
```

Navegar a:
- `http://localhost:4200/fullscreen` - Mapa a pantalla completa
- `http://localhost:4200/markers` - Marcadores
- `http://localhost:4200/houses` - Propiedades

## Estructura del Proyecto

```
src/app/
├── maps/
│   └── components/
│       └── mini-map/      # Componente de mapa pequeño
├── pages/
│   ├── fullscreen-map-page/  # Mapa completo
│   ├── markers-page/         # Marcadores
│   └── houses-page/          # Propiedades
└── environments/
    └── environment.ts        # Configuración de Mapbox
```

Esta aplicación demuestra cómo integrar librerías de mapas con Angular, gestionar estado reactivo, y crear componentes reutilizables para visualización geográfica.
