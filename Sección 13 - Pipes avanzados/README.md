# PipesApp - Pipes Avanzados

Esta versión extiende la aplicación de pipes con **pipes personalizados** que demuestran cómo crear transformaciones de datos específicas para tu aplicación, incluyendo filtrado, ordenamiento, y transformaciones complejas.

## Contenido del Proyecto

### Pipes Personalizados Implementados

#### 1. **ToggleCasePipe** (`pipes/toggle-case.pipe.ts`)
Transforma texto a mayúsculas o minúsculas según un parámetro:

```typescript
@Pipe({ name: 'toggleCase' })
export class ToggleCasePipe implements PipeTransform {
  transform(value: string, upper: boolean = true): string {
    return upper ? value.toUpperCase() : value.toLowerCase();
  }
}
```

**Uso:**
```html
{{ name() | toggleCase : upperCase() }}
```

#### 2. **HeroFilterPipe** (`pipes/hero-filter.pipe.ts`)
Filtra un array de héroes por nombre:

```typescript
@Pipe({ name: 'heroFilter' })
export class HeroFilterPipe implements PipeTransform {
  transform(value: Hero[], search: string): Hero[] {
    if (!search) return value;
    search = search.toLowerCase();
    return value.filter((hero) => hero.name.toLowerCase().includes(search));
  }
}
```

**Uso:**
```html
@for (hero of heroes() | heroFilter:searchQuery(); track hero.id) {
  <!-- contenido -->
}
```

#### 3. **HeroSortByPipe** (`pipes/hero-sort-by.pipe.ts`)
Ordena héroes por diferentes propiedades:

```typescript
@Pipe({ name: 'heroSortBy' })
export class HeroSortByPipe implements PipeTransform {
  transform(value: Hero[], sortBy: keyof Hero | null): Hero[] {
    if (!sortBy) return value;
    
    switch (sortBy) {
      case 'name':
        return value.sort((a, b) => a.name.localeCompare(b.name));
      case 'canFly':
        return value.sort((a, b) => (a.canFly ? 1 : -1) - (b.canFly ? 1 : -1));
      case 'color':
        return value.sort((a, b) => a.color - b.color);
      case 'creator':
        return value.sort((a, b) => a.creator - b.creator);
      default:
        return value;
    }
  }
}
```

**Uso:**
```html
@for (hero of heroes() | heroFilter:searchQuery() | heroSortBy:sortBy(); track hero.id) {
  <!-- contenido -->
}
```

**Nota**: Múltiples pipes encadenados para filtrado y ordenamiento.

#### 4. **CanFlyPipe** (`pipes/can-fly.pipe.ts`)
Transforma boolean a texto legible:

```typescript
@Pipe({ name: 'canFly' })
export class CanFyPipe implements PipeTransform {
  transform(value: boolean): 'Puede volar' | 'No puede volar' {
    return value ? 'Puede volar' : 'No puede volar';
  }
}
```

#### 5. **HeroColorPipe** (`pipes/heroColor.pipe.ts`)
Convierte enum de color a string:

```typescript
@Pipe({ name: 'heroColor' })
export class HeroColorPipe implements PipeTransform {
  transform(value: Color): string {
    return Color[value];
  }
}
```

#### 6. **HeroTextColorPipe** (`pipes/hero-text-color.pipe.ts`)
Retorna color CSS basado en el color del héroe (para estilos).

#### 7. **HeroCreatorPipe** (`pipes/hero-creator.pipe.ts`)
Transforma ID de creador a nombre legible.

### CustomPageComponent

Página completa que demuestra todos los pipes personalizados:

**Características:**
- Búsqueda en tiempo real con `HeroFilterPipe`
- Ordenamiento dinámico con `HeroSortByPipe`
- Transformaciones visuales con pipes de color y texto
- Tabla interactiva con filtrado y ordenamiento

**Implementación:**
```typescript
export default class CustomPageComponent {
  name = signal('Fernando Herrera');
  upperCase = signal(true);
  heroes = signal(heroes);
  sortBy = signal<keyof Hero | null>(null);
  searchQuery = signal('');
}
```

**Template:**
```html
<!-- Filtrado y ordenamiento combinados -->
@for (hero of heroes() | heroFilter:searchQuery() | heroSortBy:sortBy(); track hero.id) {
  <tr>
    <td>{{ hero.name }}</td>
    <td>{{ hero.canFly | canFly }}</td>
    <td [style.color]="hero.color | heroTextColor">
      {{ hero.color | heroColor | titlecase }}
    </td>
    <td>{{ hero.creator | heroCreator }}</td>
  </tr>
}
```

## Conceptos Aprendidos

### 1. **Crear Pipes Personalizados**

**Estructura básica:**
```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'myPipe',  // Nombre para usar en templates
  standalone: true  // Si es standalone
})
export class MyPipe implements PipeTransform {
  transform(value: any, ...args: any[]): any {
    // Lógica de transformación
    return transformedValue;
  }
}
```

### 2. **Pipes Puros vs Impuros**

**Pipes puros (por defecto):**
- Se ejecutan solo cuando cambia la referencia del valor de entrada
- Más eficientes
- Ejemplo: `transform(value: string)`

**Pipes impuros:**
- Se ejecutan en cada ciclo de detección de cambios
- Menos eficientes
- Se marca con `pure: false`
```typescript
@Pipe({
  name: 'myPipe',
  pure: false  // Pipe impuro
})
```

### 3. **Múltiples Parámetros**
```typescript
transform(value: string, param1: string, param2: number): string {
  // ...
}
```
```html
{{ value | myPipe : param1 : param2 }}
```

### 4. **Chaining de Pipes**
```html
{{ heroes() | heroFilter:searchQuery() | heroSortBy:sortBy() }}
```
Los pipes se ejecutan de izquierda a derecha.

### 5. **Type Safety en Pipes**
```typescript
transform(value: Hero[], sortBy: keyof Hero | null): Hero[]
```
Usar tipos específicos para mejor autocompletado y detección de errores.

### 6. **Pipes para Filtrado y Ordenamiento**

**Consideraciones:**
- Los pipes puros no se ejecutan cuando cambia el array interno
- Para filtrado/ordenamiento dinámico, considera usar computed signals o métodos
- Los pipes son mejores para transformaciones simples

**Alternativa con computed:**
```typescript
filteredHeroes = computed(() => {
  return this.heroes().filter(hero => 
    hero.name.toLowerCase().includes(this.searchQuery().toLowerCase())
  );
});
```

### 7. **Pipes para Transformación de Datos**
- Enums a strings
- Booleans a texto legible
- IDs a nombres
- Formateo personalizado

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve
```

Navegar a `http://localhost:4200/custom` para ver todos los pipes personalizados en acción.

## Estructura del Proyecto

```
src/app/
├── pipes/
│   ├── toggle-case.pipe.ts      # Toggle mayúsculas/minúsculas
│   ├── hero-filter.pipe.ts      # Filtrado de héroes
│   ├── hero-sort-by.pipe.ts     # Ordenamiento de héroes
│   ├── can-fly.pipe.ts          # Boolean a texto
│   ├── heroColor.pipe.ts        # Enum a string
│   ├── hero-text-color.pipe.ts  # Color CSS
│   └── hero-creator.pipe.ts     # ID a nombre
├── pages/
│   └── custom-page/             # Demostración completa
└── interfaces/
    └── hero.interface.ts        # Interface de Hero
```

## Mejores Prácticas

1. **Pipes puros cuando sea posible**: Mejor rendimiento
2. **Type safety**: Usar tipos específicos
3. **Pipes simples**: Para transformaciones, no lógica compleja
4. **Considerar computed signals**: Para filtrado/ordenamiento complejo
5. **Reutilizables**: Crear pipes que puedan usarse en múltiples lugares

## Diferencias con la Versión Anterior

**Sección 12 (Pipes Básicos)**:
- Solo pipes incluidos en Angular
- CustomPageComponent vacío

**Sección 13 (Pipes Avanzados)**:
- ✅ 7 pipes personalizados implementados
- ✅ Filtrado y ordenamiento funcional
- ✅ Transformaciones complejas
- ✅ Ejemplo completo e interactivo

Esta versión demuestra cómo extender Angular con pipes personalizados para necesidades específicas de tu aplicación, manteniendo la lógica de transformación fuera de los componentes.
