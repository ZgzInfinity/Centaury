# Angular App Países - Intermedio Avanzado

Esta versión añade funcionalidades avanzadas como caché de consultas, búsqueda por región, `linkedSignal` para sincronización con query params, y validación de parámetros.

## Contenido del Proyecto

### Nuevas Funcionalidades

- ✅ **Caché de consultas** para optimizar peticiones HTTP
- ✅ **Búsqueda por región** completamente funcional
- ✅ **linkedSignal** para sincronizar con query params
- ✅ **Validación de parámetros** de región
- ✅ **Navegación con query params** para compartir URLs
- ✅ **Type safety** con tipos literales para regiones

### Mejoras Implementadas

#### 1. **CountryService con Caché** (`services/country.service.ts`)

**Sistema de caché implementado:**

```typescript
private queryCacheCapital = new Map<string, Country[]>();
private queryCacheCountry = new Map<string, Country[]>();
private queryCacheRegion = new Map<Region, Country[]>();
```

**Búsqueda con caché:**
```typescript
searchByCapital(query: string): Observable<Country[]> {
  query = query.toLowerCase();
  
  // Verificar caché
  if (this.queryCacheCapital.has(query)) {
    return of(this.queryCacheCapital.get(query) ?? []);
  }
  
  // Si no está en caché, hacer petición
  return this.http.get<RESTCountry[]>(`${API_URL}/capital/${query}`).pipe(
    map((resp) => CountryMapper.mapRestCountryArrayToCountryArray(resp)),
    tap((countries) => this.queryCacheCapital.set(query, countries)), // Guardar en caché
    catchError(...)
  );
}
```

**Características del caché:**
- Evita peticiones HTTP duplicadas
- Respuestas instantáneas para consultas repetidas
- Caché separado por tipo de búsqueda
- Uso de `Map` para búsqueda O(1)

**Nuevo método: searchByRegion**
```typescript
searchByRegion(region: Region): Observable<Country[]> {
  if (this.queryCacheRegion.has(region)) {
    return of(this.queryCacheRegion.get(region) ?? []);
  }
  
  return this.http.get<RESTCountry[]>(`${API_URL}/region/${region}`).pipe(
    map((resp) => CountryMapper.mapRestCountryArrayToCountryArray(resp)),
    tap((countries) => this.queryCacheRegion.set(region, countries)),
    catchError(...)
  );
}
```

#### 2. **Region Type** (`interfaces/region.type.ts`)

Tipo literal para regiones válidas:

```typescript
export type Region =
  | 'Africa'
  | 'Americas'
  | 'Asia'
  | 'Europe'
  | 'Oceania'
  | 'Antarctic';
```

**Ventajas:**
- Type safety: solo acepta regiones válidas
- Autocompletado en IDE
- Previene errores de tipeo

#### 3. **ByRegionPageComponent Avanzado** (`pages/by-region-page/`)

**Conceptos avanzados implementados:**

**linkedSignal - Sincronización con Query Params:**
```typescript
activatedRoute = inject(ActivatedRoute);
queryParam = this.activatedRoute.snapshot.queryParamMap.get('region') ?? '';

selectedRegion = linkedSignal<Region>(() =>
  validateQueryParam(this.queryParam)
);
```

**`linkedSignal`**:
- Crea una señal vinculada a una expresión
- Se recalcula cuando cambian las dependencias
- Útil para sincronizar con valores externos

**Validación de Query Params:**
```typescript
function validateQueryParam(queryParam: string): Region {
  queryParam = queryParam.toLowerCase();
  
  const validRegions: Record<string, Region> = {
    africa: 'Africa',
    americas: 'Americas',
    asia: 'Asia',
    europe: 'Europe',
    oceania: 'Oceania',
    antarctic: 'Antarctic',
  };
  
  return validRegions[queryParam] ?? 'Americas'; // Valor por defecto
}
```

**Navegación con Query Params:**
```typescript
countryResource = rxResource({
  request: () => ({ region: this.selectedRegion() }),
  loader: ({ request }) => {
    if (!request.region) return of([]);
    
    // Actualizar URL con query params
    this.router.navigate(['/country/by-region'], {
      queryParams: { region: request.region },
    });
    
    return this.countryService.searchByRegion(request.region);
  },
});
```

**Características:**
- URL compartible: `/country/by-region?region=Europe`
- Sincronización bidireccional: URL ↔ Estado
- Validación automática de parámetros

#### 4. **ByCapitalPageComponent Mejorado**

**linkedSignal con Query Params:**
```typescript
activatedRoute = inject(ActivatedRoute);
router = inject(Router);

queryParam = this.activatedRoute.snapshot.queryParamMap.get('query') ?? '';

query = linkedSignal(() => this.queryParam);

countryResource = rxResource({
  request: () => ({ query: this.query() }),
  loader: ({ request }) => {
    if (!request.query) return of([]);
    
    // Actualizar URL
    this.router.navigate(['/country/by-capital'], {
      queryParams: { query: request.query },
    });
    
    return this.countryService.searchByCapital(request.query);
  },
});
```

**Mejoras:**
- URLs compartibles
- Estado sincronizado con URL
- Navegación del navegador funciona correctamente

## Conceptos Aprendidos

### 1. **Caché de Consultas**
Patrón para optimizar peticiones HTTP:
- Almacenar resultados en memoria
- Verificar antes de hacer petición
- Reducir carga en servidor
- Mejorar tiempo de respuesta

### 2. **linkedSignal()**
Crea una señal vinculada a una expresión:

```typescript
query = linkedSignal(() => this.queryParam);
```

**Características:**
- Se recalcula cuando cambian dependencias
- Útil para valores derivados
- Sincronización con valores externos

### 3. **Query Params en Angular Router**
```typescript
// Leer
activatedRoute.snapshot.queryParamMap.get('region')

// Escribir
router.navigate(['/path'], {
  queryParams: { region: 'Europe' }
});
```

**Ventajas:**
- URLs compartibles
- Estado en la URL
- Navegación del navegador funciona

### 4. **Validación de Parámetros**
Función para validar y normalizar parámetros:
- Convierte a minúsculas
- Mapea a valores válidos
- Proporciona valor por defecto

### 5. **Record Type en TypeScript**
```typescript
const validRegions: Record<string, Region> = {
  africa: 'Africa',
  // ...
};
```

Tipo para objetos con claves string.

### 6. **Map vs Object para Caché**
```typescript
// Map es mejor para caché
private cache = new Map<string, Country[]>();

// Ventajas:
// - Búsqueda O(1)
// - Claves pueden ser cualquier tipo
// - Mejor rendimiento para muchas entradas
```

### 7. **RxJS tap() Operator**
Efectos secundarios sin modificar el stream:
```typescript
tap((countries) => this.queryCacheCapital.set(query, countries))
```

### 8. **of() para Observables Síncronos**
```typescript
if (cache.has(key)) {
  return of(cache.get(key) ?? []); // Observable inmediato
}
```

## Diferencias con Versiones Anteriores

**Sección 10 Básico**:
- Sin caché (siempre hace peticiones HTTP)
- Búsqueda por región no implementada
- Sin query params en URL
- Sin validación de parámetros

**Sección 10 Intermedio Avanzado**:
- ✅ Caché completo para todas las búsquedas
- ✅ Búsqueda por región funcional
- ✅ Query params en URLs
- ✅ linkedSignal para sincronización
- ✅ Validación de parámetros
- ✅ URLs compartibles
- ✅ Mejor rendimiento

## Desarrollo

Para iniciar el servidor de desarrollo:

```bash
ng serve
```

Navegar a:
- `http://localhost:4200/country/by-capital?query=Madrid`
- `http://localhost:4200/country/by-region?region=Europe`
- `http://localhost:4200/country/by/:code`

**Nota**: Las URLs con query params son compartibles y el estado se mantiene al recargar.

## Estructura del Proyecto

```
src/app/country/
├── interfaces/
│   ├── country.interface.ts
│   ├── region.type.ts          # ✨ Nuevo: Tipo para regiones
│   └── rest-countries.interface.ts
├── services/
│   └── country.service.ts      # ✨ Mejorado: Con caché
└── pages/
    ├── by-capital-page/        # ✨ Mejorado: Con query params
    ├── by-region-page/          # ✨ Nuevo: Funcional completo
    └── ...
```

Esta versión demuestra técnicas avanzadas de Angular para optimizar rendimiento (caché), mejorar UX (URLs compartibles), y mantener sincronización entre estado y URL usando `linkedSignal` y query params.
