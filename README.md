[handoff-rendicion-gastos.md](https://github.com/user-attachments/files/34285927/handoff-rendicion-gastos.md)
# HANDOFF — Proyecto "Rendición de Gastos" (Gobantes)

> Pega este documento al inicio de la conversación en la cuenta nueva y adjunta el archivo `index_2_8.html`.
> La cuenta nueva no comparte memoria ni historial: todo lo necesario está aquí.
> Fecha del handoff: 02-11-2026 · Responsable: Marco (TI, Gobantes Materiales Eléctricos, Santiago de Chile).

## 1. Qué es y estado actual
Aplicación web de **un solo archivo HTML** (JS vanilla, sin servidor) para rendir gastos con boletas y facturas chilenas.
Se fotografía el documento (o se importa un Excel), la IA de Anthropic extrae los datos, se editan en una tabla y se exporta a PDF, Excel o impresión.

- **Versión vigente:** `index_2_8.html` — carpeta local `C:\Proyecto MVP\Rendicion de gatos`
- Versiones anteriores en la misma carpeta: `index_2_6.html` (base con Excel/PDF), `index_2_7.html` (colores pastel Gobantes + "Firma autorizador").
- Regla: cada cambio se hace sobre una copia nueva (`index_2_X.html`); nunca se sobrescribe la versión anterior.

## 2. Historial de lo hecho
1. **Jun-2026:** primera versión (artifact React) con visión de Claude para boletas chilenas; luego HTML autónomo, con Excel (SheetJS, hojas Detalle y Resumen). Se corrigió el botón de cámara (usar `<label for>` + `<input capture="environment">`, no `.click()` programático).
2. **Pendiente histórico:** publicar en GitHub Pages para tener URL fija (Marco no recordaba sus credenciales; no se creó repositorio).
3. **Oct-2026 (`index_2_6` → `2_7`):** paleta pastel con colores de Gobantes en pantalla, PDF e impresión; bloque **"Firma autorizador"** en PDF, impresión y hoja Resumen del Excel.
4. **Nov-2026 (`index_2_7` → `2_8`):** botón **"Ordenar por fecha"** (ver §4).

## 3. Especificación (conocimiento clave)
**Funciones:** carga/cámara/arrastrar imágenes; extracción con IA; importar Excel/CSV (.xlsx, .xls, .csv, .ods) y plantilla vacía; tabla editable; totales; exportar Excel; generar PDF; imprimir; lightbox; limpiar con confirmación.

**Columnas:** PROVEEDOR, FOLIO, RUT, FECHA, CATEGORÍA, TIPO, NETO, IVA, TOTAL.
**Categorías:** Transporte, Alimentación, Alojamiento, Materiales, Servicios, Combustible, Otro (la IA deja `Otro`; el usuario cambia). **Tipos:** Boleta, Factura.
**Moneda:** pesos chilenos enteros, formato es-CL.

**IA:** `POST https://api.anthropic.com/v1/messages` con `x-api-key`, `anthropic-version: 2023-06-01`, `anthropic-dangerous-direct-browser-access: true`; modelo `claude-sonnet-4-6`, `max_tokens 500`. Debe responder solo JSON: `proveedor, folio, rut (XX.XXX.XXX-X), fecha (DD/MM/YYYY), neto, iva, total` (enteros). La API key la ingresa el usuario y se guarda solo en `localStorage` (`gobantes_anthropic_key`); no es necesaria para importar Excel ni generar PDF.

**Librerías (CDN):** `xlsx@0.18.5`, `jspdf@2.5.1` (cdnjs); fuentes DM Mono y Playfair Display.

**Identidad visual:** rojo del logo `#c21632`, gris carbón `#3f414b`, fondo `#fdf5f5`, rosas pastel `#fbe8ea` / `#f4c2c8`, rojo oscuro `#7d0e20`, verde pastel `#5fae8a` / `#e3f4ec`. Todo definido con variables CSS en `:root`.

## 4. Reglas de decisión vigentes
- Archivo único, sin frameworks ni build. Cámara con `<label>`/`input capture`, nunca `.click()` programático.
- Sin API key → se abre el modal; una imagen con error queda en estado `error` sin bloquear las demás.
- Importación Excel: detecta encabezados sin importar tildes/mayúsculas y con sinónimos; ignora filas "TOTALES".
- Totales siempre calculados desde las filas.
- **Firma autorizador** obligatoria en PDF (abajo a la derecha, pasa a página nueva si no cabe), impresión (`#firma-autorizador`, solo en `@media print`) y hoja Resumen del Excel.
- PDF: A4 horizontal, cabecera pastel con línea roja, tabla, TOTALES, resumen por categoría, "Página X de Y".
- **Ordenar por fecha:** botón en la barra de acciones. Primer clic = más antigua primero; segundo clic = más reciente primero (alterna). Reordena `state.rows` en sitio, así el PDF, el Excel y la impresión salen en ese orden. Las filas sin fecha o con fecha ilegible van siempre al final. Entiende `DD/MM/YYYY`, `DD-MM-YYYY`, `DD.MM.YYYY`, `YYYY-MM-DD` y años de 2 dígitos (funciones `fechaKey()` y `sortByFecha()`).
- Ante ambigüedad: opción más simple, avisarlo en una línea, sin agregar funciones no pedidas.

## 5. Control de calidad (lo que se verificó y lo que no)
Verificado con navegador headless: interfaz pastel sin restos de negro/dorado; PDF con firma bien ubicada; orden por fecha con formatos mixtos y filas sin fecha (asc y desc), sin errores de consola.
**No verificado:** extracción real con IA (requiere API key real), impresión desde el navegador, Excel exportado con la firma, importación de Excel real, revisión en celular.
Checklist para cambios futuros: sin errores de consola; totales cuadran; captura de pantalla; PDF de prueba (también con muchas filas); Excel de dos hojas; importación con tildes y fila TOTALES; ancho móvil ≈390 px sin scroll horizontal de página.

## 6. Pendientes / ideas
- Publicar en GitHub Pages (URL fija, instalable en celular).
- Probar con boletas reales y una API key real.
- Opcional: campo hora y orden por hora (existió en la versión de junio, no en la actual).
- Opcional: que el orden por fecha se aplique automáticamente al exportar.

## 7. Cómo continuar en la cuenta nueva
1. Adjunta `index_2_8.html` y este documento.
2. Si trabajas con acceso a la carpeta local, usa `C:\Proyecto MVP\Rendicion de gatos` y guarda el resultado como `index_2_9.html`.
3. Pide primero lo que quieras cambiar; trabaja sobre una copia y verifica con el checklist del §5.
