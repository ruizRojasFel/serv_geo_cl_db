<div align="center">

<h1> 💾 database api geo cl </h1>

*Scripts SQL para la creación y población de la base de datos de la API de geolocalización de Chile.*

[![Website](https://img.shields.io/badge/Próximamente-sitio.web-lightblue)](https://github.com/ruizRojasFel) [![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/ruizRojasFel/serv_geo_cl_db?tab=MIT-1-ov-file)

</div>

<br>

## Descripción

Este repositorio reúne todo lo necesario para levantar en PostgreSQL la base de datos que sirve a la API: configuración del usuario y la base, definición del esquema y carga de datos. Incluye la división político-administrativa completa del país (16 regiones, 56 provincias y 346 comunas), basada en la codificación oficial del [Código Único Territorial (CUT)](https://www.subdere.gov.cl/) del Ministerio del Interior.

---

## Estructura

```
db_api_geo_cl/
├── 01_config.sql              # Creación de base de datos, usuario y privilegios
├── 02_tables_serial.sql       # Esquema de tablas con SERIAL (INT, 4 bytes)
├── 03_tables_bigserial.sql    # Esquema de tablas con BIGSERIAL (BIGINT, 8 bytes)
└── 04_seed.sql                # Datos iniciales: regiones, provincias y comunas
```

---

## Descripción de archivos

### `01_config.sql`
Crea la base de datos, el usuario y le otorga los privilegios necesarios. Utiliza variables de entorno en formato `[VARIABLE]` que deben ser reemplazadas antes de ejecutar.

### `02_tables_serial.sql`
Define las tablas usando `SERIAL` (`INT`, 4 bytes). Soporta hasta ~2.100 millones de registros. Recomendado para este proyecto, ya que los datos geográficos de Chile son acotados y no escalan masivamente.

### `03_tables_bigserial.sql`
Define las mismas tablas usando `BIGSERIAL` (`BIGINT`, 8 bytes). Soporta hasta ~9.200 billones de registros. Recomendado si la API se extiende a otros países o se prevé un alto volumen de datos derivados.

> Usar solo uno de los dos: `02_tables_serial.sql` o `03_tables_bigserial.sql`.

### `04_seed.sql`
Inserta los datos oficiales de la división político-administrativa de Chile.

| Entidad    | Total |
|------------|-------|
| Regiones   | 16    |
| Provincias | 56    |
| Comunas    | 346   |

---

## Modelo de datos

```
region
  └── provincia (región_id → region.id)
        └── comuna (provincia_id → provincia.id)
```

Cada tabla incluye un `id` autogenerado como clave primaria. Las comunas incluyen el campo `codigo_cut` (único), que corresponde al código oficial del Ministerio del Interior.

---

## Ejecución

Ejecutar los scripts en orden:

```bash
psql -U postgres -f 01_config.sql
psql -U [DB_USER] -d [DB_NAME] -f 02_tables_serial.sql   # o 03_tables_bigserial.sql
psql -U [DB_USER] -d [DB_NAME] -f 04_seed.sql
```

---

## Requisitos

- PostgreSQL 13 o superior

---

## License

[![License](https://img.shields.io/badge/License-MIT-yellow)](https://github.com/ruizRojasFel/db-api-geo-cl/tree/main?tab=MIT-1-ov-file)

<br>

---

<div align="center">

<h2> Developer </h2>

<h3> Felipe Andrés Ruiz Rojas </h3>

[![LinkedIn](https://img.shields.io/badge/LinkedIn-linkedin.com%2Fin%2Fruizrojasfel-blue)](https://www.linkedin.com/in/ruizrojasfel) [![Website](https://img.shields.io/badge/Website-felruiz--dev.netlify.app-lightblue)](https://felruiz-dev.netlify.app/)

Copyright © 2026 Fel Ruiz
</div>
