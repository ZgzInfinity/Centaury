# Bases de Angular

Este proyecto introduce los conceptos fundamentales de Angular, incluyendo componentes, routing, señales (signals) y computed properties.

## Contenido del Proyecto

### Estructura de Componentes

El proyecto está organizado con un componente principal (`AppComponent`) y dos páginas de ejemplo:

#### 1. **AppComponent** (`app.component.ts`)
- Componente raíz de la aplicación
- Configuración básica con `@Component` decorator
- Uso de `RouterOutlet` para navegación
- Template simple con sección y router outlet

#### 2. **CounterPageComponent** (`pages/counter/`)
- **Conceptos clave**: Señales (Signals) y estado reactivo
- Implementa un contador con dos versiones:
  - Contador tradicional con propiedad `counter`
  - Contador con señal: `counterSignal = signal(10)`
- Métodos:
  - `increaseBy(value: number)`: Incrementa/decrementa el contador
    - Usa `signal.update()` para actualizar señales
  - `resetCounter()`: Reinicia el contador a 0
- **Diferencia importante**: Muestra la comparación entre estado tradicional y señales

#### 3. **HeroPageComponent** (`pages/hero/`)
- **Conceptos clave**: Señales computadas (computed signals) y pipes
- Señales reactivas:
  - `name = signal('Ironman')`
  - `age = signal(45)`
- Señales computadas:
  - `heroDescription`: Combina nombre y edad
  - `capitalizedName`: Convierte el nombre a mayúsculas
- Uso de pipes: `UpperCasePipe` de Angular
- Métodos para modificar el estado:
  - `changeHero()`: Cambia nombre y edad
  - `changeAge()`: Solo cambia la edad
  - `resetForm()`: Restaura valores iniciales

### Routing

El proyecto implementa routing básico con Angular Router:

```typescript
export const routes: Routes = [
  { path: '', component: CounterPageComponent },
  { path: 'hero', component: HeroPageComponent },
];
```

- Ruta raíz (`''`) → CounterPageComponent
- Ruta `/hero` → HeroPageComponent

## Conceptos Aprendidos

### 1. **Señales (Signals)**
Las señales son una nueva característica de Angular para manejar estado reactivo:

```typescript
counterSignal = signal(10);           // Crear señal
counterSignal()                       // Leer valor
counterSignal.set(20)                 // Establecer valor
counterSignal.update(current => current + 1)  // Actualizar basado en valor actual
```

### 2. **Señales Computadas (Computed Signals)**
Derivan valores de otras señales y se actualizan automáticamente:

```typescript
heroDescription = computed(() => {
  return `${this.name()} - ${this.age()}`;
});
```

### 3. **Componentes Standalone**
Los componentes son standalone (no necesitan módulos):
- `imports: [RouterOutlet]` - Importación directa de dependencias
- `imports: [UpperCasePipe]` - Importación de pipes

### 4. **Event Binding**
Uso de `(click)` para manejar eventos:
```html
<button (click)="increaseBy(1)">+1</button>
```

### 5. **Interpolación**
Uso de `{{ }}` para mostrar valores en templates:
```html
<h1>Counter: {{ counter }}</h1>
<h1>Counter Signal: {{ counterSignal() }}</h1>
```

**Nota importante**: Las señales se invocan como funciones `()` en los templates.

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve
```

Navegar a `http://localhost:4200/` para ver el contador o `http://localhost:4200/hero` para ver el componente de héroe.

## Estructura del Proyecto

```
src/
├── app/
│   ├── app.component.ts          # Componente raíz
│   ├── app.component.html        # Template raíz
│   ├── app.config.ts             # Configuración de la app
│   ├── app.routes.ts             # Definición de rutas
│   └── pages/
│       ├── counter/
│       │   ├── counter-page.component.ts
│       │   └── counter-page.component.html
│       └── hero/
│           ├── hero-page.component.ts
│           └── hero-page.component.html
├── main.ts                       # Bootstrap de la aplicación
└── styles.css                    # Estilos globales
```

Este proyecto sienta las bases para entender cómo funciona Angular con su nuevo sistema de señales y la arquitectura de componentes standalone.
