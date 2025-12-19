# Expansión de Bases de Angular

Este proyecto expande los conceptos básicos de Angular introduciendo servicios, comunicación entre componentes, inyección de dependencias, y nuevas características del template de Angular como control flow (`@for`, `@if`).

## Contenido del Proyecto

### Estructura

El proyecto incluye:
- Componentes reutilizables
- Servicios con inyección de dependencias
- Comunicación padre-hijo con `input()` y `output()`
- Control flow moderno de Angular
- Persistencia en LocalStorage con `effect()`

### Componentes Principales

#### 1. **NavbarComponent** (`components/shared/navbar/`)
- Componente compartido de navegación
- Uso de `RouterLink` y `RouterLinkActive` para navegación
- Estilos inline con clases activas
- Ejemplo de componente reutilizable

#### 2. **DragonballPageComponent** (`pages/dragonball/`)
- **Conceptos clave**: Two-way binding, control flow, class binding
- Gestión de estado local con señales
- Formulario para agregar personajes
- Lista de personajes con filtrado condicional
- Uso de `@for` y `@if` (nuevo control flow de Angular)
- Class binding dinámico basado en el poder del personaje

**Características destacadas:**
```typescript
// Two-way binding manual con señales
[value]="name()"
(input)="name.set(txtName.value)"

// Control flow moderno
@for (character of characters(); track character.id; let idx = $index) {
  @if( character.power > 500 ) {
    // contenido
  }
}

// Class binding condicional
[class.text-danger]="character.power > 9000"
[class.text-primary]="character.power < 9000"
```

#### 3. **DragonballSuperPageComponent** (`pages/dragonball-super/`)
- **Conceptos clave**: Servicios, comunicación entre componentes, inyección de dependencias
- Usa `inject()` para obtener el servicio
- Comunicación entre componentes padre e hijo:
  - `CharacterAddComponent` emite eventos con `output()`
  - `CharacterListComponent` recibe datos con `input()`
- Ejemplo de arquitectura más escalable

#### 4. **CharacterAddComponent** (`components/dragonball/character-add/`)
- Componente hijo para agregar personajes
- Usa `output()` para emitir eventos al padre:
  ```typescript
  newCharacter = output<Character>();
  this.newCharacter.emit(newCharacter);
  ```
- Encapsula la lógica del formulario

#### 5. **CharacterListComponent** (`components/dragonball/character-list/`)
- Componente hijo para mostrar lista de personajes
- Usa `input.required()` para recibir datos del padre:
  ```typescript
  characters = input.required<Character[]>();
  listName = input.required<string>();
  ```
- Componente presentacional (dumb component)

### Servicios

#### **DragonballService** (`services/dragonball.service.ts`)
- **Conceptos clave**: Servicios, señales en servicios, efectos, persistencia
- Servicio singleton con `@Injectable({ providedIn: 'root' })`
- Estado compartido con señal:
  ```typescript
  characters = signal<Character[]>(loadFromLocalStorage());
  ```
- Uso de `effect()` para persistir automáticamente en LocalStorage:
  ```typescript
  saveToLocalStorage = effect(() => {
    localStorage.setItem('characters', JSON.stringify(this.characters()));
  });
  ```
- Métodos para modificar el estado:
  ```typescript
  addCharacter(character: Character) {
    this.characters.update((list) => [...list, character]);
  }
  ```

### Interfaces

#### **Character Interface** (`interfaces/character.interface.ts`)
```typescript
export interface Character {
  id: number;
  name: string;
  power: number;
}
```

## Conceptos Aprendidos

### 1. **Inyección de Dependencias**
```typescript
// Método moderno con inject()
public dragonballService = inject(DragonballService);
```

### 2. **Comunicación entre Componentes**
- **Padre → Hijo**: Usando `input()`
  ```typescript
  characters = input.required<Character[]>();
  ```
- **Hijo → Padre**: Usando `output()`
  ```typescript
  newCharacter = output<Character>();
  this.newCharacter.emit(newCharacter);
  ```

### 3. **Control Flow Moderno de Angular**
Reemplazo de `*ngFor` y `*ngIf`:
```html
@for (character of characters(); track character.id; let idx = $index) {
  @if( character.power > 500 ) {
    <!-- contenido -->
  }
}
```

### 4. **Template Reference Variables**
Uso de `#` para referenciar elementos:
```html
<input #txtName (input)="name.set(txtName.value)" />
```

### 5. **Effects**
Reacciones automáticas a cambios en señales:
```typescript
saveToLocalStorage = effect(() => {
  // Se ejecuta automáticamente cuando characters() cambia
  localStorage.setItem('characters', JSON.stringify(this.characters()));
});
```

### 6. **Routing Avanzado**
- Ruta comodín (`**`) para redirección:
  ```typescript
  { path: '**', redirectTo: '' }
  ```
- `RouterLinkActive` para estilos activos

### 7. **Class Binding Dinámico**
```html
[class.text-danger]="character.power > 9000"
[class.text-primary]="character.power < 9000"
```

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve
```

Navegar a:
- `http://localhost:4200/` - Contador
- `http://localhost:4200/hero` - Hero
- `http://localhost:4200/dragonball` - Dragonball (versión simple)
- `http://localhost:4200/dragonball-super` - Dragonball Super (con servicios y componentes)

## Estructura del Proyecto

```
src/
├── app/
│   ├── components/
│   │   ├── dragonball/
│   │   │   ├── character-add/      # Componente para agregar
│   │   │   └── character-list/      # Componente para listar
│   │   └── shared/
│   │       └── navbar/              # Navbar compartido
│   ├── interfaces/
│   │   └── character.interface.ts   # Interface Character
│   ├── pages/
│   │   ├── counter/                 # Página contador
│   │   ├── hero/                    # Página héroe
│   │   ├── dragonball/              # Página Dragonball simple
│   │   └── dragonball-super/        # Página con servicios
│   ├── services/
│   │   └── dragonball.service.ts    # Servicio con estado compartido
│   ├── app.component.ts
│   ├── app.routes.ts
│   └── app.config.ts
└── main.ts
```

## Diferencias Clave

**DragonballPageComponent vs DragonballSuperPageComponent:**
- **DragonballPageComponent**: Todo el estado y lógica en un solo componente
- **DragonballSuperPageComponent**: Arquitectura modular con:
  - Servicio para estado compartido
  - Componentes reutilizables
  - Comunicación padre-hijo
  - Persistencia automática

Este proyecto demuestra cómo escalar una aplicación Angular usando servicios, componentes reutilizables y una arquitectura más mantenible.
