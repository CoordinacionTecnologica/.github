<p align="center">
  <img src="logo.svg" alt="Logo Coordinación Tecnológica" height="90" />
</p>

# Coordinación Tecnológica
### Secretaría de Salud - Municipalidad de Córdoba

Somos el área de tecnología de la **Secretaría de Salud de la Municipalidad de Córdoba**, responsable del diseño, desarrollo y mantenimiento de los sistemas de información que sostienen la gestión sanitaria municipal.

---

## ¿Qué hacemos?

Desarrollamos soluciones digitales orientadas a mejorar la atención ciudadana y la gestión interna de la red de salud municipal, incluyendo:

- Sistemas de gestión de turnos y atención en centros de salud
- Herramientas de seguimiento epidemiológico y estadísticas sanitarias
- Integraciones con sistemas provinciales y nacionales de salud
- Plataformas de soporte a equipos de salud en territorio

## Stack tecnológico

| Capa | Tecnología |
|------|-----------|
| Frontend | React 19 + Vite 6 |
| Base de datos | SQL Server / PostgreSQL |
| Autenticación | JWT (token via hash en URL) |
| Estilos | CSS puro con variables (dark mode nativo) |
| Iconos | FontAwesome |
| Alertas | SweetAlert2 |
| Tablas | DataTables |

## Integraciones

Nos integramos a servicios externos vía API-REST:

- **RENAPER** — Registro Nacional de las Personas
- **PUCO** — Padrón Único Consolidado Operativo

## Nuestro enfoque

- **Una plantilla, todas las apps** — todas las aplicaciones internas parten del mismo [template](https://github.com/CoordinacionTecnologica/template), con autenticación JWT, layout con sidebar/navbar y control de acceso por roles ya resuelto.
- **Roles en un solo lugar** — los roles y rutas visibles para cada perfil de usuario se definen en `ChildButtonService.js`, sin lógica dispersa por el código.
- **Auth centralizada** — el token JWT llega desde el portal/landing vía hash en la URL. Los roles se leen de `user_configs[].rol` dentro del payload.
- **Datos bajo nuestro control** — priorizamos infraestructura propia y evitamos dependencias externas innecesarias.
- **Código mantenible** — apostamos por soluciones documentadas, auditables y fáciles de traspasar entre miembros del equipo.

---

## ¿Cómo arranco un nuevo proyecto?

Usamos una plantilla base para todas las apps internas. Cloná el repositorio [template](https://github.com/CoordinacionTecnologica/template) y seguí estos pasos:

### 1 · Clonar y configurar el nombre

```bash
git clone https://github.com/CoordinacionTecnologica/template.git mi-nuevo-proyecto
cd mi-nuevo-proyecto
```

Cambiá el `name` en `package.json` al nombre del proyecto.

### 2 · Agregar las pestañas del menú

Todo el sistema de rutas, roles y pestañas de la sidebar se define en un solo lugar:

**`src/services/ChildButtonService.js`**

```js
export const childButtons = [
  {
    route: '/mi-ruta',
    label: 'Nombre en el menú',
    roles: ['admin', 'superAdmin'],  // quién puede verla
    icon: 'faPlus'                   // icono de FontAwesome
  }
];
```

Cada entrada en este array genera automáticamente una pestaña en la sidebar visible solo para los roles indicados.

### 3 · Registrar la ruta en App.jsx

```jsx
<Route
  path="/mi-ruta"
  element={<PrivateRoute element={<MiComponente />} path="/mi-ruta" />}
/>
```

La ruta en `App.jsx`, en `PrivateRoute` y en `ChildButtonService.js` deben coincidir exactamente.

### 4 · Configurar variables de entorno

Crear `.env.development` y `.env.production` con:

```env
VITE_LOGIN_URL=https://...
VITE_LANDING_PAGE_URL=https://...
VITE_NOT_ACCESS_RESOURCE=https://...
VITE_DISABLE_AUTH=true          # solo en desarrollo
```

### 5 · Configurar el basename (importante para producción)

Si la app se despliega bajo una subruta (`/mi-app/`), configurar en **dos lugares**:

**`src/main.jsx`**
```jsx
<BrowserRouter basename="/mi-app">
```

**`vite.config.js`**
```js
export default defineConfig({
  base: '/mi-app/',
  // ...
})
```

Si la app corre en la raíz del dominio, omitir este paso.

---

> Para más detalle sobre la arquitectura del template (autenticación JWT, roles, dark mode, navbar configurable) ver el `CLAUDE.md` del repositorio [template](https://github.com/CoordinacionTecnologica/template).

---

<sub>Municipalidad de Córdoba · Secretaría de Salud · Coordinación Tecnológica</sub>

---

## ¿Cómo hago CICD?

[CICD.txt](https://github.com/user-attachments/files/29296131/CICD.txt)



ir a GitHub
settings>accions>runners>new self-hosted runner
Windows x64

--------------------------------------------------------------------------------
CREAR CARPETA DONDE ALOJAR RUNNER DE MI PROYECTO
C:\Users\martinf\Desktop\actionsRunner
crear carpeta accionRECETAS
cd accionRECETAS
-------------------------------------------------------------------------------
-------------------------------------------------------------------------------
--------------------------------------------------------------------------------
ENTRAR A POWERSHELL

MOVERME A CARPETA DONDE INSTALAR RUNNNERS
cd C:\Users\martinf\Desktop\actionsRunner\runnerABM
-------------------------------------------------------------------------------
EMPEZAR A TIRAR COMANDOS POWERSHELL

// crea carpeta y se mueve dentro
mkdir actions-runner; cd actions-runner


//descarga zip de runner
Invoke-WebRequest -Uri https://github.com/actions/runner/releases/download/v2.334.0/actions-runner-win-x64-2.334.0.zip -OutFile actions-runner-win-x64-2.334.0.zip


//extrtaer e instalar
Add-Type -AssemblyName System.IO.Compression.FileSystem ; [System.IO.Compression.ZipFile]::ExtractToDirectory("$PWD/actions-runner-win-x64-2.334.0.zip", "$PWD")

-------------------------------------------------------------------------------
-------------------------------------------------------------------------------
-------------------------------------------------------------------------------
configurar


entrar a cmd con permisos de administrador

config.cmd --url https://github.com/xxx/xxx --token xxxxxxxxxxxxxxx --runasservice --windowslogonaccount "martinf" --windowslogonpassword "xxxxxxxx"


DESPES DE ESTE COMANDO SE ABRE PARA CONFIGFURAR

--------------------------------------------------------------------------------
|        ____ _ _   _   _       _          _        _   _                      |
|       / ___(_) |_| | | |_   _| |__      / \   ___| |_(_) ___  _ __  ___      |
|      | |  _| | __| |_| | | | | '_ \    / _ \ / __| __| |/ _ \| '_ \/ __|     |
|      | |_| | | |_|  _  | |_| | |_) |  / ___ \ (__| |_| | (_) | | | \__ \     |
|       \____|_|\__|_| |_|\__,_|_.__/  /_/   \_\___|\__|_|\___/|_| |_|___/     |
|                                                                              |
|                       Self-hosted runner registration                        |
|                                                                              |
--------------------------------------------------------------------------------

CONFIGURAR 

1
Un Runner Group es una agrupación lógica de runners.
Si no configuraste ninguno en GitHub:
ENTER

2
Nombre del runner
runnerRECETAS

3
enter

4 
enter





## ¿Cómo agrego un usuario y selecciono la reparticion?

---------------------------------------------------------------------------
///////////////////////////////////////////////////////////////////////////
IMPORTANTE

en aws no tocar código
siempre clonar, cambiar y pushear 
si se pushea a rama maion se hace el CICD
si algo esta mal o error de mime, hace rollback
///////////////////////////////////////////////////////////////////////////
---------------------------------------------------------------------------



///////////////////////////////////////////////////////////////////////////
CUANDO SE CREA UN ROL NUEVO
ir al front landign

agregarlo en src/services/RoutesAvailable.js
en la etiqueta 
route: '/landing'

queda tocar el back
///////////////////////////////////////////////////////////////////////////
CUANDO EL ROL DEBE DE PODE ASOCIARLO A UNA REPARTICION

ir al front landign
agregar el rol en "RoutesAvailable.js"


agregar la configuración en "AsociationService.jsx"

            'recetasUser-DispensAR': {
                endpoint: `${this.baseUrlApiAcces}/reparticiones`,
                label: 'Reparticiones',
                associationEndpoint: `${this.baseUrlApiAcces}/asociacionUsuario`,
                fieldName: 'id_reparticion',
                requiresRole: true,
                system: 'DispensAR',
                isMulti: true
            },

hay que tocar la apiacces, agregar el rol en Access/userasociation/idUser
///////////////////////////////////////////////////////////////////////////
[ROLES.txt](https://github.com/user-attachments/files/29296143/ROLES.txt)

