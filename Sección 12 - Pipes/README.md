# PipesApp

Este proyecto demuestra el uso de los pipes básicos incluidos en Angular, incluyendo pipes de texto, números, fechas, y pipes menos comunes como i18nSelect, i18nPlural, Slice, Json, KeyValue y Async.

## Contenido del Proyecto

### Páginas de Ejemplo

#### 1. **BasicPageComponent** (`pages/basic-page/`)
Demuestra los pipes básicos de transformación de texto y fechas:

**Pipes de Texto:**
- `lowercase`: Convierte texto a minúsculas
- `uppercase`: Convierte texto a mayúsculas
- `titlecase`: Convierte a formato título (primera letra de cada palabra en mayúscula)

**Pipes de Fecha:**
- `date`: Formato básico de fecha
- `date: "long"`: Formato largo
- `date: "short"`: Formato corto
- `date: "yyyy MM"`: Formato personalizado
- Con zona horaria: `date: "long": "GMT-6"`

**Ejemplos:**
```html
{{ nameUpper() | lowercase }}
{{ nameLower() | uppercase }}
{{ fullName() | titlecase }}
{{ customDate() | date }}
{{ customDate() | date : "long" : "GMT-6" }}
```

#### 2. **NumbersPageComponent** (`pages/numbers-page/`)
Demuestra pipes numéricos y de moneda:

**Number Pipe:**
- `number: "1.2-2"`: Mínimo 1 dígito entero, 2-2 decimales
- `number: "1.1-2"`: Mínimo 1 dígito entero, 1-2 decimales

**Currency Pipe:**
- `currency: "CAD"`: Moneda canadiense
- `currency: "CAD": "symbol-narrow"`: Símbolo corto
- `currency: "CAD": "symbol-narrow": "1.4-4"`: Con formato de decimales

**Percent Pipe:**
- `percent: "1.2-2"`: Formato de porcentaje

**Ejemplos:**
```html
{{ totalSells() | number : "1.2-2" }}
{{ totalSells() | currency : "CAD" : "symbol-narrow" : "1.4-4" }}
{{ percent() | percent : "1.2-2" }}
```

#### 3. **UncommonPageComponent** (`pages/uncommon-page/`)
Demuestra pipes menos comunes pero muy útiles:

**i18nSelectPipe:**
- Selección de texto basado en un valor
```html
{{ client().gender | i18nSelect : invitationMap }}
```
```typescript
invitationMap = {
  male: 'invitarlo',
  female: 'invitarla',
};
```

**i18nPluralPipe:**
- Pluralización inteligente
```html
{{ clients().length | i18nPlural : clientsMap() }}
```
```typescript
clientsMap = {
  '=0': 'no tenemos ningún cliente esperando',
  '=1': 'tenemos un cliente esperando',
  '=2': 'tenemos 2 clientes esperando',
  other: 'tenemos # clientes esperando',
};
```

**SlicePipe:**
- Extraer porciones de arrays o strings
```html
{{ clients() | slice : 0 : 2 }}  <!-- Primeros 2 -->
{{ clients() | slice : 1 : 3 }}  <!-- Del índice 1 al 3 -->
{{ clients() | slice : 0 : -3 }} <!-- Todos excepto los últimos 3 -->
```

**JsonPipe:**
- Convertir objetos a JSON (útil para debugging)
```html
{{ clients() | json }}
{{ client() | json }}
```

**KeyValuePipe:**
- Iterar sobre propiedades de objetos
```html
@for (item of profile | keyvalue; track item.key) {
  <b>{{ item.key }}:</b> {{ item.value }}
}
```

**AsyncPipe:**
- Suscribirse automáticamente a Observables y Promesas
```html
{{ promiseValue | async }}
{{ myObservableTimer | async }}
```
```typescript
promiseValue: Promise<string> = new Promise((resolve) => {
  setTimeout(() => resolve('Tenemos data'), 3500);
});

myObservableTimer = interval(2000).pipe(
  map((value) => value + 1)
);
```

#### 4. **CustomPageComponent** (`pages/custom-page/`)
- Página preparada para pipes personalizados (vacía en esta versión)
- Se implementará en la siguiente sección

## Conceptos Aprendidos

### 1. **Pipes Básicos de Angular**
- Transformación de datos en templates
- Sintaxis: `{{ valor | pipe }}`
- Pipes con parámetros: `{{ valor | pipe : param1 : param2 }}`
- Chaining de pipes: `{{ valor | pipe1 | pipe2 }}`

### 2. **Formato de Números**
- `number: "minIntegerDigits.minFractionDigits-maxFractionDigits"`
- Ejemplo: `"1.2-2"` = mínimo 1 entero, 2-2 decimales

### 3. **Formato de Fechas**
- Formatos predefinidos: `short`, `medium`, `long`, `full`
- Formatos personalizados: `yyyy MM dd`
- Zonas horarias: `"GMT-6"`, `"GMT-4"`

### 4. **Internacionalización (i18n)**
- `i18nSelect`: Selección de texto según valor
- `i18nPlural`: Pluralización según cantidad
- Útiles para aplicaciones multiidioma

### 5. **AsyncPipe**
- Suscripción automática a Observables/Promesas
- Desuscripción automática al destruir componente
- Manejo de errores
- **Mejor práctica**: Usar AsyncPipe en lugar de `.subscribe()` manual

### 6. **SlicePipe**
- Similar a `Array.slice()` y `String.slice()`
- Útil para paginación y límites de visualización

### 7. **JsonPipe**
- Debugging de objetos
- **Advertencia**: No usar en producción para datos sensibles

### 8. **KeyValuePipe**
- Iterar sobre objetos como si fueran arrays
- Ordenamiento automático de claves

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve
```

Navegar a:
- `http://localhost:4200/basic` - Pipes básicos
- `http://localhost:4200/numbers` - Pipes numéricos
- `http://localhost:4200/uncommon` - Pipes no comunes
- `http://localhost:4200/custom` - Pipes personalizados (vacío)

## Estructura del Proyecto

```
src/app/
├── components/
│   ├── card/              # Componente de tarjeta reutilizable
│   └── navbar/            # Barra de navegación
├── pages/
│   ├── basic-page/        # Pipes básicos
│   ├── numbers-page/      # Pipes numéricos
│   ├── uncommon-page/     # Pipes no comunes
│   └── custom-page/       # Pipes personalizados (vacío)
└── app.routes.ts          # Rutas con lazy loading
```

## Notas Importantes

1. **Pipes puros vs impuros**: La mayoría de pipes son puros (solo se ejecutan cuando cambia el valor de entrada)
2. **Performance**: Los pipes se ejecutan en cada ciclo de detección de cambios, evita lógica pesada
3. **AsyncPipe**: Siempre preferir sobre suscripciones manuales
4. **JsonPipe**: Solo para debugging, no para producción

Este proyecto sienta las bases para entender cómo Angular transforma y presenta datos en los templates usando pipes.
