# 04 – API Especificaciones

## 1. Principios de la API

- La API será RESTFULL.
- Todas las respuestas serán en formato JSON.
- Todas las rutas protegidas requerirán autenticación JWT.
- Los recursos siempre estarán asociados al usuario autenticado.
- Los UUID serán utilizados como identificadores públicos.
- Los errores tendrán estructura consistente.

Base URL (local):
/api/v1

---

## 2. Autenticación

### 2.1 Registro de Usuario

POST /api/v1/auth/register

Body:

{
  "email": "user@email.com",
  "password": "string"
}

Response 201:

{
  "id": "uuid",
  "email": "user@email.com",
  "created_at": "timestamp"
}

---

### 2.2 Login

POST /api/v1/auth/login

Body:

{
  "email": "user@email.com",
  "password": "string"
}

Response 200:

{
  "access_token": "jwt_token",
  "expires_in": 3600
}

---

## 3. Projects

### 3.1 Crear Proyecto

POST /api/v1/projects

Headers:
Authorization: Bearer <token>

Body:

{
  "name": "Proyecto A",
  "description": "Opcional",
  "color": "#FF5733"
}

Response 201:

{
  "id": "uuid",
  "name": "Proyecto A",
  "description": "Opcional",
  "color": "#FF5733",
  "created_at": "timestamp",
  "updated_at": "timestamp"
}

---

### 3.2 Obtener Proyectos del Usuario

GET /api/v1/projects

Response 200:

[
  {
    "id": "uuid",
    "name": "Proyecto A",
    "description": "Opcional",
    "color": "#FF5733",
    "created_at": "timestamp",
    "updated_at": "timestamp"
  }
]

---

### 3.3 Actualizar Proyecto

PATCH /api/v1/projects/{project_id}

Body (parcial):

{
  "name": "Nuevo nombre"
}

Response 200:
Proyecto actualizado.

---

### 3.4 Eliminar Proyecto

DELETE /api/v1/projects/{project_id}

Response 204:
Sin contenido.

(Borrado físico)

---

## 4. WorkSessions

### 4.1 Crear Sesión

POST /api/v1/work-sessions

Body:

{
  "project_id": "uuid",
  "date": "YYYY-MM-DD",
  "start_time": "HH:MM",
  "end_time": "HH:MM",
  "notes": "Opcional"
}

Response 201:

{
  "id": "uuid",
  "project_id": "uuid",
  "date": "YYYY-MM-DD",
  "start_time": "HH:MM",
  "end_time": "HH:MM",
  "duration_minutes": 120,
  "notes": "Opcional",
  "created_at": "timestamp",
  "updated_at": "timestamp"
}

---

### 4.2 Obtener Sesiones

GET /api/v1/work-sessions

Query params opcionales:
- project_id
- date
- from
- to

Response 200:
Lista de sesiones.

---

### 4.3 Eliminar Sesión

DELETE /api/v1/work-sessions/{session_id}

Response 204

---

## 5. Analytics

### 5.1 Resumen General

GET /api/v1/analytics/summary

Response 200:

{
  "total_minutes": 1234,
  "total_hours": 20.56,
  "weekly_minutes": 300,
  "project_breakdown": [
    {
      "project_id": "uuid",
      "total_minutes": 500
    }
  ]
}

---

## 6. Estructura de Errores

Todos los errores seguirán este formato:

{
  "status": 400,
  "error": "Bad Request",
  "message": "Descripción clara del error"
}