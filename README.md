# rueda-ia-catalogo

Catálogo público de módulos de IA local (motores, modelos, oído y voz) para el programa de escritorio **Rueda de Tonalidad**.

El programa lee [`catalogo.json`](catalogo.json) al arrancar (una vez al día como mucho) y con el botón **«🔄 Buscar actualizaciones»** de Ajustes → IA. Si aquí hay una versión más nueva que la suya, avisa de los modelos o motores nuevos y los descarga de sus páginas oficiales cuando se le pide, comprobando la huella SHA-256 de cada archivo.

- Solo contiene esta lista: ningún dato personal, ningún modelo ni programa (cada módulo enlaza a su sitio oficial: GitHub de llama.cpp / whisper.cpp / Piper y Hugging Face).
- Cada enlace está fijado a una revisión concreta, con su tamaño exacto, su SHA-256 y su licencia.

## Formato (versión 2)

| Campo | Qué es |
|---|---|
| `version` | Número del catálogo. Se sube en cada cambio; el programa se queda con el más alto. |
| `formato` | Versión del formato. Un programa ignora los catálogos de un formato que no entiende. |
| `modulos[].rol` | `motor`, `chat`, `busqueda`, `oido-motor`, `oido-modelo`, `voz-motor` o `voz`. Dentro de cada rol, del mejor al peor. |
| `modulos[].si` | Cuándo se recomienda: `gpu` (`nvidia`, `otra`), `vram_min` (GB), o `nunca`. En los profesores (`chat`), el programa decide además si **caben** con su tamaño, la memoria de la tarjeta y la RAM del equipo (formato 2, desde la versión 3 del catálogo). |
| `modulos[].params_b` | Miles de millones de parámetros del modelo. |
| `modulos[].activos_b` | Solo en los MoE: los que trabajan en cada palabra. Un MoE que no cabe entero en la tarjeta puede repartirse con la RAM (llama.cpp `--fit`). |
| `modulos[].destaca` | En qué destaca, en una frase (se enseña en «🎓 Elige tu profesor»). |
| `modulos[].build` | Versión del motor (llama.cpp `bNNNNN`; whisper.cpp 10900 = 1.9.0). |
| `modulos[].motor_min` | Versión mínima de llama.cpp que necesita un modelo: el programa actualiza el motor antes. |
| `modulos[].desde` | Versión del catálogo en la que llegó el módulo (para avisar de lo nuevo). |
| `modulos[].porque` | Por qué es mejor (se enseña en el aviso). |
| `modulos[].archivos[]` | `url`, `sha256`, `tam` (bytes) y, si es un zip o tar.gz, `descomprimir` y `quitar_carpeta`. |

## Orden de los profesores

Los módulos `chat` van **de mejor a peor**. El programa enseña solo los que caben en el equipo, en ese orden: 🥇 Recomendado, 🥈 2.ª opción, 🥉 3.ª opción y «otros que también te valen». Hoy, para una tarjeta de 24 GB: Qwen3.8 27B, Gemma 4 31B y Qwen3.6 35B-A3B (con 96 GB de RAM o más entra Qwen3.8-Flash-Next como 3.ª opción).

## Cómo se publica una versión nueva

1. Comprobar en la fuente oficial el enlace, el tamaño, el SHA-256 y la licencia de cada archivo nuevo.
2. Añadir el módulo con `desde` igual a la nueva `version`, y subir `version` en uno.
3. Validar el JSON y hacer *push* a `main`. Los programas lo verán en su siguiente comprobación.
