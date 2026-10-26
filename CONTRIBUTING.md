# Cómo trabajamos en el código de Auge

Esta guía vale para todos los repositorios de Auge, salvo que un repositorio traiga su propio `CONTRIBUTING.md`. Es corta a propósito: si algo no está aquí, pregunta antes de inventar una regla nueva.

## Antes de empezar

1. Lee el `README.md` del repositorio: ahí está cómo arrancarlo en local y cómo correr las pruebas.
2. Copia `.env.example` a `.env` y llénalo con tus propios valores. **Nunca subas un `.env`** ni ninguna clave, contraseña o token, ni siquiera "por un momento": lo que entra al historial de git se queda ahí.
3. Configura git con tu nombre y tu correo. Cada persona hace commits con su propia cuenta; nunca con una cuenta compartida.

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu-correo@ejemplo.com"
```

## El flujo

Usamos un flujo sencillo: `main` siempre funciona, y todo cambio entra por Pull Request.

1. Actualiza `main`: `git switch main && git pull`.
2. Crea una rama corta para tu cambio: `git switch -c feat/pedidos-por-whatsapp`.
3. Haz commits pequeños y con sentido.
4. Sube la rama y abre un Pull Request hacia `main`.
5. Espera a que el CI pase en verde y a que alguien lo revise.
6. Se une con **Squash and merge** y la rama se borra.

Una rama vive días, no semanas. Si un cambio es grande, pártelo en varios Pull Requests que se puedan revisar por separado.

## Nombres de rama

`tipo/descripcion-corta`, en minúsculas, sin acentos y con guiones.

| Tipo | Para | Ejemplo |
| --- | --- | --- |
| `feat/` | Algo nuevo | `feat/recordatorios-whatsapp` |
| `fix/` | Corregir un error | `fix/total-en-bolivares` |
| `docs/` | Solo documentación | `docs/arranque-local` |
| `refactor/` | Reordenar código sin cambiar lo que hace | `refactor/servicio-de-cobros` |
| `test/` | Solo pruebas | `test/ciclos-de-membresia` |
| `chore/` | Mantenimiento y dependencias | `chore/actualizar-next` |
| `hotfix/` | Arreglo urgente en producción | `hotfix/pago-movil-duplicado` |

## Mensajes de commit

Seguimos [Conventional Commits](https://www.conventionalcommits.org/es/v1.0.0/), en español, en presente y en una línea de 72 caracteres o menos:

```
tipo(ámbito opcional): qué cambia
```

```
feat(pedidos): enviar el pedido por WhatsApp
fix(caja): redondear el total en bolívares
docs: explicar cómo correr las pruebas de base de datos
chore(deps): actualizar Next.js a 15.5
refactor(cobros): separar la conciliación del pago móvil
feat(api)!: cambiar el formato de la respuesta de pedidos
```

- Los tipos son los de las ramas, más `perf` (rendimiento), `style` (formato, sin cambiar lógica), `build` y `ci`.
- El `!` después del tipo marca un cambio que rompe algo. Explica en el cuerpo del commit qué se rompe y cómo adaptarse.
- Si hace falta contexto, deja una línea en blanco y escribe el porqué, no el qué: el qué ya está en el código.

## Pull Requests

- Título con el mismo formato de un commit: es el mensaje que queda en `main` al unir.
- Llena la plantilla: qué cambia, por qué y cómo se probó.
- Si cambia algo visible, agrega una captura o un video corto.
- Si toca la base de datos, incluye la migración y di si es reversible.
- No mezcles en un mismo PR un cambio de comportamiento con una limpieza grande de código.

### Al revisar

- Revisa que haga lo que dice, que tenga pruebas y que no suba secretos.
- Comenta el código, no a la persona. Si algo es una preferencia y no un problema, márcalo como "opcional".
- Aprueba cuando lo unirías tú mismo.

## Versiones

Usamos [versionado semántico](https://semver.org/lang/es/): `MAYOR.MENOR.PARCHE`.

| Sube | Cuando | Ejemplo |
| --- | --- | --- |
| PARCHE | Corriges un error | `1.4.2` → `1.4.3` |
| MENOR | Agregas algo sin romper nada | `1.4.3` → `1.5.0` |
| MAYOR | Cambias algo que obliga a clientes o datos a adaptarse | `1.5.0` → `2.0.0` |

- Un producto empieza en `0.1.0` y pasa a `1.0.0` con su primer cliente real en producción.
- Cada versión se marca con una etiqueta `vX.Y.Z` sobre `main` y un Release en GitHub con la lista de cambios.
- La versión también va en `package.json` o `composer.json`.

## Lo que nunca entra a un repositorio

- Archivos `.env` con valores reales, claves, tokens, certificados privados o volcados de base de datos.
- Datos personales de clientes o prospectos.
- Dependencias instaladas (`node_modules/`, `vendor/`) y lo que se genera al compilar (`.next/`, `dist/`, `public/build/`).
- Archivos comprimidos y binarios pesados. GitHub rechaza archivos de más de 100 MB.

Si subiste un secreto por error, avisa de inmediato: la clave se cambia primero y el historial se limpia después.

## Estilo y calidad

- Respeta el formato que ya tiene el repositorio (`.editorconfig`, Prettier, Pint). No reformatees archivos que no tocaste.
- Código, nombres de variables y comentarios técnicos pueden ir en inglés; textos que ve el cliente, documentación y commits, en español.
- Los textos que ve el cliente siguen la voz de Auge: claros, directos y cálidos. Tú, frases cortas, sin "soluciones integrales" ni "IA mágica". Números en formato venezolano: `1.473`, `866,56`, `Bs 3.400`.
- Toda lógica de dinero, fechas y ciclos lleva pruebas.

## ¿Dudas?

Escribe a [contacto@iauge.com](mailto:contacto@iauge.com) o abre un issue en el repositorio. Si es un problema de seguridad, sigue nuestra [política de seguridad](SECURITY.md) y no lo publiques en un issue.
