# Contactos NFC – Asesores

Cada asesor tiene un link secreto a su tarjeta de contacto (`.vcf`), que se graba en un tag NFC.
Al acercar un celular (iPhone o Android) al tag, se abre el contacto listo para guardar.

## Seguridad con repo público

- El repo solo contiene **código**. Los datos de los asesores viven en el secreto **ASESORES_CSV**, que nadie puede leer (ni siquiera los administradores: solo se reemplaza).
- Los `.vcf` **no se guardan en el repo**. Se publican directo en GitHub Pages con nombres aleatorios de 16 caracteres, sin página índice y con `robots.txt` que bloquea buscadores.
- La página `grabar.html` no contiene datos: calcula los links en el celular con el CSV y la clave, sin enviarlos a ningún lado.
- Los errores no muestran datos personales en los logs (son públicos).
- **Nunca** subas el CSV al repo. Guarda el archivo maestro en tu PC o en OneDrive/SharePoint de la empresa.

## Configuración inicial (una sola vez)

1. Sube al repo `README.md`, `.gitignore`, las carpetas `scripts` y `web`, y `.github/workflows/publicar.yml`.
2. **Settings → Secrets and variables → Actions → New repository secret**:
   - `NFC_SECRETO`: una frase larga y aleatoria. Guárdala: la pide la página grabar.html y **si cambia, cambian todos los links**.
   - `ASESORES_CSV`: abre tu CSV con el Bloc de notas, copia **todo** el texto y pégalo aquí.
3. **Settings → Pages → Source: GitHub Actions**.
4. **Actions → Publicar contactos NFC → Run workflow**.

## Grabar tags (desde el celular)

1. En Chrome (Android) abre `https://itkcolombia.github.io/nfc-contactos/grabar.html`.
2. Escribe la clave `NFC_SECRETO` y carga el CSV.
3. Toca **Grabar tag** en el asesor y acerca el tag. En iPhone: **Copiar link** y grábalo con NFC Tools (URL/URI).
4. Al tocar el tag, el cliente ve la tarjeta del asesor con **Guardar contacto**, Llamar, WhatsApp y Correo.

## Actualizar asesores

1. Edita el CSV en tu PC.
2. **Settings → Secrets and variables → Actions → ASESORES_CSV → lápiz** → pega el contenido completo → **Update secret**.
3. **Actions → Publicar contactos NFC → Run workflow**.

Los asesores que ya tenían tag conservan su link. Solo graba los nuevos.

## Formato del CSV

`EMPRESA;NOMBRE;CELULAR;EMAIL` (también acepta `,`). Columna opcional `CODIGO` (cédula o código interno) para que el link no dependa del email.
A los celulares sin indicativo se les agrega `+57`. Límite del secreto: 48 KB (aprox. 500 asesores).

## Recomendaciones

- Después de probar cada tag, **bloquéalo** en NFC Tools (Otros → Bloquear etiqueta). Es irreversible.
- Para desactivar a un asesor que se retiró, bórralo del CSV, actualiza el secreto y ejecuta de nuevo.
