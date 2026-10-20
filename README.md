# 🛡️ SegurosOSINT — Sistema de Prospección y Enriquecimiento Automatizado
> **Plataforma Integral de Prospección B2B y Profesionales para Productores Asesores de Seguros (PAS)**  
> **Área geográfica:** La Plata y Gran La Plata, Provincia de Buenos Aires, Argentina.

---

## 📋 Resumen del Proyecto

**SegurosOSINT** es una solución Full-Stack diseñada para automatizar el ciclo completo de prospección comercial de un Productor Asesor de Seguros en La Plata. El sistema extrae prospectos calificados desde fuentes oficiales gubernamentales y colegiadas, los enriquece de forma autónoma con datos de contacto públicos (Teléfono, WhatsApp, Email, Web, LinkedIn) utilizando técnicas **OSINT**, y ofrece un **Dashboard de Gestión** interactivo con generador de propuestas de seguros personalizadas (ART, Vida Obligatorio, Responsabilidad Civil Profesional, Mala Praxis, Todo Riesgo Construcción).

```
                      ┌──────────────────────────────────────────┐
                      │          FUENTES OFICIALES               │
                      │  • Boletín Oficial PBA (Sociedades SRL/SA)│
                      │  • Colegios Profesionales La Plata       │
                      └────────────────────┬─────────────────────┘
                                           │
                                           ▼ [Scrapers Anti-Bot: Cheerio + Axios + Puppeteer]
                      ┌──────────────────────────────────────────┐
                      │       MOTOR DE EXTRACCIÓN & PARSING      │
                      │  • Normalización de Razón Social & CUIT  │
                      │  • Mapeo de Ramos Asegurables            │
                      └────────────────────┬─────────────────────┘
                                           │
                                           ▼
                      ┌──────────────────────────────────────────┐
                      │        MOTOR ENRIQUECEDOR OSINT          │
                      │  • Búsqueda Autónoma en Fuentes Públicas │
                      │  • Detección Web, Teléfonos (0221 / WA)  │
                      │  • Crawler de Emails & Perfiles LinkedIn │
                      └────────────────────┬─────────────────────┘
                                           │
                                           ▼ [Sequelize ORM]
                      ┌──────────────────────────────────────────┐
                      │      POSTGRESQL (Base de Datos Central)  │
                      │  • Tabla `Prospectos` con JSONB          │
                      └────────────────────┬─────────────────────┘
                                           │
                                           ▼ [REST API Express]
                      ┌──────────────────────────────────────────┐
                      │         DASHBOARD REACT + SCSS           │
                      │  • KPIs & Métricas en Tiempo Real        │
                      │  • Tabla Interactiva & Filtros Avanzados │
                      │  • Drawer de Ficha con 1-Click WhatsApp  │
                      └──────────────────────────────────────────┘
```

---

## 🛠️ Stack Tecnológico Obligatorio

| Capa | Tecnologías | Descripción |
| :--- | :--- | :--- |
| **Backend API** | Node.js (v18+) + Express.js | Arquitectura modular con controladores, servicios y rutas RESTful. |
| **Base de Datos & ORM** | PostgreSQL + Sequelize ORM | Modelado relacional con campos `JSONB`, índices optimizados y compatibilidad con fallback SQLite. |
| **Scraping & OSINT** | Cheerio + Axios + Puppeteer | Extracción de edictos, navegación headless con evasión anti-bot y rastreo web. |
| **Frontend / UI** | React (JSX) + SCSS Modules + Vite | Dashboard SPA sin TypeScript en la UI, diseño responsivo y tema oscuro premium. |

---

## 🏛️ Estructura del Proyecto

```
scraping micaela/
├── backend/
│   ├── config/
│   │   └── database.js              # Configuración Sequelize (PostgreSQL con fallback)
│   ├── models/
│   │   ├── Prospecto.js             # Modelo Sequelize con campos y validaciones
│   │   └── index.js                 # Inicialización y sincronización de base de datos
│   ├── scrapers/
│   │   ├── baseScraper.js           # Anti-bot: rotación User-Agent, delays, headers reales
│   │   ├── boletinOficialScraper.js # Extracción BOPBA (SRL, SA, SAU, SAS)
│   │   └── colegiosScraper.js       # Extracción Colegios (CALP, CPCEPBA, CAPBA, CIPBA, CMPBA)
│   ├── services/
│   │   └── osint/
│   │       └── osintEnricher.js     # Motor autónomo OSINT (Búsqueda web, RegEx, scoring)
│   ├── controllers/
│   │   └── prospectoController.js   # Lógica de endpoints, filtros, stats y triggers
│   ├── routes/
│   │   └── api.js                   # Definición de rutas Express
│   ├── utils/
│   │   └── seedData.js              # Padrón de prueba realista de La Plata
│   ├── .env.example                 # Variables de entorno de ejemplo
│   ├── .env                         # Variables de entorno locales
│   ├── package.json                 # Dependencias y scripts de backend
│   └── server.js                    # Punto de entrada Express
│
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Header/              # Barra superior con estado del motor y accesos rápidos
│   │   │   ├── StatsOverview/       # Tarjetas KPI del embudo de conversión y score OSINT
│   │   │   ├── FilterBar/           # Buscador textual y filtros por tipo, fuente y estado
│   │   │   ├── ProspectTable/       # Tabla interactiva con cambio ágil de estado y paginación
│   │   │   ├── LeadDrawer/          # Drawer lateral con contacto enriquecido y pitch WhatsApp
│   │   │   ├── ScraperModal/        # Diálogo para disparar extracción de fuentes oficiales
│   │   │   └── OSINTModal/          # Diálogo para disparar enriquecimiento autónomo
│   │   ├── services/
│   │   │   └── api.js               # Cliente Axios conectado al backend
│   │   ├── styles/
│   │   │   ├── _variables.scss      # Tokens de diseño (paleta, tipografías, gradientes)
│   │   │   └── globals.scss         # Estilos globales y reset
│   │   ├── App.jsx                  # Estado central de la aplicación
│   │   ├── App.scss                 # Estructura del layout
│   │   └── main.jsx                 # Montaje React
│   ├── index.html                   # HTML base con fuentes Plus Jakarta Sans
│   ├── vite.config.js               # Configuración Vite con proxy API a Express
│   └── package.json                 # Dependencias de frontend
│
└── README.md                        # Documentación técnica completa
```

---

## 📦 Módulos Implementados

### Módulo 1: Scraping de Fuentes Oficiales
1. **Boletín Oficial de la Provincia de Buenos Aires (BOPBA):**
   - Monitorea avisos judiciales y comerciales en busca de nuevas constituciones (`S.R.L.`, `S.A.`, `S.A.S.`, `S.A.U.`).
   - Extrae: Razón Social, CUIT, Domicilio Legal en La Plata, Fecha de Publicación y Objeto Social.
   - Algoritmo de mapeo automático de seguros obligatorios:
     - Sociedades constructoras/viales ➔ **ART + RC Cruzada + Caución de Obra**.
     - Gastronomía y comercio ➔ **ART + Vida Obligatorio (Dec. 1567/74) + Integral de Comercio**.
     - Empresas de logística ➔ **ART + Seguro de Flota Comercial + Transporte de Cargas**.

2. **Colegios Profesionales de La Plata:**
   - Extrae padrones públicos de matriculados activos de:
     - **CALP:** Colegio de la Abogacía de La Plata ➔ *RC Profesional Abogados / Caución Judicial*.
     - **CPCEPBA:** Consejo Profesional de Ciencias Económicas ➔ *RC Profesional Contadores / Sindicatura*.
     - **CMPBA D1:** Colegio de Médicos Distrito I La Plata ➔ *RC Médica / Mala Praxis Individual*.
     - **CAPBA D1:** Colegio de Arquitectos Distrito 1 ➔ *RC Profesional Arquitectura / Todo Riesgo Obra*.
     - **CIPBA D1:** Colegio de Ingenieros Distrito 1 ➔ *RC Profesional Ingeniería / Higiene y Seguridad*.

### Módulo 2: Motor Autónomo de Enriquecimiento OSINT
Si un prospecto no posee datos de contacto directos:
1. Genera queries dirigidas combinando Razón Social, Matrícula o Nombre con términos geolocalizados de La Plata.
2. Consulta motores de búsqueda y rastreadores web públicos de forma autónoma.
3. Extrae:
   - **Sitio web oficial** de la empresa o estudio profesional.
   - **Perfil corporativo o personal de LinkedIn**.
   - **Teléfonos y WhatsApp** con normalización de prefijos argentinos (`+54 9 221` / `0221`).
   - **Correos electrónicos corporativos** validados mediante RegEx.
4. Calcula un **Score OSINT** (0 al 100%) en base a la completitud y verificabilidad de los canales obtenidos.
5. Persiste la información en el campo `datos_contacto` (`JSONB`) del modelo Sequelize.

### Módulo 3: Base de Datos y Dashboard de Gestión
1. **Modelo Sequelize `Prospecto`:**
   - `id`: Identificador numérico autoincremental.
   - `tipo`: `'Empresa'` o `'Profesional'`.
   - `nombre_o_razon_social`: Razón social o nombre completo del colegiado.
   - `cuit`: CUIT fiscal normalizado.
   - `fuente`: Fuente oficial de extracción.
   - `matricula`: Número de matrícula profesional (si aplica).
   - `objeto_social_o_especialidad`: Objeto societario o especialidad médica/jurídica.
   - `domicilio_legal`: Domicilio o sede social registrada.
   - `datos_contacto`: Objeto JSON con `telefonos`, `emails`, `web`, `linkedin`, `whatsapp`, etc.
   - `seguros_recomendados`: Array con coberturas prioritarias sugeridas.
   - `score_osint`: Indicador de calidad y completitud de datos (0-100).
   - `estado_gestion`: `'Nuevo'`, `'Contactado'`, `'Cotizado'`, `'Cerrado'`, `'Descartado'`.
   - `fecha_extraccion`: Fecha de escaneo.
   - `notas`: Bitácora y seguimiento comercial del productor.

2. **Dashboard Interactivo React (JSX + SCSS):**
   - **Tarjetas KPI:** Totales de prospectos, distribución Empresas vs. Profesionales y embudo comercial en vivo.
   - **Filtros Combinados:** Búsqueda en tiempo real por texto, tipo, fuente oficial y estado de gestión.
   - **Drawer de Ficha Comercial:** Vista detallada de cada lead con enlaces directos a LinkedIn y Web oficial.
   - **Generador 1-Click de WhatsApp:** Redacta automáticamente un mensaje comercial adaptado a las necesidades específicas de la empresa o profesional (con mención a la publicación del Boletín Oficial o al Colegio matriculado) listo para enviar por WhatsApp Web.
   - **Actualización en Tiempo Real:** Botones ágiles para transicionar el lead entre estados (`Nuevo ➔ Contactado ➔ Cotizado ➔ Cerrado`).

---

## 🛡️ Técnicas Anti-Bot Implementadas

Para asegurar la continuidad operativa de los scrapers y evitar bloqueos por parte de los portales:
1. **Rotación Aleatoria de User-Agents:** Pool de navegadores reales modernos (Chrome, Edge, Firefox, Safari sobre Windows, macOS y Linux).
2. **Emulación de Navegación Humana:** Cabeceras HTTP completas (`Accept-Language: es-AR`, `Sec-Ch-Ua`, `Sec-Fetch-*`, `Referer`).
3. **Delays Aleatorios:** Intervalos estocásticos de espera entre peticiones (1.500 ms a 4.000 ms).
4. **Reintentos con Retroceso Exponencial:** Si una petición falla por saturación o timeout, el sistema reintenta con backoff exponencial progresivo.
5. **Puppeteer Stealth Config:** Ocultación de flags de automatización (`navigator.webdriver`, plugins simulados, viewport dinámico).

---

## 🚀 Guía de Instalación y Ejecución

### Prerrequisitos
- **Node.js** v18 o superior instalado.
- **npm** v9 o superior.
- **PostgreSQL** instalado y en ejecución (puerto por defecto: `5432`).

---

### Paso 1: Configuración de la Base de Datos PostgreSQL

1. Inicia sesión en PostgreSQL mediante `psql` o **pgAdmin**:
   ```sql
   CREATE DATABASE seguros_prospeccion;
   ```
2. Crea el usuario (o utiliza el usuario `postgres` por defecto):
   ```sql
   CREATE USER postgres WITH PASSWORD 'tu_password_aqui';
   GRANT ALL PRIVILEGES ON DATABASE seguros_prospeccion TO postgres;
   ```

---

### Paso 2: Configuración del Backend

1. Accede al directorio `backend`:
   ```bash
   cd backend
   ```
2. Instala las dependencias:
   ```bash
   npm install
   ```
3. Edita el archivo `.env` configurando las credenciales de tu PostgreSQL:
   ```env
   PORT=5000
   NODE_ENV=development

   # Conexión PostgreSQL (Sequelize)
   DB_DIALECT=postgres
   DB_HOST=localhost
   DB_PORT=5432
   DB_NAME=seguros_prospeccion
   DB_USER=postgres
   DB_PASSWORD=tu_password_aqui
   DB_LOGGING=false

   # Si deseas probar localmente sin credenciales de PostgreSQL, puedes usar:
   # DB_DIALECT=sqlite
   ```

4. Ejecuta el seed de datos para poblar la base con prospectos iniciales de La Plata:
   ```bash
   npm run db:seed
   ```

5. Inicia el servidor backend en modo desarrollo:
   ```bash
   npm run dev
   # o bien: npm start
   ```
   El backend iniciará en: **`http://localhost:5000`**

---

### Paso 3: Configuración y Ejecución del Frontend

1. En una nueva terminal, accede al directorio `frontend`:
   ```bash
   cd frontend
   ```
2. Instala las dependencias:
   ```bash
   npm install
   ```
3. Inicia el servidor de desarrollo de Vite:
   ```bash
   npm run dev
   ```
4. Abre tu navegador en: **`http://localhost:5173`**

---

## 📡 Referencia de la API REST

### Prospectos
- `GET /api/prospectos`: Listado paginado con filtros (`?tipo=Empresa&fuente=CALP&estado_gestion=Nuevo&busqueda=LaPlata&page=1&limit=15`).
- `GET /api/prospectos/:id`: Ficha completa de un prospecto.
- `PATCH /api/prospectos/:id/estado`: Actualiza estado de gestión (`Nuevo`, `Contactado`, `Cotizado`, `Cerrado`, `Descartado`) y notas.
- `PUT /api/prospectos/:id`: Actualización general de datos.
- `DELETE /api/prospectos/:id`: Elimina un registro.
- `GET /api/prospectos/stats`: Métricas en vivo (totales, desglose por estado, tipo, fuentes y score OSINT medio).

### Scrapers & Automatización
- `POST /api/scraper/boletin`: Dispara la extracción del Boletín Oficial de la Pcia. de Buenos Aires.
- `POST /api/scraper/colegios`: Dispara el escaneo de padrones de colegios profesionales.
- `POST /api/scraper/all`: Ejecuta todos los scrapers en paralelo.

### Motor OSINT
- `POST /api/osint/enrich-all`: Procesa en lote todos los prospectos pendientes de enriquecimiento.
- `POST /api/osint/enrich/:id`: Ejecuta el enriquecimiento autónomo para un lead en específico.

---

## 📄 Licencia y Autor
Diseñado y desarrollado como solución de ingeniería Full-Stack para prospección automatizada de seguros en La Plata, Argentina.
Código modular, tipado seguro mediante Sequelize ORM y arquitectura escalable para incorporación de nuevas fuentes oficiales bonaerenses.
# osint-micaela
