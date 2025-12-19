# Angular App Países - Básico

Esta versión añade la funcionalidad completa de búsqueda de países usando la API de REST Countries, incluyendo servicios HTTP, mappers, recursos reactivos (`rxResource`), y manejo de errores.

## Contenido del Proyecto

### Funcionalidades Implementadas

- ✅ Búsqueda de países por capital
- ✅ Búsqueda de países por nombre
- ✅ Búsqueda de país por código alpha (detalle)
- ✅ Visualización de información completa de países
- ✅ Mappers para transformar datos de la API
- ✅ Manejo de errores con RxJS
- ✅ Recursos reactivos con `rxResource`

### Componentes y Servicios

#### 1. **CountryService** (`services/country.service.ts`)

Servicio principal que gestiona todas las peticiones HTTP:

**Métodos principales:**

**searchByCapital(query: string):**
```typescript
searchByCapital(query: string): Observable<Country[]> {
  return this.http.get<RESTCountry[]>(`${API_URL}/capital/${query}`).pipe(
    map((resp) => CountryMapper.mapRestCountryArrayToCountryArray(resp)),
    catchError((error) => {
      return throwError(() => new Error(`No se pudo obtener países...`));
    })
  );
}
```

**searchByCountry(query: string):**
- Similar a `searchByCapital` pero busca por nombre
- Incluye `delay(2000)` para simular carga

**searchCountryByAlphaCode(code: string):**
- Busca un país específico por código (ej: "ES", "US")
- Retorna un solo país: `map((countries) => countries.at(0))`

**Características:**
- Manejo de errores con `catchError`
- Transformación de datos con `map`
- Uso de mappers para limpiar datos

#### 2. **ByCapitalPageComponent** (`pages/by-capital-page/`)

**Conceptos clave: `rxResource`**

```typescript
query = signal('');

countryResource = rxResource({
  request: () => ({ query: this.query() }),
  loader: ({ request }) => {
    if (!request.query) return of([]);
    return this.countryService.searchByCapital(request.query);
  },
});
```

**`rxResource` es una API moderna de Angular que:**
- Gestiona automáticamente el estado de carga
- Maneja errores automáticamente
- Se actualiza reactivamente cuando cambia `request()`
- Proporciona `value()`, `isLoading()`, `error()` como señales

**Uso en template:**
```html
@if (countryResource.isLoading()) {
  <p>Cargando...</p>
}

@if (countryResource.error(); as error) {
  <p>Error: {{ error }}</p>
}

@if (countryResource.value(); as countries) {
  <country-list [countries]="countries" />
}
```

#### 3. **CountryPageComponent** (`pages/country-page/`)

Página de detalle de un país:

```typescript
countryCode = inject(ActivatedRoute).snapshot.params['code'];

countryResource = rxResource({
  request: () => ({ code: this.countryCode }),
  loader: ({ request }) => {
    return this.countryService.searchCountryByAlphaCode(request.code);
  },
});
```

**Características:**
- Lee parámetro de ruta con `ActivatedRoute.snapshot`
- Usa `rxResource` para cargar datos
- Muestra componente `CountryInformationComponent` con los datos

#### 4. **CountryMapper** (`mappers/country.mapper.ts`)

Transforma datos de la API al modelo interno:

```typescript
static mapRestCountryToCountry(restCountry: RESTCountry): Country {
  return {
    capital: restCountry.capital.join(','),
    cca2: restCountry.cca2,
    flag: restCountry.flag,
    flagSvg: restCountry.flags.svg,
    name: restCountry.translations['spa'].common ?? 'No Spanish Name',
    population: restCountry.population,
    region: restCountry.region,
    subRegion: restCountry.subregion,
  };
}
```

**Características:**
- Extrae solo los datos necesarios
- Transforma nombres a español
- Normaliza estructura de datos

### Interfaces

#### **Country Interface** (`interfaces/country.interface.ts`)
```typescript
export interface Country {
  cca2: string;
  flag: string;
  flagSvg: string;
  name: string;
  capital: string;
  population: number;
  region: string;
  subRegion: string;
}
```

#### **RESTCountry Interface** (`interfaces/rest-countries.interface.ts`)
Interfaces completas para la respuesta de la API de REST Countries.

### Componentes Adicionales

#### **NotFoundComponent** (`shared/components/not-found/`)
- Componente para mostrar errores 404
- Usado cuando no se encuentra un país

#### **CountryInformationComponent** (`pages/country-page/country-information/`)
- Componente para mostrar información detallada de un país
- Recibe el país como input

## Conceptos Aprendidos

### 1. **rxResource - Recursos Reactivos**
API moderna de Angular para gestionar datos asíncronos:

```typescript
countryResource = rxResource({
  request: () => ({ query: this.query() }),
  loader: ({ request }) => this.service.getData(request.query),
});
```

**Ventajas:**
- Estado automático: `isLoading()`, `error()`, `value()`
- Reactivo: se actualiza cuando cambia `request()`
- Type-safe
- Manejo automático de suscripciones

### 2. **RxJS Operators Avanzados**
- `catchError`: Manejo de errores
- `throwError`: Lanzar errores personalizados
- `delay`: Simular latencia
- `map`: Transformación de datos
- `of`: Crear observable con valor

### 3. **ActivatedRoute - Parámetros de Ruta**
```typescript
// Snapshot (una vez)
inject(ActivatedRoute).snapshot.params['code']

// Observable (reactivo)
inject(ActivatedRoute).params.pipe(map(params => params['code']))
```

### 4. **Mappers Pattern**
Separación de responsabilidades:
- API retorna estructura compleja
- Mapper transforma a modelo interno simple
- Componentes trabajan con modelo limpio

### 5. **Manejo de Errores con RxJS**
```typescript
catchError((error) => {
  console.log('Error fetching ', error);
  return throwError(() => new Error('Mensaje personalizado'));
})
```

### 6. **Optional Chaining y Nullish Coalescing**
```typescript
restCountry.translations['spa'].common ?? 'No Spanish Name'
countries.at(0) // Safe array access
```

## API Utilizada

**REST Countries API**: `https://restcountries.com/v3.1`

Endpoints utilizados:
- `/capital/{capital}` - Buscar por capital
- `/name/{name}` - Buscar por nombre
- `/alpha/{code}` - Buscar por código alpha

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve
```

Navegar a:
- `http://localhost:4200/country/by-capital` - Búsqueda por capital
- `http://localhost:4200/country/by-country` - Búsqueda por nombre
- `http://localhost:4200/country/by/:code` - Detalle de país

## Estructura del Proyecto

```
src/app/country/
├── components/
│   ├── country-list/         # Lista de países
│   ├── search-input/         # Input de búsqueda
│   └── top-menu/             # Menú superior
├── interfaces/
│   ├── country.interface.ts   # Modelo interno
│   └── rest-countries.interface.ts  # Modelos de API
├── mappers/
│   └── country.mapper.ts     # Transformación de datos
├── pages/
│   ├── by-capital-page/      # Búsqueda por capital
│   ├── by-country-page/      # Búsqueda por nombre
│   ├── by-region-page/       # Búsqueda por región (vacío)
│   └── country-page/         # Detalle de país
├── services/
│   └── country.service.ts    # Lógica de negocio
└── layouts/
    └── CountryLayout/         # Layout compartido
```

## Diferencias con la Versión Anterior

**Sección 9 (Inicio)**:
- Solo estructura y layout
- Sin funcionalidad real
- Componentes vacíos

**Sección 10 (Básico)**:
- ✅ Integración completa con API
- ✅ Búsqueda funcional
- ✅ `rxResource` para gestión de estado
- ✅ Mappers para transformación
- ✅ Manejo de errores
- ✅ Página de detalle

Esta versión demuestra cómo usar `rxResource`, una de las APIs más modernas de Angular para gestionar datos asíncronos de forma reactiva y declarativa.
