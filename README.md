# 🏦 EconoApp — Plataforma de Gestión Económica
### Proyecto Corte 2 · Grupo 1 · Programación 5925 · Periodo 2026-1

---

## 📋 Descripción del Proyecto

**EconoApp** es una plataforma de gestión económica personal desarrollada con arquitectura profesional de tres capas: presentación, lógica de negocio (POO) y persistencia (SQLite). Permite a clientes registrar y administrar sus activos financieros e historial de transacciones, con reportes consolidados mediante consultas JOIN.

---

## 🏗️ Arquitectura del Sistema

```
┌─────────────────────────────────────────────┐
│              CAPA DE PRESENTACIÓN            │
│          Menú interactivo (while)            │
├─────────────────────────────────────────────┤
│              CAPA DE NEGOCIO (POO)           │
│  Persona  → Cliente  / Analista             │
│  Activo   → Inversion / CuentaBancaria      │
│  Transaccion → Ingreso / Egreso             │
├─────────────────────────────────────────────┤
│           CAPA DE PERSISTENCIA               │
│    SQLite · 3 tablas · PK · FK · JOIN        │
└─────────────────────────────────────────────┘
```

---

## 🧬 Jerarquía de Clases (POO)

```
Persona  (padre)
  ├── Cliente    (hijo) — perfil de riesgo, saldo inicial
  └── Analista   (hijo) — especialidad, certificación

Activo   (padre)
  ├── Inversion      (hijo) — tipo, tasa de retorno, plazo
  └── CuentaBancaria (hijo) — banco, tipo de cuenta, número

Transaccion (padre)
  ├── Ingreso (hijo) — fuente
  └── Egreso  (hijo) — categoría
```

Cada clase implementa **encapsulamiento** con atributos privados (`__`), getters/setters y método `descripcion()` con polimorfismo.

---

## 🗄️ Modelo de Base de Datos

### Tablas y relaciones

| Tabla | Llave Primaria | Llave Foránea | Descripción |
|---|---|---|---|
| `clientes` | `id` (PK) | — | Datos del cliente y perfil de riesgo |
| `activos` | `id` (PK) | `id_cliente → clientes.id` | Inversiones y cuentas bancarias |
| `transacciones` | `id` (PK) | `id_cliente → clientes.id` | Ingresos y egresos por cliente |

> Ambas FK tienen `ON DELETE CASCADE`: al eliminar un cliente, sus activos y transacciones se eliminan automáticamente.

### Diagrama entidad-relación

Ver carpeta [`design/ERD_EconoApp.pdf`](design/ERD_EconoApp.pdf)

---

## ⚙️ Operaciones CRUD Implementadas

| Tabla | Crear | Leer | Actualizar | Eliminar |
|---|---|---|---|---|
| Clientes | ✅ | ✅ | ✅ | ✅ |
| Activos | ✅ | ✅ (JOIN) | ✅ | ✅ |
| Transacciones | ✅ | ✅ (JOIN) | ✅ | ✅ |

**Consulta JOIN destacada:** `resumen_financiero_por_cliente()` — une las tres tablas para calcular total en activos, ingresos, egresos y flujo neto por cliente.

---

## 🛡️ Robustez y Manejo de Errores

- Todas las operaciones de BD están envueltas en bloques `try-except-finally`
- Validación de entradas numéricas en el menú interactivo
- Integridad referencial activada con `PRAGMA foreign_keys = ON`
- Función `verificar_auditoria()` confirma mínimo 5 registros por tabla
- Datos iniciales de auditoría con 6 clientes, 6 activos y 7 transacciones

---

## 🚀 Cómo Ejecutar

### Requisitos
- Python 3.8+
- Jupyter Notebook o JupyterLab
- Librería estándar: `sqlite3`, `os`, `datetime` (sin instalación adicional)

### Pasos

```bash
# 1. Clonar el repositorio
git clone https://github.com/<usuario>/Proyecto_Corte_2_Grupo_1.git
cd Proyecto_Corte_2_Grupo_1

# 2. Abrir el notebook
jupyter notebook app_escalada_grupo_1.ipynb

# 3. Ejecutar todas las celdas en orden (Kernel → Restart & Run All)
```

Al ejecutar la última celda se abre el **menú interactivo** en la consola del notebook. La base de datos `base_datos_startup_grupo_1.db` se crea automáticamente en el mismo directorio.

---

## 📁 Estructura del Repositorio

```
Proyecto_Corte_2_Grupo_1/
│
├── README.md                          ← Este archivo
├── app_escalada_grupo_1.ipynb         ← Código fuente principal
├── base_datos_startup_grupo_1.db      ← BD generada al ejecutar (auto)
│
└── design/
    ├── DiagramaClases_EconoApp.pdf    ← Diagrama de clases POO
    └── ERD_EconoApp.pdf               ← Modelo Entidad-Relación
```

---

## 🧪 Verificación de Criterios de Evaluación

| Criterio | Detalle | Estado |
|---|---|---|
| **Lógica y Diseño** | Diagrama de clases + ERD en `design/` | ✅ |
| **POO y Encapsulamiento** | 3 clases padre, 6 clases hijo, self, getters/setters | ✅ |
| **SQL y Relaciones** | 3 tablas con PK/FK, 4 ops CRUD, JOIN, ≥5 registros | ✅ |
| **Robustez** | try-except en todas las funciones | ✅ |
| **GitHub** | Repositorio público con estructura requerida | ✅ |

---

## 👥 Integrantes del Grupo 1

| Nombre | Rol |
|---|---|
| *(Integrante 1)* | Desarrollo POO |
| *(Integrante 2)* | Capa de persistencia SQL |
| *(Integrante 3)* | Menú e integración |

---

> *"Los datos son el activo más valioso de su empresa; la arquitectura que los sostiene define su éxito."*
