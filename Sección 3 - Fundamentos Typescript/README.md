# Ejercicios Introductorios a TypeScript

Este proyecto contiene ejercicios prácticos para aprender los fundamentos de TypeScript, que son esenciales antes de comenzar con Angular.

## Contenido del Proyecto

El proyecto está estructurado en 11 temas diferentes, cada uno cubriendo conceptos clave de TypeScript:

### 1. **01-basic-types.ts** - Tipos Básicos
- Tipos primitivos: `string`, `number`, `boolean`
- Union types: `number | 'FULL'`
- Ejemplo con variables tipadas y valores literales

### 2. **02-object-interface.ts** - Interfaces y Objetos
- Definición de interfaces (`Character`)
- Propiedades opcionales (`hometown?`)
- Trabajo con objetos tipados

### 3. **03-functions.ts** - Funciones
- Funciones con tipos de parámetros y retorno
- Funciones flecha con tipos
- Parámetros opcionales y valores por defecto
- Funciones como propiedades en interfaces
- Uso de `this` en métodos

### 4. **04-homework-types.ts** - Ejercicio Práctico
- Interfaces anidadas (`SuperHero` con `Address`)
- Métodos en interfaces
- Acceso a propiedades anidadas

### 5. **05-basic-destructuring.ts** - Desestructuración Básica
- Desestructuración de objetos
- Renombrado de variables en desestructuración
- Desestructuración anidada
- Desestructuración de arrays con valores por defecto

### 6. **06-function-destructuring.ts** - Desestructuración en Funciones
- Desestructuración de parámetros de función
- Exportación de interfaces y funciones
- Uso de desestructuración en callbacks (`forEach`)

### 7. **07-import-export.ts** - Módulos
- Importación y exportación de funciones e interfaces
- Uso de módulos ES6 en TypeScript
- Ejemplo práctico de cálculo de impuestos

### 8. **08-classes.ts** - Clases
- Definición de clases con modificadores de acceso (`public`, `private`)
- Constructores con parámetros
- Valores por defecto en constructores
- Composición vs herencia
- Ejemplo con `Person` y `Hero`

### 9. **09-generics.ts** - Genéricos
- Funciones genéricas con `<T>`
- Type safety con genéricos
- Ejemplos con diferentes tipos: `string`, `number`, `number[]`

### 10. **10-decorators.ts** - Decoradores
- Decoradores de clase
- Modificación de clases en tiempo de ejecución
- Ejemplo de class decorator que extiende funcionalidad

### 11. **11-optional-chaining.ts** - Optional Chaining
- Operador de encadenamiento opcional (`?.`)
- Valores por defecto con `||`
- Non-null assertion operator (`!`)
- Manejo seguro de propiedades opcionales

## Cómo Ejecutar

1. Clonar o descargar el proyecto
2. Ejecutar `npm install` en la carpeta del proyecto
3. En el archivo `main.ts`, descomentar la línea del ejercicio que quieres ejecutar:
   ```typescript
   // import './topics/01-basic-types';
   import './topics/01-basic-types'; // Descomentar esta línea
   ```
4. Ejecutar `npm run dev` para ejecutar el programa

## Estructura del Proyecto

```
src/
├── main.ts              # Punto de entrada - aquí se importan los ejercicios
├── style.css            # Estilos básicos
└── topics/              # Carpeta con todos los ejercicios
    ├── 01-basic-types.ts
    ├── 02-object-interface.ts
    ├── 03-functions.ts
    ├── 04-homework-types.ts
    ├── 05-basic-destructuring.ts
    ├── 06-function-destructuring.ts
    ├── 07-import-export.ts
    ├── 08-classes.ts
    ├── 09-generics.ts
    ├── 10-decorators.ts
    └── 11-optional-chaining.ts
```

## Conceptos Clave Aprendidos

- **Tipado estático**: TypeScript añade tipos a JavaScript
- **Interfaces**: Contratos que definen la estructura de objetos
- **Desestructuración**: Extracción de valores de objetos y arrays
- **Módulos**: Organización del código en archivos separados
- **Clases**: Programación orientada a objetos
- **Genéricos**: Reutilización de código con diferentes tipos
- **Decoradores**: Metaprogramación y modificación de clases
- **Optional Chaining**: Acceso seguro a propiedades opcionales

Estos conceptos son fundamentales para trabajar eficientemente con Angular, ya que Angular está construido con TypeScript y utiliza muchos de estos patrones.
