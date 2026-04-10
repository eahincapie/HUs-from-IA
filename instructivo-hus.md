# Contexto para generar Historias de Usuario (INVEST) + Historias Técnicas
Este documento define **cómo debes generar épicas e historias** para un proyecto, de modo que el backlog quede **listo para ejecutar**.

El objetivo es generar historias **claras: independientes, negociables, de valor, estimables,  pequeñas y testeables (INVEST)**, con criterios de aceptación completos y trazabilidad técnica.

---

## 0) Reglas
1. **Cada historia debe ser INVEST de verdad.** Si no lo es, debes **dividirla** hasta que lo sea.
2. **Evita “mega-historias” tipo “Gestionar X”.** Debes **partir por acciones** que generan comportamiento distinto del sistema (ej: listar, ver detalle, crear, editar, eliminar, subir imagen, buscar/filtrar, exportar, etc.).
3. **Historias funcionales ≠ tareas técnicas.**  
   - Funcional: valor al usuario/negocio (UI + reglas + datos).
   - Técnica (enabler): habilita o asegura capacidad (API, seguridad, CI/CD, observabilidad, performance, migraciones, etc.) y **también debe ser INVEST**.
4. Cada historia debe incluir **Criterios de Aceptación (CA)** claros y verificables.
5. Cada épica debe incluir **contexto suficiente** (alcance, supuestos, NFR, riesgos, dependencias).
6. Si hay incertidumbre relevante, crea un **Spike** (investigación acotada) con entregables claros.

---

## 1) Qué significa INVEST (y cómo comprobarlo)
INVEST es un checklist para verificar calidad de una historia:

- **I – Independent (Independiente):** idealmente puede implementarse sin depender de otras historias.
  - Si depende: declara la dependencia y/o crea una historia “habilitadora”.
- **N – Negotiable (Negociable):** no es un contrato rígido; describe necesidad/valor y deja espacio a solución.
- **V – Valuable (Valiosa):** aporta valor observable al usuario/negocio.
- **E – Estimable (Estimable):** el equipo puede estimarla porque hay claridad suficiente (o se crea spike).
- **S – Small (Pequeña):** cabe en un sprint (o en pocos días). Si no, divide.
- **T – Testable (Testeable):** tiene CA que permiten verificar “hecho / no hecho”.

### Checklist rápido INVEST por historia (obligatorio)
- ¿Se puede implementar y desplegar sin completar 5 cosas antes? (I)
- ¿Está redactada como necesidad, no como solución cerrada? (N)
- ¿Se entiende el valor? (V)
- ¿Se puede estimar con la info disponible? (E)
- ¿Cabe en el sprint del equipo? (S)
- ¿Se puede probar con CA claros (happy path + errores + permisos)? (T)

---

## 2) Cómo dividir historias (“cada acción genera historia” — con criterio)
### 2.1 División recomendada por acciones (CRUD+)
Para cualquier “Gestionar X” o cualquier otra acción que sea muy general, por defecto considera estas historias (ajusta según aplique):
1) **Listar X** (con paginación/orden/filtros básicos si el valor lo requiere, en caso de listados complejos podría implicar partir la historia, ver numeral 7)  
2) **Ver detalle de X**
3) **Crear X**
4) **Editar X**
5) **Eliminar X** (o inactivar/archivar, según reglas)
6) **Subir/gestionar archivos/imágenes de X** (si cambia flujos/servicios, a veces la creación o modificación de datos también incluye archivos, deberían separarse para poder estimarlas y poder aislar los flujos)
7) **Buscar/filtrar X** (si tiene valor real distinto al listar)
8) **Permisos/Roles para X** (si no es transversal ya definido)
9) **Auditoría/bitácora de X** (si aplica por compliance)
10) **Exportar/importar X** (si es requerido)

> Regla práctica: **si la acción cambia endpoints, reglas, UI y validaciones**, merece historia propia.

### 2.2 División vertical (por valor)
Divide por “rebanadas” que entregan valor completo:
- UI mínima + validación + persistencia + respuesta visible para el usuario.
- Evita “solo backend” y “solo frontend” en historias funcionales (eso va dentro de tareas, o en historia técnica si es habilitador transversal).

### 2.3 Señales de que debes dividir más
- Más de 8–10 criterios de aceptación.
- Incluye 2–3 pantallas o más.
- Incluye múltiples roles con reglas muy distintas.
- Mezcla “crear” con “aprobar” con “notificar” con “reportar”.
- Tiene múltiples integraciones externas.

---

## 3) Estructura obligatoria del backlog
El backlog se organiza así:

- **Épicas**
  - Contexto, objetivo, alcance y supuestos
  - Personas/roles involucrados
  - Requisitos no funcionales (NFR) aplicables
  - Dependencias / integraciones
  - Glosario y entidades de datos clave
  - Lista de historias (Modo I–V según nivel requerido)

---

## 4) Requisitos no funcionales (NFR) y cómo impactan historias
Los NFR NO son “notas sueltas”; deben convertirse en:
- **(A)** reglas transversales en **Definition of Done (DoD)**, y/o
- **(B)** **historias técnicas** (enablers) cuando requieran trabajo explícito, y/o
- **(C)** criterios de aceptación dentro de historias funcionales cuando apliquen a pantallas/flows específicos.

### 4.1 NFR típicos (usar como checklist)
- **Compatibilidad:** navegadores, SO, dispositivos, versiones soportadas.
- **Responsive:** rangos de resolución (mobile/tablet/desktop).
- **Performance:** tiempos objetivo, carga, lazy loading, límites.
- **Seguridad:** OWASP, roles, autorización, rate limiting, cifrado, manejo de sesiones, secretos.
- **Accesibilidad:** WCAG AA (si aplica).
- **Observabilidad:** logs, métricas, trazas, alertas.
- **Disponibilidad/DR:** backups, RPO/RTO (si aplica).
- **Privacidad/PII:** retención, masking, auditoría.

### 4.2 Cómo reflejar NFR en la estimación
- Si el NFR aplica a **todas** las historias UI: agrega una épica/historia técnica de **matriz de pruebas** + “quality gates”, y añade CA de validación mínima por historia.
- Si el NFR aplica a **solo algunas** pantallas: agrega CA específicos a esas historias.
- Si requiere infraestructura/herramientas: crea historias técnicas (ej. WAF, headers, SAST/DAST, CDN, cache, pipeline, etc.).

---

## 5) Definiciones (DoR y DoD)
### 5.1 Definition of Ready (DoR) mínima por historia
Una historia está “Ready” si:
- Tiene persona + necesidad + valor.
- Tiene CA (modo IV o V cuando aplique).
- Tiene dependencias declaradas.
- Tiene supuestos/alcance claro.
- Si hay incertidumbre, existe spike o se acotó el alcance.

### 5.2 Definition of Done (DoD) sugerida (ajusta al proyecto)
Para marcar “Done”:
- Código mergeado con revisión.
- Pruebas unitarias mínimas (según criticidad).
- Pruebas funcionales según CA.
- Manejo de errores y estados vacíos.
- Logs relevantes (si aplica).
- Documentación mínima (README / endpoints / notas operativas).
- Cumple NFR aplicables (seguridad/responsive/performance mínimos).
- Desplegado en ambiente objetivo (si aplica en el sprint).

---

## 6) Cómo redactar Criterios de Aceptación (CA) impecables
### 6.1 Reglas para CA
- Cada CA debe ser **atómico**, verificable y sin ambigüedad.
- Incluye **Happy Path** + **Errores/validaciones** + **Permisos** + **Estados vacíos**.
- Evita “debe ser rápido” → usa métrica o elimina ambigüedad.
- Evita “como en el sistema actual” → describe comportamiento.
- Si hay roles, define: **quién puede hacer qué**.

### 6.2 Formato recomendado de CA
- **Modo IV:** prosa estructurada **Dado / Cuando / Entonces** (en texto, sin Gherkin).
- **Modo V:** Gherkin (BDD) en bloque `gherkin`.

Ejemplo CA (Modo IV – prosa con D/C/T):
- **Dado** que el usuario tiene rol Administrador  
  **Cuando** crea un producto con nombre válido y precio válido  
  **Entonces** el sistema guarda el producto y lo muestra en el listado

Ejemplo CA (Modo V – Gherkin):
```gherkin
Scenario: Crear producto exitosamente
  Given el usuario tiene rol "Administrador"
  And estoy en "Crear producto"
  When ingreso nombre "Café Premium" y precio "25000"
  And presiono "Guardar"
  Then veo el mensaje "Producto creado"
  And el producto aparece en el listado
````

---

## 7) “Modos” de representación (I, II, III, IV, V)

> Los modos indican el **nivel de detalle** con el que se documenta la historia.
> Usa el modo solicitado por la organización. Si no se especifica, usa **Modo IV por defecto**.

### Modo I — Solo título

**Formato:**

* ID + Título conciso orientado a valor

**Ejemplo (Modo I):**

* HU-PROD-001: Listar productos

---

### Modo II — Título + Como/Quiero/Para

**Formato:**

* ID + Título
* Como / Quiero / Para

**Ejemplo (Modo II):**

* HU-PROD-001: Listar productos
  **Como** vendedor
  **Quiero** ver el listado de productos
  **Para** agregarlos rápidamente a una venta

---

### Modo III — Modo II + Contexto de wireframe (boceto)

**Cuándo usar:** UI compleja, muchas decisiones visuales, validaciones, estados.

**Formato:**

* Todo lo del Modo II
* Wireframe (texto) + estados + componentes + responsive

**Ejemplo (Modo III):**

* HU-PROD-001: Listar productos
  Como vendedor, Quiero ver productos, Para agregarlos a una venta.

**Wireframe (texto):**

* Pantalla: **Productos**
* Componentes:

  * Header: “Productos” + botón **Crear**
  * Barra de búsqueda: [________] (placeholder: “Buscar por nombre o SKU”)
  * Filtros: Categoría (dropdown), Estado (Activo/Inactivo)
  * Tabla/Listado:

    * Columnas: Imagen | Nombre | SKU | Precio | Stock | Estado | Acciones(Ver/Editar)
  * Paginación: 10/25/50 por página + contador
* Estados:

  * Loading: skeleton
  * Empty: “No hay productos. Crea el primero.”
  * Error: banner con reintento
* Responsive:

  * Mobile: listado en tarjetas (Nombre, Precio, Stock, Acciones)
  * Desktop: tabla completa

---

### Modo IV — Modo II + CA en prosa (Dado/Cuando/Entonces)

**Formato:**

* Todo lo del Modo II
* Criterios de aceptación en prosa estructurada D/C/T

**Ejemplo (Modo IV):**

* HU-PROD-003: Crear producto
  **Como** administrador
  **Quiero** crear un producto
  **Para** venderlo en el POS

**Criterios de Aceptación:**

1. **Dado** que el usuario tiene rol Administrador
   **Cuando** diligencia nombre, precio y stock válidos y presiona “Guardar”
   **Entonces** el sistema crea el producto y muestra confirmación.
2. **Dado** que el nombre está vacío
   **Cuando** el usuario intenta guardar
   **Entonces** el sistema muestra validación “El nombre es obligatorio” y no guarda.
3. **Dado** que el precio no es numérico o es negativo
   **Cuando** el usuario intenta guardar
   **Entonces** el sistema bloquea el guardado y muestra el error correspondiente.
4. **Dado** que el producto fue creado
   **Cuando** vuelvo al listado
   **Entonces** el producto aparece con estado “Activo”.

---

### Modo V — Modo II + CA en BDD (Gherkin)

**Formato:**

* Todo lo del Modo II
* Bloque `gherkin` con Feature/Scenarios

**Ejemplo (Modo V):**

* HU-PROD-004: Editar producto
  Como administrador, quiero editar un producto, para mantener información actualizada.

```gherkin
Feature: Edición de productos

Scenario: Editar producto exitosamente
  Given el usuario tiene rol "Administrador"
  And existe el producto "Café Premium" con precio 25000
  When edito el precio a 27000 y presiono "Guardar"
  Then veo el mensaje "Producto actualizado"
  And el producto muestra precio 27000 en el detalle

Scenario: No permitir precio negativo
  Given el usuario tiene rol "Administrador"
  And existe un producto
  When edito el precio a -1 y presiono "Guardar"
  Then veo el error "El precio debe ser mayor o igual a 0"
  And el sistema no guarda cambios
```

---

## 8) Plantilla estándar de historia (recomendada para Copilot)

> Cuando generes historias, usa esta plantilla y rellena todo lo posible.

### 8.1 Historia funcional (plantilla)

* **ID:** HU-XXXX-###
* **Épica:** EPIC-XXXX
* **Título:** (verbo + objeto + valor)
* **Persona:** (rol)
* **Historia:** Como ___ Quiero ___ Para ___
* **Valor/impacto:** (1–2 líneas)
* **Alcance (In/Out):**

  * In: …
  * Out: …
* **Dependencias:** (historias, APIs, proveedores)
* **Supuestos:** (si aplica)
* **Datos/Entidades impactadas:** (ej. Product, Category)
* **Permisos:** (roles autorizados)
* **Telemetría/Analítica:** (eventos si aplica)
* **NFR aplicables:** (seguridad, responsive, performance, etc.)
* **Modo:** (I/II/III/IV/V)
* **Criterios de aceptación:** (Modo IV o V cuando aplique)
* **Notas de implementación (opcional):** endpoints, validaciones, reglas

### 8.2 Historia técnica (enabler) — plantilla

> Las historias técnicas deben ser INVEST: pequeñas, valiosas (para el sistema/equipo), testeables.

* **ID:** HT-XXXX-###
* **Épica:** EPIC-PLATAFORMA / EPIC-SEGURIDAD / EPIC-DEVOPS (según aplique)
* **Título:** (verbo + capacidad técnica)
* **Contexto:** qué habilita y por qué es necesaria
* **Resultado esperado:** observable/medible
* **Alcance (In/Out):**
* **Riesgos/Dependencias:**
* **Criterios de aceptación (Modo IV o V):**

  * Dado/Cuando/Entonces o Gherkin
* **Evidencia de done:** logs, screenshots, pipeline, reporte, endpoints, etc.

Ejemplos de historias técnicas comunes:

* Configurar **pipeline CI/CD** con quality gates (lint, tests, build, deploy).
* Implementar **observabilidad** (logs estructurados, métricas, trazas).
* Aplicar **headers de seguridad** + configuración CSP.
* Integrar **SAST/DAST** y remediación base.
* Crear **migraciones** y seed de datos inicial.
* Implementar **rate limiting** y protección básica anti abuso.
* Crear **contratos OpenAPI** + versionado.
* Configurar **cache/CDN** si aplica.

---

## 9) Ejemplo completo: Épica “Gestión de Productos” (cómo debe quedar)

### Épica: EPIC-PROD — Gestión de Productos

**Objetivo:** Permitir administrar productos para venta (crear, consultar, editar, desactivar) con soporte de imagen y búsqueda.
**Personas:** Administrador, Vendedor.
**Alcance (In):** CRUD de productos, imagen principal, listado con búsqueda/filtro básico.
**Fuera de alcance (Out):** Variantes avanzadas, combos, sincronización con ERP externo (si no está explícito).
**Entidades:** Product(id, name, sku, price, stock, status, imageUrl, createdAt, updatedAt)
**NFR:** Responsive (360–1440px), permisos por rol, auditoría mínima (created/updated by), validaciones.
**Dependencias:** autenticación/roles, storage para imágenes (si aplica).

#### Historias (ejemplo de partición INVEST)

1. HU-PROD-001: Listar productos (Modo III/IV)
2. HU-PROD-002: Ver detalle de producto (Modo IV)
3. HU-PROD-003: Crear producto (Modo IV/V)
4. HU-PROD-004: Editar producto (Modo IV/V)
5. HU-PROD-005: Desactivar/Eliminar producto (Modo IV)
6. HU-PROD-006: Subir/actualizar imagen principal de producto (Modo III/IV)
7. HU-PROD-007: Buscar/filtrar productos (Modo III/IV)

#### Historias técnicas relacionadas (ejemplo)

* HT-PLAT-010: Endpoint y contrato OpenAPI para productos (Modo IV/V)
* HT-SEC-020: Control de acceso por roles para productos (Modo IV)
* HT-OBS-030: Logs estructurados de operaciones CRUD (Modo IV)

---

## 10) Guía para “Wireframes” (cuando el modo incluye UI)

Cuando una historia requiera wireframe (Modo III o superior), describe:

* Nombre de pantalla y objetivo
* Componentes (inputs, tablas, botones, mensajes)
* Estados: loading, empty, error, success
* Validaciones (inline y al guardar)
* Comportamiento responsive (mobile vs desktop)
* Accesibilidad básica (labels, foco, contraste si aplica)

> El wireframe puede ser textual (ASCII) o una lista detallada.
> Si existe Figma, referencia la ruta/URL (si está disponible) y resume decisiones clave.

---

## 11) Instrucciones para Copilot: cómo generar el backlog a partir de un requerimiento

Cuando recibas un requerimiento (texto, lista de funcionalidades o módulos), debes:

1. Crear **épicas** por dominio/funcionalidad.
2. Por cada épica, generar historias funcionales **partidas por acción** (INVEST).
3. Para cada historia, elegir el **modo de representación** (IV por defecto; III si UI compleja; V si hay reglas críticas).
4. Incluir **CA completos** (errores + permisos + estados).
5. Identificar **historias técnicas** necesarias (seguridad, API, CI/CD, observabilidad, migraciones, performance).
6. Si hay incertidumbre, crear **spikes** (tiempo acotado, entregables claros).

---

## 12) Convenciones de nomenclatura (sugeridas)

* Épicas: `EPIC-<DOMINIO>` (ej. EPIC-PROD, EPIC-VENTA, EPIC-SEG)
* Historias funcionales: `HU-<DOMINIO>-###` (ej. HU-PROD-003)
* Historias técnicas: `HT-<AREA>-###` (ej. HT-SEC-020, HT-DEVOPS-001)
* Spikes: `SP-<DOMINIO>-###`

---

## 13) Calidad mínima esperada del backlog (para “cero fricción”)

Un backlog listo para Operaciones debe permitir:

* Entender claramente **qué construir** y **cómo validar**.
* Detectar dependencias temprano.
* Evitar re-trabajo por NFR omitidos.
* Estimar sin adivinar.

> Si una historia no se puede probar con sus CA, no está lista.

---
