# ReactiveFormsApp - Formularios Reactivos 1

Este proyecto introduce los **formularios reactivos** de Angular, una forma poderosa y escalable de manejar formularios con validación, FormGroups, FormArrays, y utilidades para manejo de errores.

## Contenido del Proyecto

### Páginas de Ejemplo

#### 1. **BasicPageComponent** (`reactive/pages/basic-page/`)
Demuestra formularios reactivos básicos con validación:

**FormGroup con FormBuilder:**
```typescript
myForm: FormGroup = this.fb.group({
  name: ['', [Validators.required, Validators.minLength(3)]],
  price: [0, [Validators.required, Validators.min(10)]],
  inStorage: [0, [Validators.required, Validators.min(0)]],
});
```

**Validadores:**
- `Validators.required`: Campo obligatorio
- `Validators.minLength(3)`: Longitud mínima
- `Validators.min(10)`: Valor mínimo

**Validación en template:**
```html
<input formControlName="name" />

@if (formUtils.isValidField(myForm, 'name')) {
  <span class="form-text text-danger">
    {{ formUtils.getFieldError(myForm, "name") }}
  </span>
}
```

**Envío del formulario:**
```typescript
onSave() {
  if (this.myForm.invalid) {
    this.myForm.markAllAsTouched(); // Marcar todos como tocados para mostrar errores
    return;
  }
  
  console.log(this.myForm.value);
  this.myForm.reset({ price: 0, inStorage: 0 });
}
```

#### 2. **DynamicPageComponent** (`reactive/pages/dynamic-page/`)
Demuestra **FormArray** para campos dinámicos:

**FormArray de juegos favoritos:**
```typescript
myForm: FormGroup = this.fb.group({
  name: ['', [Validators.required, Validators.minLength(3)]],
  favoriteGames: this.fb.array(
    [
      ['Metal Gear', Validators.required],
      ['Death Stranding', Validators.required],
    ],
    Validators.minLength(2) // Validación a nivel de array
  ),
});
```

**Getter para acceder al FormArray:**
```typescript
get favoriteGames() {
  return this.myForm.get('favoriteGames') as FormArray;
}
```

**Agregar elementos dinámicamente:**
```typescript
onAddToFavorites() {
  if (this.newFavorite.invalid) return;
  
  const newGame = this.newFavorite.value;
  this.favoriteGames.push(this.fb.control(newGame, Validators.required));
  this.newFavorite.reset();
}
```

**Eliminar elementos:**
```typescript
onDeleteFavorite(index: number) {
  this.favoriteGames.removeAt(index);
}
```

**Template:**
```html
@for (game of favoriteGames.controls; track $index) {
  <div class="input-group">
    <input [formControl]="game" />
    <button (click)="onDeleteFavorite($index)">Eliminar</button>
  </div>
}
```

#### 3. **SwitchesPageComponent** (`reactive/pages/switches-page/`)
Demuestra checkboxes y switches con FormGroup:

**FormGroup con booleanos:**
```typescript
myForm: FormGroup = this.fb.group({
  terms: [false, Validators.requiredTrue], // Debe ser true
  conditions: [false, Validators.requiredTrue],
});
```

**Validators.requiredTrue**: Específico para checkboxes que deben estar marcados.

### FormUtils - Utilidades para Formularios

Clase estática con métodos helper (`utils/form-utils.ts`):

**isValidField:**
```typescript
static isValidField(form: FormGroup, fieldName: string): boolean | null {
  return (
    !!form.controls[fieldName].errors && 
    form.controls[fieldName].touched
  );
}
```

**getFieldError:**
```typescript
static getFieldError(form: FormGroup, fieldName: string): string | null {
  if (!form.controls[fieldName]) return null;
  
  const errors = form.controls[fieldName].errors ?? {};
  return FormUtils.getTextError(errors);
}
```

**getTextError:**
```typescript
static getTextError(errors: ValidationErrors) {
  for (const key of Object.keys(errors)) {
    switch (key) {
      case 'required':
        return 'Este campo es requerido';
      case 'minlength':
        return `Mínimo de ${errors['minlength'].requiredLength} caracteres.`;
      case 'min':
        return `Valor mínimo de ${errors['min'].min}`;
    }
  }
  return null;
}
```

**Para FormArrays:**
```typescript
static isValidFieldInArray(formArray: FormArray, index: number) {
  return (
    formArray.controls[index].errors && 
    formArray.controls[index].touched
  );
}
```

## Conceptos Aprendidos

### 1. **FormBuilder vs FormGroup Manual**

**Con FormBuilder (recomendado):**
```typescript
myForm = this.fb.group({
  name: ['', Validators.required],
});
```

**Manual:**
```typescript
myForm = new FormGroup({
  name: new FormControl('', Validators.required),
});
```

### 2. **FormControl**
Control individual de formulario:
```typescript
newFavorite = new FormControl('', Validators.required);
```
```html
<input [formControl]="newFavorite" />
```

### 3. **FormGroup**
Grupo de controles:
```typescript
myForm: FormGroup = this.fb.group({...});
```
```html
<form [formGroup]="myForm">
  <input formControlName="name" />
</form>
```

### 4. **FormArray**
Array de controles dinámicos:
```typescript
favoriteGames: FormArray = this.fb.array([...]);
```
```html
@for (game of favoriteGames.controls; track $index) {
  <input [formControl]="game" />
}
```

### 5. **Validadores**

**Validadores síncronos:**
- `Validators.required`
- `Validators.minLength(n)`
- `Validators.maxLength(n)`
- `Validators.min(n)`
- `Validators.max(n)`
- `Validators.email`
- `Validators.pattern(regex)`
- `Validators.requiredTrue` (para checkboxes)

**Múltiples validadores:**
```typescript
name: ['', [Validators.required, Validators.minLength(3)]]
```

### 6. **Estados del Formulario**

- `form.valid` / `form.invalid`
- `form.touched` / `form.untouched`
- `form.dirty` / `form.pristine`
- `form.pending` (validación asíncrona en curso)

**Para controles individuales:**
- `control.errors`
- `control.touched`
- `control.dirty`

### 7. **Métodos Útiles**

- `form.markAllAsTouched()`: Marcar todos como tocados
- `form.reset()`: Resetear formulario
- `form.patchValue({...})`: Actualizar valores parciales
- `form.setValue({...})`: Establecer todos los valores
- `formArray.push(control)`: Agregar control
- `formArray.removeAt(index)`: Eliminar control

### 8. **ReactiveFormsModule**
Debe importarse en el componente:
```typescript
imports: [ReactiveFormsModule]
```

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve
```

Navegar a:
- `http://localhost:4200/reactive/basic` - Formularios básicos
- `http://localhost:4200/reactive/dynamic` - Formularios dinámicos
- `http://localhost:4200/reactive/switches` - Checkboxes y switches

## Estructura del Proyecto

```
src/app/
├── reactive/
│   ├── pages/
│   │   ├── basic-page/        # Formularios básicos
│   │   ├── dynamic-page/      # FormArrays dinámicos
│   │   └── switches-page/     # Checkboxes
│   └── reactive.routes.ts    # Rutas del módulo
├── utils/
│   └── form-utils.ts         # Utilidades para formularios
└── app.routes.ts
```

## Ventajas de Formularios Reactivos

1. **Type-safe**: Mejor soporte de TypeScript
2. **Testeable**: Fácil de probar unitariamente
3. **Escalable**: Mejor para formularios complejos
4. **Programático**: Control total desde el componente
5. **Validación avanzada**: Validadores personalizados y asíncronos
6. **Dinámico**: Fácil agregar/eliminar campos

## Mejores Prácticas

1. **Usar FormBuilder**: Más limpio y legible
2. **Validación en el modelo**: No solo en template
3. **Utilidades reutilizables**: Como FormUtils
4. **Mensajes de error claros**: Mejor UX
5. **Validación temprana**: Marcar como touched solo después de interacción
6. **Reset apropiado**: Limpiar formulario después de envío exitoso

Este proyecto sienta las bases para trabajar con formularios reactivos en Angular, proporcionando una alternativa más poderosa y escalable que los formularios basados en plantillas.
