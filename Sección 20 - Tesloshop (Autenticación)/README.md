# Tesloshop - Con Autenticación

Extensión de Tesloshop que añade **autenticación completa** con JWT, guards, interceptors, y gestión de sesión de usuario.

## Mejoras Implementadas

### Backend
- Endpoints de autenticación (login, register)
- JWT tokens
- Guards para proteger rutas
- Validación de usuarios

### Frontend
- Páginas de login y registro
- Guards de Angular (`CanActivate`)
- Interceptors para añadir tokens a peticiones
- Servicio de autenticación
- Gestión de sesión (localStorage/sessionStorage)
- Protección de rutas

## Conceptos Clave

- **JWT (JSON Web Tokens)**: Autenticación stateless
- **Guards**: Protección de rutas
- **Interceptors**: Modificación de peticiones HTTP
- **AuthService**: Gestión centralizada de autenticación

## Desarrollo

Similar a la Sección 18, pero con sistema completo de autenticación implementado.

