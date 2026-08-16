# ☀️ Buscador de Clima
Aplicación web construida con **React** y **TypeScript** que consulta el clima en tiempo real de una ciudad a partir de la API de **OpenWeatherMap**. La respuesta de la API se valida en tiempo de ejecución con **Zod**, garantizando que los datos tengan la forma esperada antes de mostrarlos en pantalla.

## 🌐 Demo
🔗 [https://current-weather-andresmdevco.vercel.app/](https://current-weather-andresmdevco.vercel.app/)

## 🛠️ Tecnologías Utilizadas
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)
- React 19
- CSS Modules

Además:
- **Zod** — validación en tiempo de ejecución de la respuesta de la API
- **Axios** — cliente HTTP para consumir la API de OpenWeatherMap

## ✨ Características
- 🔎 Búsqueda del clima actual por ciudad y país, seleccionando el país desde un listado predefinido.
- ✅ Validación de formulario: ciudad y país son obligatorios antes de consultar.
- 🚫 Mensaje de "Ciudad no Encontrada" cuando la búsqueda no arroja resultados.
- ⏳ Indicador de carga (spinner) mientras se consulta la API.
- 🌡️ Visualización de la temperatura actual, mínima y máxima en grados Celsius.
- 🔑 Uso de variable de entorno para la API key, sin exponerla en el código.
- 🎨 Interfaz estilizada con CSS Modules e imagen de fondo.

## 📂 Archivos principales
| Archivo | Descripción |
|---|---|
| `App.tsx` | Componente raíz. Consume `useWeather` y renderiza condicionalmente `Form`, `Spinner`, `WeatherDetail` y `Alert` según el estado de la búsqueda |
| `hooks/useWeather.ts` | Hook principal de la app. Define el esquema de Zod (`Weather`), realiza las dos llamadas a la API (geocodificación y clima) y expone `weather`, `loading`, `notFound`, `fetchWeather` y `hasWeatherData` |
| `components/Form/Form.tsx` | Formulario controlado para capturar ciudad y país; valida que ambos campos estén completos antes de invocar `fetchWeather` |
| `components/WeatherDetail/WeatherDetail.tsx` | Muestra el nombre de la ciudad y las temperaturas actual, mínima y máxima formateadas |
| `components/Spinner/Spinner.tsx` | Indicador de carga mostrado mientras se resuelve la petición a la API |
| `components/Alert/Alert.tsx` | Componente reutilizable para mostrar mensajes de alerta (validación del formulario, ciudad no encontrada) |
| `data/countries.ts` | Catálogo de países (`code`, `name`) usado en el selector del formulario |
| `utils/index.ts` | Función `formatTemperature` usada para formatear la temperatura antes de mostrarla |
| `types/index.ts` | Tipos `SearchType`, `Country` y `Weather` compartidos por la app |

## 🧠 Cómo funciona
1. `Form` captura `city` y `country`; si algún campo está vacío, muestra una `Alert` y no continúa.
2. Al enviarse el formulario, se invoca `fetchWeather` desde `useWeather`, que reinicia el estado (`weather`, `notFound`) y activa `loading`.
3. `useWeather` hace una primera petición a la **API de geocodificación** de OpenWeatherMap para obtener la latitud y longitud de la ciudad indicada.
4. Si la ciudad no existe, se activa `notFound` y `App.tsx` muestra la alerta "Ciudad no Encontrada".
5. Con las coordenadas obtenidas, se hace una segunda petición a la **API del clima**.
6. La respuesta se valida con el esquema `Weather` de **Zod** mediante `safeParse`; solo si la validación es exitosa (`result.success`) se actualiza el estado `weather` con `result.data`, ya tipado según el esquema (`z.infer<typeof Weather>`).
7. `hasWeatherData` (derivado con `useMemo` a partir de `weather.name`) determina si `App.tsx` debe renderizar `WeatherDetail` con los datos obtenidos.
8. `WeatherDetail` muestra la temperatura actual, mínima y máxima, formateadas mediante la función `formatTemperature`.

## 📚 Conceptos aplicados
- Validación de esquemas en tiempo de ejecución con **Zod** (`z.object`, `z.infer`, `safeParse`) para verificar datos que provienen de una fuente externa (la API), algo que TypeScript por sí solo no puede garantizar.
- Consumo de una API externa encadenando dos peticiones con **Axios** (geocodificación → clima).
- Manejo de variables de entorno con Vite (`import.meta.env.VITE_API_KEY`).
- Manejo de estados de una petición asíncrona: `loading`, `notFound` y datos obtenidos.
- Estado derivado con `useMemo` (`hasWeatherData`) para controlar el renderizado condicional.
- Formularios controlados y validación de campos obligatorios.
- Tipado de props, funciones y esquemas con TypeScript.
- Estilizado por componente con **CSS Modules**.
- Organización del proyecto por responsabilidades (`components`, `hooks`, `data`, `types`, `utils`).

## 🚀 Cómo ejecutar el proyecto
1. Clonar el repositorio:
```bash
   git clone https://github.com/andresmdevco/weather-app.git
   cd weather-app
```
2. Instalar las dependencias:
```bash
   npm install
```
3. Crear un archivo `.env` en la raíz del proyecto con tu API key de OpenWeatherMap:
```bash
   VITE_API_KEY=tu_api_key_aqui
```
4. Ejecutar el proyecto en modo desarrollo:
```bash
   npm run dev
```
5. Abrir [http://localhost:5173](http://localhost:5173) en el navegador