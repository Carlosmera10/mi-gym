# Mi Gym

App web propia para registrar los entrenamientos del gimnasio, al estilo de Hevy y sin suscripciones. Funciona en el móvil y en el ordenador (Chrome), se instala como app y va sin conexión.

Dirección: <https://carlosmera10.github.io/mi-gym/>

## Qué hace

- **Rutinas** (Piernas, Torso + Hombro, Full Body + Brazos, Comba + core y Sin impacto + core) con series, rangos de repeticiones, prioridad para sesiones de 30 / 45 min o completas, descanso, aproximaciones y alternativas por si la máquina está ocupada.
- **Sesión en el gimnasio**: «la última vez» en cada serie, la nota que dejaste para la próxima vez, sugerencia de progresión, peso en kg o lb, temporizador de descanso con vibración y sonido, y pantalla siempre encendida.
- **Técnica** de cada ejercicio (cómo hacerlo, cómo evitar lesiones y alternativas) y una guía general.
- **Historial** por sesión y por ejercicio.
- **Importación del historial de Hevy** (CSV) y copias de seguridad en JSON.
- **Sincronización entre el móvil y el ordenador** mediante un Gist secreto de GitHub, con los datos cifrados en el navegador.

## Archivos

| Archivo | Para qué |
|---|---|
| `index.html` | Toda la app (HTML, CSS y JavaScript, sin librerías). |
| `sw.js` | Service worker: funcionamiento sin conexión. Subir el número de `CACHE` en cada cambio. |
| `app.webmanifest` | Datos para instalarla. |
| `icon-192.png`, `icon-512.png` | Iconos. |

## Datos

Los datos se guardan solo en el navegador de cada dispositivo (IndexedDB, base `migym_db`). Este repositorio es público y **solo contiene código, nunca datos**. Todo lo que guarda la app empieza por `migym` para no mezclarse con las demás apps del mismo dominio.

## Sincronización

- Se activa en **Ajustes → Sincronización con el otro dispositivo** con un *fine-grained token* de GitHub que solo tenga el permiso **Account permissions → Gists: Read and write** (sirve el mismo del Revisor de Tareas) y una contraseña de cifrado.
- Los datos se cifran antes de salir del navegador (PBKDF2-SHA256 de 600.000 vueltas y AES-GCM de 256 bits, comprimidos con gzip) y se guardan en el archivo `migym-sync.json` de un Gist secreto. Ni GitHub ni nadie sin la contraseña puede leerlos.
- El token y la contraseña solo están en el `localStorage` de cada dispositivo (`migym_sync`), nunca en el Gist ni en este repositorio.
- Fusión elemento a elemento (ejercicios, carpetas, rutinas y sesiones): gana el cambio más reciente y los borrados se respetan salvo que el elemento se modificara después. Los ajustes y la interfaz no se sincronizan.
