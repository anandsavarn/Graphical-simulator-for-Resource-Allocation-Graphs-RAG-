

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d0d0d,50:1a0033,100:3d0066&height=210&section=header&text=Deadlock%20Detection%20Simulator&fontSize=34&fontColor=cc99ff&fontAlignY=36&desc=Resource%20Allocation%20Graph%20%7C%20Python%20%7C%20NetworkX%20%7C%20Tkinter&descAlignY=56&descColor=e0b3ff&animation=fadeIn" width="100%"/>

<br/>

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![NetworkX](https://img.shields.io/badge/NetworkX-Graph%20Theory-orange?style=for-the-badge)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-blue?style=for-the-badge)
![Tkinter](https://img.shields.io/badge/Tkinter-GUI-green?style=for-the-badge)
![OS Concepts](https://img.shields.io/badge/OS-Deadlock%20Detection-red?style=for-the-badge)

<br/>

> **Un simulador interactivo basado en Python que visualiza Grafos de Asignación de Recursos (RAG) en tiempo real y detecta interbloqueos mediante algoritmos de detección de ciclos — construido con Tkinter, NetworkX y Matplotlib.**

<br/>

[![Portfolio](https://img.shields.io/badge/🌐%20Portfolio-anandsavarn.vercel.app-00C896?style=for-the-badge)](https://anandsavarn.vercel.app)
[![GitHub](https://img.shields.io/badge/GitHub-Anandsavran-181717?style=for-the-badge&logo=github)](https://github.com/Anandsavarn)

</div>

---

## 📘 ¿Qué es este proyecto?

En los Sistemas Operativos, un **interbloqueo (deadlock)** ocurre cuando un grupo de procesos están esperando recursos que poseen mutuamente y ninguno puede continuar. Este proyecto **simula ese escenario exacto** de forma visual.

Utilizando un **Grafo de Asignación de Recursos (RAG)**, la herramienta te permite:
- Agregar procesos y recursos dinámicamente
- Dibujar aristas de solicitud (proceso → recurso) y aristas de asignación (recurso → proceso)
- Detectar interbloqueos al instante mediante **detección de ciclos** en el grafo dirigido
- Ver todo actualizarse en vivo en una **visualización con interfaz gráfica (GUI)**

Piénsalo como un campo de pruebas visual para interbloqueos de sistemas operativos, construido completamente en Python.

---

## 🎬 Demostración — ¿Cómo funciona?

```
1. Inicia la aplicación
2. Agrega Procesos  →  P1, P2, P3  (mostrados como círculos azules)
3. Agrega Recursos  →  R1, R2      (mostrados como cuadrados grises)
4. P1 solicita R1 → arista naranja dibujada: P1 ──► R1
5. R1 asignado a P2 → arista verde dibujada: R1 ──► P2
6. P2 solicita R2 → arista naranja: P2 ──► R2
7. R2 asignado a P1 → arista verde: R2 ──► P1
8. Haz clic en "Detectar Interbloqueo"
   → INTERBLOQUEO DETECTADO: Ciclo encontrado [P1 → R1 → P2 → R2 → P1]
```

---

## 🏗️ Arquitectura del Proyecto

El proyecto está dividido en **3 módulos claros**, cada uno con una responsabilidad única:

```
┌─────────────────────────────────────────────────────────┐
│                   main.py (Punto de entrada)             │
└────────────┬───────────────────┬────────────────────────┘
             │                   │
     ┌───────▼──────┐   ┌───────▼──────────┐
     │   Módulo 1   │   │    Módulo 2       │
     │     GUI      │   │  GraphHandler     │
     │  (Tkinter)   │   │  (Lógica NetworkX) │
     └───────┬──────┘   └───────┬──────────┘
             │                   │
             └────────┬──────────┘
                      │
              ┌───────▼──────────┐
              │    Módulo 3       │
              │ GraphVisualizer   │
              │ (Matplotlib/plt)  │
              └──────────────────┘
```

| Módulo | Clase | Responsabilidad |
|---|---|---|
| Módulo 1 | `GUI` | Botones, diálogos de entrada de usuario, diseño de ventana |
| Módulo 2 | `GraphHandler` | Lógica del grafo, gestión de nodos/aristas, detección de interbloqueos |
| Módulo 3 | `GraphVisualizer` | Renderiza el grafo en vivo dentro de la ventana de Tkinter |

---

## 🔑 Características Principales

### 🔄 1. Construcción dinámica de grafos
Agrega cualquier cantidad de procesos y recursos en tiempo de ejecución. Cada uno se añade como un nodo a un **grafo dirigido (DiGraph)** utilizando NetworkX. No requiere reiniciar la aplicación.

### 🔗 2. Aristas de solicitud y asignación

| Tipo de arista | Dirección | Color | Significado |
|---|---|---|---|
| Solicitud | Proceso → Recurso | 🟠 Naranja | El proceso está **esperando** este recurso |
| Asignación | Recurso → Proceso | 🟢 Verde | El recurso está **asignado** a este proceso |

### ❌ 3. Detección de interbloqueos
Utiliza `nx.simple_cycles()`: el algoritmo integrado de **detección de ciclos** de NetworkX.

```
Si existe un ciclo en el RAG → INTERBLOQUEO
Si no existe un ciclo        → ESTADO SEGURO
```

Un ciclo significa: El Proceso A está esperando un recurso retenido por el Proceso B, que a su vez espera un recurso retenido por el Proceso A (o cadenas más largas).

### 📊 4. Visualización en vivo del grafo
El grafo se redibuja en tiempo real dentro de la ventana de Tkinter utilizando el lienzo incrustado de Matplotlib, sin necesidad de abrir ventanas adicionales.

```
🔵 Círculos azules    →  Procesos (P1, P2, P3...)
⬜ Cuadrados grises   →  Recursos (R1, R2, R3...)
🟠 Flechas naranjas   →  Aristas de solicitud
🟢 Flechas verdes     →  Aristas de asignación
```

---

## 🧠 Concepto principal — Cómo funciona la detección de interbloqueos con RAG

Un **Grafo de Asignación de Recursos (RAG)** es un grafo dirigido donde:
- **Nodos** = Procesos + Recursos
- **Aristas** = Relaciones (solicitud o asignación)

**Regla:** Si hay un **ciclo** en el RAG, un interbloqueo **puede existir** (para recursos de múltiples instancias) o **existe definitivamente** (para recursos de instancia única).

### Ejemplo de un ciclo de interbloqueo:

```
P1 ──solicita──► R1 ──retenido por──► P2
▲                                     │
│                                     │
└───────── solicita ◄── R2 ◄───── ┘

Ciclo: P1 → R1 → P2 → R2 → P1   ← ¡INTERBLOQUEO!
```

### Ejemplo de un estado seguro (sin ciclo):

```
P1 ──solicita──► R1 ──retenido por──► P2
               
P2 ──solicita──► R2  (R2 está libre — sin ciclo)

Sin ciclo → SEGURO ✅
```

---

## 📂 Recorrido del código

### `GUI` Class — Módulo 1
```python
# Crea todos los botones y los conecta a los métodos de GraphHandler
self.add_process_btn    → graph_handler.add_process()
self.add_resource_btn   → graph_handler.add_resource()
self.add_request_btn    → graph_handler.request_resource()
self.add_allocation_btn → graph_handler.allocate_resource()
self.detect_deadlock_btn→ graph_handler.detect_deadlock()
self.show_graph_btn     → graph_handler.show_graph()
```

### `GraphHandler` Class — Módulo 2
```python
# Estructura de datos principal
self.graph = nx.DiGraph()   # Grafo dirigido (la dirección importa para las aristas)
self.processes = set()      # Rastrea todos los nombres de nodos de procesos
self.resources = set()      # Rastrea todos los nombres de nodos de recursos

# Detección de interbloqueos: la función clave
cycles = list(nx.simple_cycles(self.graph))
# Si la lista de ciclos no está vacía → se encontró un interbloqueo
```

### `GraphVisualizer` Class — Módulo 3
```python
# Incorpora Matplotlib dentro de la ventana de Tkinter
self.figure = plt.Figure(figsize=(6, 6))
self.canvas = FigureCanvasTkAgg(self.figure, master=root)

# Lee los atributos de color de los nodos/aristas para codificar por colores el grafo
node_colors = [graph.nodes[n].get('color', 'black') for n in graph.nodes]
edge_colors = [graph[u][v].get('color', 'black') for u, v in graph.edges]
```

---

## 🛠️ Instalación y configuración

### Prerequisitos
Asegúrate de tener **Python 3.7+** instalado.

```bash
# Verificar versión de Python
python --version
```

### Instalar bibliotecas requeridas

```bash
pip install networkx matplotlib
```

> **Nota:** `tkinter` viene preinstalado con Python en Windows y la mayoría de las distribuciones de Linux. En Ubuntu/Debian:
> ```bash
> sudo apt-get install python3-tk
> ```

### Clonar y ejecutar

```bash
# Clonar el repositorio
git clone https://github.com/Anandsavran/Deadlock-Detection-RAG.git

# Navegar al directorio
cd Deadlock-Detection-RAG

# Ejecutar el simulador
python main.py
```

---

## 🎮 Cómo usarlo — Paso a paso

```
Paso 1 → Haz clic en "Agregar Proceso"
         Ingresa: P1  →  OK
         Ingresa: P2  →  OK
         Ingresa: P3  →  OK (agrega tantos como necesites)

Paso 2 → Haz clic en "Agregar Recurso"
         Ingresa: R1  →  OK
         Ingresa: R2  →  OK

Paso 3 → Haz clic en "Solicitar Recurso"
         Proceso: P1 | Recurso: R1
         (P1 ahora espera R1 — aparece una flecha naranja)

Paso 4 → Haz clic en "Asignar Recurso"
         Recurso: R1 | Proceso: P2
         (R1 ahora es retenido por P2 — aparece una flecha verde)

Paso 5 → Repite los pasos 3–4 para crear más aristas

Paso 6 → Haz clic en "Detectar Interbloqueo"
         → Si existe un ciclo: el cuadro de advertencia muestra el ciclo
         → Si es seguro: el cuadro informativo confirma que no hay interbloqueo

Paso 7 → Haz clic en "Mostrar Grafo" en cualquier momento para actualizar la visualización
```

---

## 📁 Estructura del repositorio

```
📦 Deadlock-Detection-RAG
 ┣ 📄 main.py              ← Código fuente completo (los 3 módulos)
 ┣ 📄 README.md            ← Documentación del proyecto
 ┗ 📁 assets/
    ┗ 🖼️ demo-screenshot.png  ← Captura de pantalla de la app (agrega la tuya aquí)
```

---

## 🔮 Mejoras futuras

- [ ] 🟥 Resaltar las aristas del ciclo de interbloqueo en **rojo** cuando se detecte
- [ ] 🔁 Agregar soporte para **recursos de múltiples instancias** (Algoritmo del Banquero)
- [ ] 💾 Exportar el grafo como PNG o guardar/cargar el estado de la sesión
- [ ] ↩️ Botón para deshacer la última acción
- [ ] 🌐 Versión web utilizando **D3.js** o **Streamlit + Pyvis**
- [ ] 🧮 Algoritmo del Banquero para predecir el estado seguro antes de la asignación

---

## 🧠 Conceptos de Sistemas Operativos cubiertos

| Concepto | Dónde se usa |
|---|---|
| Grafo de Asignación de Recursos (RAG) | Estructura de datos principal — `nx.DiGraph()` |
| Condiciones de interbloqueo (Retención y espera, Espera circular) | Simuladas mediante aristas de solicitud y asignación |
| Detección de ciclos | Algoritmo `nx.simple_cycles()` |
| Estado seguro vs inseguro | Resultado de la detección de interbloqueos |
| Interacción Proceso-Recurso | Simulación en la GUI mediante botones Agregar/Solicitar/Asignar |

---

## 📖 Referencias

- [Documentación de NetworkX](https://networkx.org/documentation/stable/)
- [Documentación de Tkinter en Python](https://docs.python.org/3/library/tkinter.html)
- [Conceptos de Sistemas Operativos — Silberschatz, Galvin](https://www.os-book.com/)
- [Detección de interbloqueos — GeeksForGeeks](https://www.geeksforgeeks.org/deadlock-detection-algorithm/)

---

<div align="center">

### 👨‍💻 Autor

**Anand Kumar**
B.Tech – Computer Science Engineering (Data Science)
Lovely Professional University, Punjab

<br/>

[![Portfolio](https://img.shields.io/badge/🌐%20Portfolio-anandsavarn.vercel.app-00C896?style=for-the-badge)](https://anandsavarn.vercel.app)
[![GitHub](https://img.shields.io/badge/GitHub-Anandsavarn-181717?style=for-the-badge&logo=github)](https://github.com/Anandsavarn)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/)

<br/>

---

*⭐ Dale una estrella a este repositorio si te ayudó a entender la detección de interbloqueos!*

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:3d0066,50:1a0033,100:0d0d0d&height=100&section=footer" width="100%"/>

</div>
