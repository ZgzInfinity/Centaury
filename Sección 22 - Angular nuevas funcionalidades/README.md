# Angular Nuevas Funcionalidades

Este proyecto demuestra las **nuevas funcionalidades de Angular** incluyendo control flow moderno (`@if`, `@for`, `@switch`), view transitions, defer blocks, y optimizaciones de change detection.

## Contenido del Proyecto

### Funcionalidades Demostradas

#### 1. **Control Flow Moderno**

**@if, @else:**
```html
@if( showContent() ) {
  <p>Hola Mundo</p>
} @else {
  <p>*********</p>
}
```

**@switch:**
```html
@switch ( grade() ) {
  @case ('A') {
    <p>Arriba de 90</p>
  }
  @case ('B') {
    <p>Arriba de 80</p>
  }
  @default {
    <p>Reprobado</p>
  }
}
```

**@for:**
```html
@for (framework of frameworks(); track framework;
  let i = $index, first = $first, last = $last, 
  even = $even, odd = $odd, count = $count) {
  <li>{{ i + 1 }}/{{ count }} - {{ framework }}</li>
} @empty {
  <li>No hay frameworks agregados</li>
}
```

#### 2. **View Transitions**

```typescript
provideRouter(
  routes,
  withViewTransitions({
    skipInitialTransition: true,
  })
)
```

Transiciones suaves entre rutas.

#### 3. **Defer Blocks**

Carga perezosa de componentes y contenido:

```html
@defer (on viewport) {
  <heavy-component />
} @placeholder {
  <div>Loading...</div>
} @loading {
  <div>Loading content...</div>
} @error {
  <div>Error loading</div>
}
```

#### 4. **Change Detection Optimizado**

```typescript
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush,
  // ...
})
```

Con señales y computed para mejor rendimiento.

## Conceptos Aprendidos

### 1. **Control Flow vs Directivas Antiguas**

**Antes:**
```html
<div *ngIf="condition">Content</div>
<div *ngFor="let item of items">...</div>
```

**Ahora:**
```html
@if (condition) { <div>Content</div> }
@for (item of items; track item.id) { ... }
```

**Ventajas:**
- Mejor type safety
- Más legible
- Mejor rendimiento
- Variables locales (`$index`, `$first`, etc.)

### 2. **View Transitions API**

Transiciones nativas del navegador entre páginas.

### 3. **Defer Blocks**

Carga perezosa con múltiples triggers:
- `on idle`: Cuando el navegador está inactivo
- `on viewport`: Cuando entra en viewport
- `on immediate`: Inmediatamente
- `on timer(duration)`: Después de un tiempo
- `on interaction`: Cuando el usuario interactúa

### 4. **Signals y Change Detection**

Mejor rendimiento con `OnPush` + signals.

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve -o
```

Navegar por las diferentes páginas para ver las nuevas funcionalidades en acción.

## Estructura del Proyecto

```
src/app/
├── dashboard/
│   └── pages/
│       ├── control-flow/      # @if, @for, @switch
│       ├── defer-views/       # Defer blocks
│       ├── defer-options/     # Opciones de defer
│       └── change-detection/  # Optimizaciones
└── app.config.ts              # View transitions config
```

Este proyecto muestra las últimas características de Angular que mejoran la experiencia de desarrollo y el rendimiento de las aplicaciones.

