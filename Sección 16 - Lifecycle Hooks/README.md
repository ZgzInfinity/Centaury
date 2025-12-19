# LifecycleHooks

Este proyecto demuestra todos los **lifecycle hooks** de Angular, tanto los tradicionales como los nuevos hooks basados en señales (`afterRender`, `afterNextRender`, `effect`).

## Contenido del Proyecto

### Lifecycle Hooks Demostrados

#### **Hooks Tradicionales** (implementando interfaces)

**HomePageComponent** demuestra todos los hooks del ciclo de vida:

1. **constructor()**: Se ejecuta primero, antes de cualquier hook
2. **ngOnChanges()**: Se ejecuta cuando cambian los inputs (implementa `OnChanges`)
3. **ngOnInit()**: Se ejecuta una vez después de inicializar inputs
4. **ngDoCheck()**: Se ejecuta en cada detección de cambios
5. **ngAfterContentInit()**: Se ejecuta una vez después de inicializar contenido proyectado
6. **ngAfterContentChecked()**: Se ejecuta cada vez que se verifica el contenido proyectado
7. **ngAfterViewInit()**: Se ejecuta una vez después de inicializar la vista
8. **ngAfterViewChecked()**: Se ejecuta cada vez que se verifica la vista
9. **ngOnDestroy()**: Se ejecuta antes de destruir el componente

#### **Nuevos Hooks Basados en Señales**

**afterNextRender():**
```typescript
afterNextRenderEffect = afterNextRender(() => {
  log('afterNextRender', 'Runs once the next time that all components have been rendered to the DOM.');
});
```
- Se ejecuta una vez la próxima vez que todos los componentes se rendericen
- Útil para inicialización que requiere el DOM

**afterRender():**
```typescript
afterRender = afterRender(() => {
  log('afterRender', 'Runs every time all components have been rendered to the DOM.');
});
```
- Se ejecuta cada vez que todos los componentes se rendericen
- Similar a `ngAfterViewChecked` pero más moderno

**effect():**
```typescript
basicEffect = effect((onCleanup) => {
  log('effect', 'Disparar efectos secundarios');
  
  onCleanup(() => {
    log('onCleanup', 'Se ejecuta cuando el efecto se va a destruir');
  });
});
```
- Reacciona a cambios en señales
- Incluye función `onCleanup` para limpieza

### TitleComponent

Demuestra `ngOnChanges` con múltiples inputs:

```typescript
export class TitleComponent implements OnChanges {
  title = input.required<string>();
  title2 = input.required<string>();
  title3 = input.required<string>();
  title4 = input.required<string>();

  ngOnChanges(changes: SimpleChanges) {
    for (const inputName in changes) {
      const inputValues = changes[inputName];
      console.log(`Previous ${inputName} == ${inputValues.previousValue}`);
      console.log(`Current ${inputName} == ${inputValues.currentValue}`);
      console.log(`Is first ${inputName} change == ${inputValues.firstChange}`);
    }
  }
}
```

## Conceptos Aprendidos

### 1. **Orden de Ejecución de Hooks**

1. `constructor`
2. `ngOnChanges` (si hay inputs)
3. `ngOnInit`
4. `ngDoCheck`
5. `ngAfterContentInit`
6. `ngAfterContentChecked`
7. `ngAfterViewInit`
8. `ngAfterViewChecked`
9. `ngOnDestroy` (al destruir)

### 2. **Cuándo Usar Cada Hook**

- **constructor**: Inicialización básica, inyección de dependencias
- **ngOnInit**: Lógica de inicialización que requiere inputs
- **ngAfterViewInit**: Acceso a elementos del DOM, inicialización de librerías
- **ngOnDestroy**: Limpieza de suscripciones, timers, event listeners
- **afterNextRender**: Inicialización que requiere DOM renderizado (moderno)
- **afterRender**: Lógica que debe ejecutarse después de cada render (moderno)
- **effect**: Reacciones a cambios en señales (moderno)

### 3. **SimpleChanges Interface**

```typescript
ngOnChanges(changes: SimpleChanges) {
  // changes contiene:
  // - previousValue: valor anterior
  // - currentValue: valor actual
  // - firstChange: si es el primer cambio
}
```

### 4. **onCleanup en Effects**

```typescript
effect((onCleanup) => {
  // lógica del efecto
  
  onCleanup(() => {
    // limpieza cuando el efecto se destruye
  });
});
```

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve
```

Abre la consola del navegador para ver el orden de ejecución de los hooks cuando interactúas con los componentes.

## Estructura del Proyecto

```
src/app/
├── components/
│   └── title/              # Componente con ngOnChanges
├── pages/
│   ├── home-page/         # Demostración completa de hooks
│   └── about-page/        # Página adicional
└── app.routes.ts
```

Este proyecto es esencial para entender el ciclo de vida de los componentes en Angular y cuándo usar cada hook para diferentes propósitos.
