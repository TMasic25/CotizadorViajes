# 🚗 Calculadora de Viajes - Transportes Masic

Aplicación web profesional para calcular tarifas de transporte de pasajeros con sistema de costos operacionales integrado.

![Transportes Masic](https://hebbkx1anhila5yf.public.blob.vercel-storage.com/Logo%201-FHIi6qXGRfyVgq4j52QPmNZS7p6s3U.png)

## ✨ Características Principales

### 📊 Cálculo de Tarifas Inteligente
- **Tarifas por tramos de distancia**: Precios diferenciados según kilómetros
  - < 15 km: $350/km
  - 15-50 km: $500/km
  - 51-99 km: $700/km
  - ≥ 100 km: $1,030/km

- **Tarifas por tiempo**: Precios según duración del viaje
  - < 20 min: $100/min
  - 20-59 min: $300/min
  - ≥ 60 min: $400/min

- **Tarifa base**: $3,000 CLP (se suma siempre)
- **Extras**: Peajes y ferry/transbordador
- **Descuentos configurables**: 0%, 5%, 10%, 15%, 20%

### 💼 Costos Operacionales en Tiempo Real

La aplicación calcula automáticamente los costos operacionales de cada viaje:

| Item | Frecuencia | Costo Base | Fórmula |
|------|-----------|------------|---------|
| Diesel | Por viaje | $1,080/litro | (Km / 7) × Precio diesel |
| Cambio de Aceite | 10,000 km | $350,000 | (Km / 10,000) × Costo |
| Cambio de Pastillas | 20,000 km | $180,000 | (Km / 20,000) × Costo |
| Cambio de Neumáticos | 40,000 km | $300,000 | (Km / 40,000) × Costo |

**Beneficios:**
- Visualización instantánea del costo real de cada viaje
- Cálculo de utilidad bruta (Tarifa - Costos operacionales)
- Todos los costos son editables y personalizables

### 📋 Cotización Profesional
- Diseño profesional con branding de la empresa
- Información detallada del viaje
- Botones para compartir por WhatsApp
- Función de impresión optimizada
- Logo de la empresa integrado

### ☁️ Integración con Google Sheets
- Sincronización automática de todos los viajes
- Respaldo en la nube
- Exportación a Excel/CSV
- Historial completo con filtros

### 📱 Responsive & Mobile-First
- Funciona perfectamente en móviles, tablets y PC
- Diseño adaptativo
- Touch-friendly
- PWA ready (Progressive Web App)

## 🚀 Instalación y Uso

### Opción 1: Uso Directo (Recomendado)
1. Descarga el archivo `index.html`
2. Abre el archivo con cualquier navegador moderno
3. ¡Listo! La aplicación funciona offline

### Opción 2: Servidor Local
```bash
# Usando Python 3
python -m http.server 8000

# Usando Node.js
npx serve

# Accede a http://localhost:8000
```

### Opción 3: Hosting Web
Sube el archivo `index.html` a cualquier servicio de hosting:
- GitHub Pages
- Netlify
- Vercel
- Firebase Hosting

## ⚙️ Configuración

### 1. Personalizar Tarifas
1. Haz clic en "✏️ Editar Tarifas y Descuentos"
2. Modifica los valores según tus necesidades
3. Haz clic en "💾 Guardar Tarifas y Configuración"

### 2. Configurar Costos Operacionales
En la sección de configuración puedes editar:
- Precio del diesel (actualízalo según el mercado)
- Costo de cambio de aceite
- Costo de cambio de pastillas
- Costo de cambio de neumáticos

### 3. Conectar con Google Sheets

#### Paso 1: Crear Google Apps Script
1. Abre una nueva Google Sheet
2. Ve a **Extensiones → Apps Script**
3. Copia y pega el siguiente código:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheets()[0];
    var data = JSON.parse(e.postData.contents);
    
    if (sheet.getLastRow() === 0) {
      sheet.appendRow(['Fecha', 'Nombre Cliente', 'Origen', 'Destino', 
                       'Distancia', 'Tiempo', 'Tarifa/Km Aplicada', 
                       'Tarifa/Min Aplicada', 'Peaje', 'Ferry/Transbordador', 
                       'Subtotal', 'Descuento aplicado', 'Total a Pagar']);
    }
    
    sheet.appendRow([
      data.date, data.client || 'Sin nombre', data.origin,
      data.destination, data.distance, data.duration,
      data.rateKm, data.rateMin, data.toll || 0, data.ferry || 0,
      data.fare, data.discountAmount || 0, data.total
    ]);
    
    return ContentService.createTextOutput(
      JSON.stringify({success: true})
    ).setMimeType(ContentService.MimeType.JSON);
  } catch(error) {
    return ContentService.createTextOutput(
      JSON.stringify({success: false, error: error.toString()})
    ).setMimeType(ContentService.MimeType.JSON);
  }
}
```

#### Paso 2: Implementar como Web App
1. Guarda el script (Ctrl+S)
2. Haz clic en **Implementar → Nueva implementación**
3. Tipo: **Aplicación web**
4. Ejecutar como: **Yo**
5. Quién tiene acceso: **Cualquier persona**
6. Copia la URL generada

#### Paso 3: Configurar en la App
1. En la aplicación, haz clic en ⚙️ (Configuración)
2. Pega la URL de Google Apps Script
3. Haz clic en "💾 Guardar URL"

¡Listo! Todos tus viajes se guardarán automáticamente en Google Sheets.

## 📖 Cómo Usar

### Calcular una Tarifa
1. Ingresa el **nombre del cliente** (opcional)
2. Completa **origen** y **destino**
3. Ingresa **distancia** (km) y **tiempo** (minutos)
4. Agrega **peaje** o **ferry** si corresponde
5. Selecciona un **descuento** si aplica
6. Haz clic en "💰 Calcular Tarifa"

La aplicación mostrará:
- ✅ Tabla de **costos operacionales** del viaje
- ✅ **Resultado detallado** con tarifa final
- ✅ **Utilidad bruta** (Tarifa - Costos)
- ✅ Botón para generar **cotización profesional**

### Generar Cotización
1. Después de calcular, haz clic en "📄 Generar Cotización Profesional"
2. Opciones disponibles:
   - **📤 Compartir**: Abre WhatsApp con la cotización
   - **🖨️ Imprimir**: Imprime o guarda como PDF

### Ver Ruta
- Haz clic en "🗺️ Ver Ruta" para abrir Google Maps
- Verifica el recorrido y detecta peajes en el camino

## 💾 Datos y Almacenamiento

### Local Storage
La aplicación guarda automáticamente:
- ✅ Tarifas personalizadas
- ✅ Costos operacionales
- ✅ Descuentos configurados
- ✅ Historial de viajes (últimos 50)
- ✅ URL de Google Sheets

### Exportar Datos
- **Google Sheets**: Sincronización automática en la nube
- **Excel/CSV**: Descarga el historial completo
- **Formato**: Compatible con Excel, Numbers, Google Sheets

## 🎨 Personalización

### Cambiar Logo
Reemplaza la URL en la línea 690:
```html
<img src="TU_URL_DEL_LOGO" alt="Tu Empresa">
```

### Modificar Colores
El diseño usa variables CSS. Color principal actual: `#1B9AAA` (turquesa)

Para cambiar el color de marca, busca y reemplaza `#1B9AAA` por tu color.

### Agregar Campos
Edita el formulario HTML y la función `calculateFare()` según necesites.

## 📱 Compatibilidad

### Navegadores Soportados
- ✅ Chrome/Edge (recomendado)
- ✅ Safari (iOS/macOS)
- ✅ Firefox
- ✅ Opera

### Dispositivos
- ✅ iPhone/iPad (iOS 12+)
- ✅ Android (5.0+)
- ✅ Windows/Mac/Linux
- ✅ Tablets

## 🔒 Privacidad y Seguridad

- ❌ **No requiere servidor**: Funciona 100% en el navegador
- ❌ **No recopila datos**: Sin tracking ni analytics
- ✅ **Datos locales**: Todo se guarda en tu dispositivo
- ✅ **Control total**: Tú decides si sincronizar con Google Sheets

## 🆘 Solución de Problemas

### No se guardan los datos en Google Sheets
1. Verifica que la URL sea la de **implementación** (termina en `/exec`)
2. Asegúrate de que "Quién tiene acceso" sea **Cualquier persona**
3. Revisa que los permisos estén autorizados

### La aplicación no carga
1. Verifica que JavaScript esté habilitado
2. Usa un navegador moderno actualizado
3. Limpia caché y cookies

### Los cálculos son incorrectos
1. Ve a "✏️ Editar Tarifas"
2. Verifica que los valores sean correctos
3. Guarda los cambios

## 🤝 Contribuciones

¿Quieres mejorar la aplicación? ¡Las contribuciones son bienvenidas!

1. Fork el repositorio
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📝 Changelog

### v2.0.0 (2024)
- ✨ **NUEVO**: Tabla de costos operacionales en tiempo real
- ✨ **NUEVO**: Cálculo de utilidad bruta automático
- ✨ **NUEVO**: Costos de diesel, aceite, pastillas y neumáticos
- ✨ Tarifas editables por tramos de distancia y tiempo
- ✨ Sistema de descuentos configurables
- ✨ Campo para ferry/transbordador
- ✨ Diseño renovado con paleta Transportes Masic
- 🔧 Mejoras en la interfaz móvil
- 🔧 Optimización de rendimiento

### v1.0.0 (2024)
- 🎉 Lanzamiento inicial
- Cálculo básico de tarifas
- Integración con Google Sheets
- Cotización profesional

## 📄 Licencia

Este proyecto es de uso privado para **Transportes Masic**. 

Todos los derechos reservados © 2024 Transportes Masic

## 👨‍💻 Soporte

¿Necesitas ayuda? Contacta a:
- 📧 Email: [transportesmasic@transportesmasic.cl]
- 📱 WhatsApp: [+56 9 6861 7845]
- 🌐 Web: [www.transportesmasic.cl]

---

**Desarrollado con ❤️ para Transportes Masic**

🚗 Calculando viajes, optimizando ganancias
