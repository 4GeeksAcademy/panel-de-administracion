# AgentHub — Documento de Especificaciones Técnicas (SPECS.md)

---

## 1. Descripción del Producto

**AgentHub** es una plataforma SaaS B2B donde las empresas pueden alquilar agentes de IA preconfigurados, equiparlos con *skills* o habilidades avanzadas (navegación web, lectura de documentos, gestión de calendarios, ejecución de consultas SQL) y desplegarlos para automatizar tareas operativas de negocio.

El usuario objetivo de esta interfaz es el **Administrador de la Plataforma (User Admin)** — un perfil técnico/operativo dentro del equipo de AgentHub que requiere supervisar el rendimiento financiero del negocio, monitorear el estado operativo de los agentes contratados por los clientes, auditar los logs de fallas técnicas y gestionar el catálogo de habilidades disponibles en el sistema.

---

## 2. Stack Tecnológico y Restricciones

- **Estructura:** HTML5 Semántico Puro (`<aside>`, `<main>`, `<header>`, `<nav>`, `<section>`, `<table>`, `<article>`).
- **Estilos:** Tailwind CSS v3 cargado exclusivamente vía CDN. Prohibido el uso de archivos CSS personalizados y atributos `style=""` en línea.
- **Interactividad y Lógica:** JavaScript Vanilla puro (ES6+), cargado mediante un único archivo o etiquetas `<script>` en cliente.
- **Restricciones Absolutas:**
  - **Sin Frameworks JS:** Prohibido el uso de React, Vue, Angular, Svelte, Alpine.js o jQuery.
  - **Sin Build Tools:** Sin Vite, Webpack, Babel ni compiladores de Tailwind CSS.
  - **Sin Backend ni APIs:** Todos los datos deben estar **hardcodeados** directamente en cliente.
  - **Single Page Feel:** Navegación entre las 6 secciones manejada dinámicamente mediante ocultamiento y exhibición de contenedores (`hidden` / `block`).
  - **Soporte Dark Mode:** Alternancia visual completa mediante la estrategia de clase `dark` en la etiqueta raíz `<html>`.

---

## 3. Especificaciones Detalladas por Sección

### 3.1. Dashboard
1. **Cuadrícula de Métricas:** Una rejilla responsiva (1 col en móvil, 2 cols en tablet, 4 cols en escritorio) con 4 tarjetas de KPI: Ingresos Totales ($48,250.00), Pérdida por Descuentos/Cupones (-$3,120.00), Agentes Activos (142) y Agentes Fallando (5). Cada tarjeta contiene un icono SVG, una etiqueta y su valor hardcodeado.
2. **Estilos de Tarjetas de Métrica:** Las tarjetas utilizan colores de acento distintos según el tipo de métrica, bordes suavizados (`rounded-xl`), fondo adaptativo (`bg-white dark:bg-slate-800`) y sombras sutiles (`shadow-sm hover:shadow-md`).
3. **Marcador de Gráfico Semanal:** Debajo de las tarjetas, un elemento `<div>` de ancho completo (`w-full`) con altura mínima (`min-h-[280px]`), borde discontinuo (`border-2 border-dashed border-slate-300 dark:border-slate-700`), fondo atenuado y una etiqueta centrada que representa el marcador de posición para el gráfico de actividad semanal.

### 3.2. Gestión de Usuarios
1. **Tabla de Usuarios Registrados:** Tabla HTML estructurada (`<table>`) con al menos 5 filas de usuarios hardcodeados (Carlos Mendoza, Ana Rivas, Roberto Gómez, Sofia Chen, David Torres) mostrando Nombre, Email, Plan (*Basic, Pro, Enterprise*) y Badge de Estado (*Activo, Inactivo, Suspendido*).
2. **Menú Kebab de Acciones:** La última columna de cada fila contiene un dropdown de tres puntos (`⋮`) que se despliega al hacer clic, con las opciones *"Ver detalle"* y *"Eliminar"*.
3. **Modal Overlay de Detalle:** *"Ver detalle"* abre un modal `fixed` centrado sobre un fondo oscuro (*backdrop blur*). El modal exhibe el registro completo del usuario y se cierra mediante el botón de cruz `✕`, un botón *"Cerrar"* o haciendo clic en el backdrop.

### 3.3. Gestión de Agentes
1. **Listado de Agentes Registrados:** Listado con 4 agentes hardcodeados (`ApexScraper`, `LeadGenius`, `DocuParse`, `QueryBot`). Cada fila/tarjeta exhibe Nombre del Agente, Propietario (Cliente), Badge de Estado (*Activo, Inactivo, Fallando*) y lista de skills colapsada.
2. **Acordeón de Skills Colapsable:** Las skills asociadas están ocultas por defecto. Hacer clic en el control expansible las revela con una transición suave (`transition-all duration-300`); hacer clic de nuevo las colapsa.
3. **Modal de Configuración (System Prompt):** Menú `⋮` con opciones *"Configurar"* y *"Eliminar"*. *"Configurar"* abre un modal con el *System Prompt* actual del agente dentro de un `<textarea>` editable.

### 3.4. Skills (Catálogo de Habilidades)
1. **Banner Explicativo de Dominio:** Contenedor informativo destacado que explica brevemente qué es una "Skill" dentro del contexto de AgentHub (módulos de integración preconstruidos para potenciar las capacidades de los agentes).
2. **Catálogo de Skills:** Al menos 4 skills hardcodeadas (`Web Scraping`, `PDF Reader`, `SQL Generator`, `Google Calendar Sync`) con Nombre, Descripción breve y Badge numérico con la cantidad de agentes que la tienen habilitada.
3. **Acciones de Catálogo:** Dropdown `⋮` con opciones *"Ver detalle"* (modal con parámetros JSON) y *"Eliminar"*.

### 3.5. Contrataciones de Agentes (Alquileres)
1. **Tabla de Histórico de Contratos:** Tabla con 4 contratos mostrando Cliente (`Acme Corp`, `DataFlow Inc`, `Nexus SaaS`, `CyberTech Ltd`), Agente Alquilado, Skills Contratadas, Fechas de Inicio/Fin e Importe Total Pagado.
2. **Modal Desglosado de Contrato:** Menú `⋮` con la opción *"Ver detalle"* que abre un modal con la factura y desglose completo del contrato.
3. **Estructura de Precios Desglosada:** El modal de detalle contiene el precio base de alquiler del agente y la lista desglosada de skills contratadas con sus precios individuales.

### 3.6. Log de Errores
1. **Tabla de Registro de Fallas:** Al menos 6 entradas de error hardcodeadas vinculadas a los agentes del sistema, con Timestamp (`YYYY-MM-DD HH:mm:ss`), Nombre del Agente, Badge de Tipo de Error con código de color y Descripción breve.
2. **Categorización Visual por Badges:** Códigos de color por gravedad: Rojo para Crítico, Amarillo para Warning/Advertencia y Azul para Informativo.
3. **Modal de Stack Trace y Resolución:** Menú `⋮` con *"Ver detalle"* (abre modal con la traza completa de error en bloque monoespaciado) y *"Marcar como resuelto"* (cambia el estado visual de la fila).

---

## 4. Interacciones Globales (JavaScript Vanilla)

1. **Toggle de Modo Oscuro/Claro:** Botón en la barra superior que alterna la clase `dark` en la etiqueta root `<html>`. El estado seleccionado persiste durante la navegación entre secciones.
2. **Cierre de Dropdowns al Clic Exterior:** Todos los menús flotantes `⋮` se cierran automáticamente cuando el usuario hace clic en cualquier punto fuera de su área.
3. **Cierre de Modales por Backdrop:** Todos los modales overlay se cierran al hacer clic directamente sobre su fondo oscuro o traslúcido (*backdrop*).

---

## 5. Inventario de Componentes UI Reutilizables

1. **Sidebar de Navegación Persistente (`w-64`):** Menú lateral con marca, lista de las 6 secciones e indicador de sección activa.
2. **Top Header Bar:** Barra superior con barra de búsqueda simulada, Toggle de Modo Oscuro y avatar de Administrador.
3. **Tarjetas de Métricas (KPI Cards):** Tarjetas con icono, etiqueta, valor hardcodeado e indicador.
4. **Dropdown de Acciones (`⋮`):** Menú flotante contextual.
5. **Modal Overlay:** Ventana emergente sobre backdrop traslúcido (`backdrop-blur-sm`).
6. **Badges / Pills de Estado:** Etiquetas redondeadas con código de color según estado.
7. **Acordeón Colapsable de Skills:** Componente desplegable para mostrar/ocultar elementos secundarios.
8. **Toggle Switch de Modo Oscuro:** Control en header para cambiar el esquema de color.

---

## 6. Criterios de Aceptación (Condiciones Verificables)

1. **[Estructura y Commit Separado]** El archivo `SPECS.md` se encuentra commiteado de forma aislada en el repositorio antes de crear cualquier archivo HTML.
2. **[Navegación]** Hacer clic en las opciones del Sidebar conmuta entre las 6 secciones mostrando la seleccionada y ocultando las demás sin recargar la página.
3. **[Modo Oscuro]** El toggle del header aplica o remueve la clase `dark` en `<html>` de forma global y conservando el estado al navegar.
4. **[Cierre de Dropdowns]** Hacer clic fuera del área de cualquier dropdown `⋮` abierto lo cierra inmediatamente.
5. **[Cierre de Modales]** Hacer clic en el backdrop oscuro de cualquier modal activo cierra la ventana emergente.
6. **[Acordeones]** El control expansible de skills en la sección Agentes muestra y oculta la lista con transición suave.
