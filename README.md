# Sistema de Automatización y Validación de Tickets Técnicos

## Descripción

Este proyecto corresponde a un script desarrollado en Google Apps Script para automatizar la revisión, validación y clasificación de tickets técnicos dentro de Google Sheets.

El sistema permite analizar tickets operacionales relacionados con:

- compensaciones técnicas,
- validaciones de OT,
- excepciones de puntaje,
- extensores cableados,
- duplicidad de tickets,
- y revisión de pagos históricos.

El objetivo principal es reducir el trabajo manual del área operativa mediante reglas automáticas de validación y respuesta.

---

# Tecnologías utilizadas

- Google Apps Script
- Google Sheets
- JavaScript

---

# Funcionalidad principal

El script procesa automáticamente cada fila de la hoja y ejecuta distintos controles operacionales.

## Funciones principales

### ✔ Control de tickets duplicados

El sistema agrupa tickets por:

```txt
Id_Orden_ticket
```

Si existen múltiples tickets asociados a una misma orden:

- identifica el ticket válido,
- detecta duplicados,
- y bloquea tickets inválidos.

### Lógica aplicada

- El ticket con el ID más alto se considera válido.
- Los tickets anteriores se marcan como duplicados.
- Se genera una respuesta automática.

Ejemplo:

```txt
Tu ticket asociado a la orden 1-XXXXXX está duplicado.
Se responderá únicamente el ticket 20015.
```

---

# ✔ Validación de pagos históricos

El script revisa:

```txt
Valor_pagado_bd_pago_app
```

y detecta:

- tickets previamente pagados,
- pagos parciales,
- pagos duplicados.

Esto permite controlar inconsistencias de pago dentro de una misma orden.

---

# ✔ Validación de OT digital

El sistema analiza el estado de la OT utilizando:

```txt
comp_ot_digital_bases
```

y clasifica automáticamente situaciones como:

| Estado detectado | Resultado |
|---|---|
| IGUAL: vacío | No existe OT digital |
| IGUAL: Con OT | OT válida |
| sin ot | No existe OT en base |
| con ot | Existe OT fuera de base de pago |

---

# ✔ Detección automática de excepciones

El script identifica automáticamente solicitudes relacionadas con:

- extensores,
- cableado UTP,
- excepciones operativas.

Utiliza expresiones regulares tolerantes a errores de escritura para detectar palabras como:

- excepción
- exepcion
- xcepcion
- extensor

---

# ✔ Cálculo automático de puntajes

El sistema calcula automáticamente:

```txt
Rgu_ticket
```

cuando se cumplen determinadas condiciones operativas.

## Requisitos

El ticket debe:

- NO ser duplicado,
- tener OT digital válida,
- contener materiales UTP,
- y poseer puntaje asociado.

---

# ✔ Automatización de respuestas

El script completa automáticamente:

```txt
Respuesta_ticket
```

con mensajes estandarizados según el resultado de las validaciones.

Ejemplos:

```txt
Excepción extensor por cable UTP.
```

```txt
No se encuentra OT digital registrada.
```

```txt
Ticket duplicado.
```

---

# Estructura de columnas requeridas

El sistema utiliza las siguientes columnas dinámicas:

| Columna | Descripción |
|---|---|
| Id_ticket | ID del ticket |
| Id_Orden_ticket | Orden asociada |
| Resolucion_ticket | Estado del ticket |
| Valor_pagado_bd_pago_app | Pago registrado |
| Valor_ticket_bd_pago_app | Valor ticket |
| Total_valorpagado_valorticket_bd_pago_app | Total calculado |
| Lista_mate_declarados | Materiales utilizados |
| comp_ot_digital_bases | Estado OT |
| Rgu_ticket | Puntaje asignado |
| Respuesta_ticket | Respuesta automática |

---

# Flujo general del sistema

## 1. Lectura de datos

El script obtiene:

- encabezados,
- columnas dinámicas,
- y registros de la hoja.

---

## 2. Construcción de mapa de órdenes

Se agrupan tickets por:

```txt
Id_Orden_ticket
```

para detectar duplicados.

---

## 3. Validación de duplicados

Se determina:

- ticket válido,
- tickets duplicados,
- y bloqueo automático de respuestas inválidas.

---

## 4. Validación de pagos

Se revisan pagos existentes dentro de tickets relacionados.

---

## 5. Validación OT

Se clasifica el estado de OT digital.

---

## 6. Detección de excepciones

Se analizan comentarios y respuestas para identificar excepciones.

---

## 7. Cálculo automático de puntaje

Se asignan valores automáticos cuando corresponde.

---

## 8. Escritura de resultados

El sistema actualiza automáticamente:

- Script_duplicados
- Script_ayuda_hisotorico_dupli
- Script_OT
- Script_Excepcion
- Rgu_ticket
- Respuesta_ticket

---

# Menú automático en Google Sheets

El script agrega automáticamente un menú personalizado:

```txt
Automatización Respuestas
```

con la opción:

```txt
Procesar Tickets para Revision
```

---

# Ejemplo de automatización

## Entrada

```txt
Ticket: 19564
Orden: 1-3KPCF6T1
Materiales: CABLE UTP
OT: IGUAL: Con OT
```

## Resultado automático

```txt
Rgu_ticket = 0.6
Respuesta_ticket = "Excepción extensor por cable UTP."
```

---

# Problemas detectados y mejoras futuras

## ⚠ Problema actual detectado

Las órdenes vacías se agrupan como si fueran la misma orden.

Ejemplo:

```txt
Orden | Tickets duplicados
```

Esto puede provocar falsos duplicados.

---

## ✔ Mejora recomendada

Excluir órdenes vacías antes de agrupar:

```javascript
if (!orden) continue;
```

---

# Posibles mejoras futuras

- Integración con SQL Server
- Integración con Power BI
- Dashboard de auditoría
- Clasificación mediante IA
- Historial automático de revisiones
- Exportación de reportes
- Sistema de trazabilidad

---

# Autor

Proyecto desarrollado para automatización operacional y control de tickets técnicos en entornos de telecomunicaciones.

---

# Licencia

Uso interno / privado.
