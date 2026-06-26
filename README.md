<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d0d0d,50:1a0033,100:3d0066&height=210&section=header&text=Deadlock%20Detection%20Simulator&fontSize=34&fontColor=cc99ff&fontAlignY=36&desc=Resource%20Allocation%20Graph%20%7C%20Python%20%7C%20NetworkX%20%7C%20Tkinter&descAlignY=56&descColor=e0b3ff&animation=fadeIn" width="100%"/>

<br/>

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![NetworkX](https://img.shields.io/badge/NetworkX-Graph%20Theory-orange?style=for-the-badge)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-blue?style=for-the-badge)
![Tkinter](https://img.shields.io/badge/Tkinter-GUI-green?style=for-the-badge)
![OS Concepts](https://img.shields.io/badge/OS-Deadlock%20Detection-red?style=for-the-badge)

<br/>

> **A Python-based interactive simulator that visualizes Resource Allocation Graphs (RAG) in real-time and detects deadlocks using cycle detection algorithms — built with Tkinter, NetworkX, and Matplotlib.**

<br/>

[![Portfolio](https://img.shields.io/badge/🌐%20Portfolio-anandsavarn.vercel.app-00C896?style=for-the-badge)](https://anandsavarn.vercel.app)
[![GitHub](https://img.shields.io/badge/GitHub-Anandsavran-181717?style=for-the-badge&logo=github)](https://github.com/Anandsavran)

</div>

---

## 📘 What Is This Project?

In Operating Systems, a **deadlock** occurs when a group of processes are all waiting for resources held by each other — and none can proceed. This project **simulates that exact scenario** visually.

Using a **Resource Allocation Graph (RAG)**, the tool lets you:
- Add processes and resources dynamically
- Draw request edges (process → resource) and allocation edges (resource → process)
- Instantly detect deadlocks using **cycle detection** on the directed graph
- See everything update live in a **GUI visualization**

Think of it as a visual OS deadlock playground — built entirely in Python.

---

## 🎬 Demo — How It Works

```
1. Launch the app
2. Add Processes  →  P1, P2, P3  (shown as blue circles)
3. Add Resources  →  R1, R2      (shown as gray squares)
4. P1 requests R1 → orange edge drawn: P1 ──► R1
5. R1 allocated to P2 → green edge drawn: R1 ──► P2
6. P2 requests R2 → orange edge: P2 ──► R2
7. R2 allocated to P1 → green edge: R2 ──► P1
8. Click "Detect Deadlock"
   → DEADLOCK DETECTED: Cycle found [P1 → R1 → P2 → R2 → P1]
```

---

## 🏗️ Project Architecture

The project is split into **3 clean modules**, each with a single responsibility:

```
┌─────────────────────────────────────────────────────────┐
│                    main.py (Entry Point)                 │
└────────────┬───────────────────┬────────────────────────┘
             │                   │
     ┌───────▼──────┐   ┌───────▼──────────┐
     │   Module 1   │   │    Module 2       │
     │     GUI      │   │  GraphHandler     │
     │  (Tkinter)   │   │  (NetworkX Logic) │
     └───────┬──────┘   └───────┬──────────┘
             │                   │
             └────────┬──────────┘
                      │
              ┌───────▼──────────┐
              │    Module 3       │
              │ GraphVisualizer   │
              │ (Matplotlib/plt)  │
              └──────────────────┘
```

| Module | Class | Responsibility |
|---|---|---|
| Module 1 | `GUI` | Buttons, user input dialogs, window layout |
| Module 2 | `GraphHandler` | Graph logic, node/edge management, deadlock detection |
| Module 3 | `GraphVisualizer` | Renders the graph live inside the Tkinter window |

---

## 🔑 Key Features

### 🔄 1. Dynamic Graph Construction
Add any number of processes and resources at runtime. Each gets added as a node to a **directed graph (DiGraph)** using NetworkX. No restart needed.

### 🔗 2. Request & Allocation Edges

| Edge Type | Direction | Color | Meaning |
|---|---|---|---|
| Request | Process → Resource | 🟠 Orange | Process is **waiting** for this resource |
| Allocation | Resource → Process | 🟢 Green | Resource is **assigned** to this process |

### ❌ 3. Deadlock Detection
Uses `nx.simple_cycles()` — NetworkX's built-in **cycle detection algorithm**.

```
If a cycle exists in the RAG → DEADLOCK
If no cycle exists           → SAFE STATE
```

A cycle means: Process A is waiting for a resource held by Process B, which is waiting for a resource held by Process A (or longer chains).

### 📊 4. Live Graph Visualization
The graph redraws in real-time inside the Tkinter window using Matplotlib's embedded canvas — no separate window needed.

```
🔵 Blue Circles    →  Processes (P1, P2, P3...)
⬜ Gray Squares    →  Resources (R1, R2, R3...)
🟠 Orange Arrows   →  Request edges
🟢 Green Arrows    →  Allocation edges
```

---

## 🧠 Core Concept — How RAG Deadlock Detection Works

A **Resource Allocation Graph** is a directed graph where:
- **Nodes** = Processes + Resources
- **Edges** = Relationships (request or allocation)

**Rule:** If there is a **cycle** in the RAG, a deadlock **may exist** (for multi-instance resources) or **definitely exists** (for single-instance resources).

### Example of a Deadlock Cycle:

```
P1 ──requests──► R1 ──held by──► P2
▲                                  │
│                                  │
└────────── requests ◄── R2 ◄──── ┘

Cycle: P1 → R1 → P2 → R2 → P1   ← DEADLOCK!
```

### Example of a Safe State (No Cycle):

```
P1 ──requests──► R1 ──held by──► P2
                 
P2 ──requests──► R2  (R2 is free — no cycle)

No cycle → SAFE ✅
```

---

## 📂 Code Walkthrough

### `GUI` Class — Module 1
```python
# Creates all buttons and wires them to GraphHandler methods
self.add_process_btn    → graph_handler.add_process()
self.add_resource_btn   → graph_handler.add_resource()
self.add_request_btn    → graph_handler.request_resource()
self.add_allocation_btn → graph_handler.allocate_resource()
self.detect_deadlock_btn→ graph_handler.detect_deadlock()
self.show_graph_btn     → graph_handler.show_graph()
```

### `GraphHandler` Class — Module 2
```python
# Core data structure
self.graph = nx.DiGraph()   # Directed graph (direction matters for edges)
self.processes = set()      # Tracks all process node names
self.resources = set()      # Tracks all resource node names

# Deadlock detection — the key function
cycles = list(nx.simple_cycles(self.graph))
# If cycles list is non-empty → deadlock found
```

### `GraphVisualizer` Class — Module 3
```python
# Embeds Matplotlib inside Tkinter window
self.figure = plt.Figure(figsize=(6, 6))
self.canvas = FigureCanvasTkAgg(self.figure, master=root)

# Reads color attributes from nodes/edges to color-code the graph
node_colors = [graph.nodes[n].get('color', 'black') for n in graph.nodes]
edge_colors = [graph[u][v].get('color', 'black') for u, v in graph.edges]
```

---

## 🛠️ Installation & Setup

### Prerequisites
Make sure you have **Python 3.7+** installed.

```bash
# Check Python version
python --version
```

### Install Required Libraries

```bash
pip install networkx matplotlib
```

> **Note:** `tkinter` comes pre-installed with Python on Windows and most Linux distros. On Ubuntu/Debian:
> ```bash
> sudo apt-get install python3-tk
> ```

### Clone & Run

```bash
# Clone the repository
git clone https://github.com/Anandsavran/Deadlock-Detection-RAG.git

# Navigate into the folder
cd Deadlock-Detection-RAG

# Run the simulator
python main.py
```

---

## 🎮 How to Use — Step by Step

```
Step 1 → Click "Add Process"
         Enter: P1  →  OK
         Enter: P2  →  OK
         Enter: P3  →  OK (add as many as needed)

Step 2 → Click "Add Resource"
         Enter: R1  →  OK
         Enter: R2  →  OK

Step 3 → Click "Request Resource"
         Process: P1 | Resource: R1
         (P1 is now waiting for R1 — orange arrow appears)

Step 4 → Click "Allocate Resource"
         Resource: R1 | Process: P2
         (R1 is now held by P2 — green arrow appears)

Step 5 → Repeat Steps 3–4 to build more edges

Step 6 → Click "Detect Deadlock"
         → If a cycle exists: WARNING popup shows the cycle
         → If safe: INFO popup confirms no deadlock

Step 7 → Click "Show Graph" anytime to refresh the visualization
```

---

## 📁 Repository Structure

```
📦 Deadlock-Detection-RAG
 ┣ 📄 main.py              ← Complete source code (all 3 modules)
 ┣ 📄 README.md            ← Project documentation
 ┗ 📁 assets/
    ┗ 🖼️ demo-screenshot.png  ← App screenshot (add yours here)
```

---

## 🔮 Future Improvements

- [ ] 🟥 Highlight deadlock cycle edges in **red** when detected
- [ ] 🔁 Add **multi-instance resource** support (Banker's Algorithm)
- [ ] 💾 Export graph as PNG or save/load session state
- [ ] ↩️ Undo last action button
- [ ] 🌐 Web version using **D3.js** or **Streamlit + Pyvis**
- [ ] 🧮 Banker's Algorithm for safe state prediction before allocation

---

## 🧠 OS Concepts Covered

| Concept | Where Used |
|---|---|
| Resource Allocation Graph (RAG) | Core data structure — `nx.DiGraph()` |
| Deadlock Conditions (Hold & Wait, Circular Wait) | Simulated via request + allocation edges |
| Cycle Detection | `nx.simple_cycles()` algorithm |
| Safe vs Unsafe State | Output of deadlock detection |
| Process-Resource Interaction | GUI simulation via Add/Request/Allocate buttons |

---

## 📖 References

- [NetworkX Documentation](https://networkx.org/documentation/stable/)
- [Tkinter Python Docs](https://docs.python.org/3/library/tkinter.html)
- [Operating System Concepts — Silberschatz, Galvin](https://www.os-book.com/)
- [Deadlock Detection — GeeksForGeeks](https://www.geeksforgeeks.org/deadlock-detection-algorithm/)

---

<div align="center">

### 👨‍💻 Author

**Anand Kumar**
B.Tech – Computer Science Engineering (Data Science)
Lovely Professional University, Punjab

<br/>

[![Portfolio](https://img.shields.io/badge/🌐%20Portfolio-anandsavarn.vercel.app-00C896?style=for-the-badge)](https://anandsavarn.vercel.app)
[![GitHub](https://img.shields.io/badge/GitHub-Anandsavran-181717?style=for-the-badge&logo=github)](https://github.com/Anandsavran)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/)

<br/>

---

*⭐ Star this repo if it helped you understand deadlock detection!*

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:3d0066,50:1a0033,100:0d0d0d&height=100&section=footer" width="100%"/>

</div>
