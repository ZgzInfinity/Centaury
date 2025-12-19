# Gif App - Intermedio Avanzado

Esta versión añade funcionalidades avanzadas como paginación infinita (infinite scroll), agrupación de GIFs, gestión de estado de scroll, y uso de lifecycle hooks con `viewChild()`.

## Contenido del Proyecto

### Nuevas Funcionalidades

- ✅ **Paginación infinita** en trending
- ✅ **Agrupación de GIFs** en grupos de 3
- ✅ **Persistencia de posición de scroll**
- ✅ **Lifecycle hooks** (`AfterViewInit`)
- ✅ **viewChild()** para referencias a elementos del DOM
- ✅ **Scroll infinito** con detección automática

### Mejoras Implementadas

#### 1. **GifService Mejorado** (`services/gifs.service.ts`)

**Paginación infinita:**
```typescript
private trendingPage = signal(0);

loadTrendingGifs() {
  if (this.trendingGifsLoading()) return; // Prevenir múltiples llamadas
  
  this.trendingGifsLoading.set(true);
  
  this.http.get<GiphyResponse>(`${environment.giphyUrl}/gifs/trending`, {
    params: {
      offset: this.trendingPage() * 20, // Paginación
      limit: 20,
    },
  }).subscribe((resp) => {
    const gifs = GifMapper.mapGiphyItemsToGifArray(resp.data);
    // Agregar a la lista existente (no reemplazar)
    this.trendingGifs.update((currentGifs) => [...currentGifs, ...gifs]);
    this.trendingPage.update((page) => page + 1);
    this.trendingGifsLoading.set(false);
  });
}
```

**Agrupación de GIFs:**
```typescript
trendingGifGroup = computed<Gif[][]>(() => {
  const groups = [];
  for (let i = 0; i < this.trendingGifs().length; i += 3) {
    groups.push(this.trendingGifs().slice(i, i + 3));
  }
  return groups; // [[g1,g2,g3], [g4,g5,g6], ...]
});
```

**Características:**
- Acumulación de resultados (no reemplazo)
- Control de página con señal
- Prevención de múltiples llamadas simultáneas
- Agrupación automática en grupos de 3

#### 2. **TrendingPageComponent Avanzado** (`pages/trending-page/`)

**Conceptos avanzados implementados:**

**viewChild() - Referencia a elementos del DOM:**
```typescript
scrollDivRef = viewChild<ElementRef<HTMLDivElement>>('groupDiv');
```

**AfterViewInit - Lifecycle Hook:**
```typescript
export default class TrendingPageComponent implements AfterViewInit {
  ngAfterViewInit(): void {
    const scrollDiv = this.scrollDivRef()?.nativeElement;
    if (!scrollDiv) return;
    
    // Restaurar posición de scroll guardada
    scrollDiv.scrollTop = this.scrollStateService.trendingScrollState();
  }
}
```

**Scroll infinito:**
```typescript
onScroll(event: Event) {
  const scrollDiv = this.scrollDivRef()?.nativeElement;
  if (!scrollDiv) return;
  
  const scrollTop = scrollDiv.scrollTop;
  const clientHeight = scrollDiv.clientHeight;
  const scrollHeight = scrollDiv.scrollHeight;
  
  // Detectar cuando está cerca del final (300px antes)
  const isAtBottom = scrollTop + clientHeight + 300 >= scrollHeight;
  
  // Guardar posición de scroll
  this.scrollStateService.trendingScrollState.set(scrollTop);
  
  // Cargar más si está al final
  if (isAtBottom) {
    this.gifService.loadTrendingGifs();
  }
}
```

#### 3. **ScrollStateService** (`shared/services/scroll-state.service.ts`)

Servicio para gestionar el estado del scroll entre navegaciones:
```typescript
@Injectable({ providedIn: 'root' })
export class ScrollStateService {
  trendingScrollState = signal(0);
}
```

**Propósito:**
- Mantener la posición de scroll cuando el usuario navega
- Restaurar posición al volver a la página
- Mejora la experiencia de usuario

### Template Actualizado

El template de `trending-page` ahora:
- Usa `trendingGifGroup()` en lugar de `trendingGifs()`
- Renderiza grupos de 3 GIFs
- Implementa listener de scroll: `(scroll)="onScroll($event)"`
- Referencia de template: `#groupDiv`

## Conceptos Aprendidos

### 1. **viewChild() - Referencias a Elementos**
```typescript
scrollDivRef = viewChild<ElementRef<HTMLDivElement>>('groupDiv');
// En template: <div #groupDiv>
```

**Características:**
- Acceso al elemento nativo del DOM
- Type-safe con TypeScript
- Usa señales (retorna `Signal<ElementRef | undefined>`)

### 2. **Lifecycle Hooks**
```typescript
implements AfterViewInit {
  ngAfterViewInit(): void {
    // Se ejecuta después de que la vista se inicializa
  }
}
```

**Cuándo usar:**
- Acceso a elementos del DOM
- Inicialización que requiere la vista renderizada
- Configuración de librerías de terceros

### 3. **Paginación Infinita**
Patrón común en aplicaciones modernas:
- Detectar scroll cerca del final
- Cargar más datos automáticamente
- Acumular resultados (no reemplazar)

### 4. **Computed Signals Complejos**
```typescript
trendingGifGroup = computed<Gif[][]>(() => {
  // Lógica compleja de agrupación
  // Se recalcula automáticamente cuando trendingGifs() cambia
});
```

### 5. **Gestión de Estado de UI**
- Servicios para estado de UI (scroll, modales, etc.)
- Persistencia de estado entre navegaciones
- Mejora de UX

### 6. **Prevención de Llamadas Múltiples**
```typescript
if (this.trendingGifsLoading()) return;
```
Evita múltiples peticiones HTTP simultáneas.

### 7. **Acceso a Propiedades del DOM**
```typescript
scrollTop, clientHeight, scrollHeight
```
Propiedades nativas del DOM para cálculos de scroll.

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve
```

Navegar a `http://localhost:4200/dashboard/trending` y hacer scroll para ver la paginación infinita en acción.

## Diferencias con Versiones Anteriores

**Sección 7 (Funcionalidad Básica)**:
- Carga única de trending GIFs
- Lista simple sin agrupación
- Sin paginación
- Sin persistencia de scroll

**Sección 8 (Intermedio Avanzado)**:
- ✅ Paginación infinita
- ✅ Agrupación de GIFs
- ✅ Scroll infinito automático
- ✅ Persistencia de posición de scroll
- ✅ Lifecycle hooks
- ✅ viewChild() para DOM
- ✅ Mejor gestión de estado

## Estructura del Proyecto

```
src/app/
├── gifs/
│   ├── services/
│   │   └── gifs.service.ts        # Servicio mejorado con paginación
│   └── pages/
│       └── trending-page/         # Componente con scroll infinito
└── shared/
    └── services/
        └── scroll-state.service.ts # Gestión de estado de scroll
```

Esta versión demuestra técnicas avanzadas de Angular para crear aplicaciones con mejor rendimiento y experiencia de usuario, incluyendo paginación infinita, gestión de scroll, y uso de lifecycle hooks.
