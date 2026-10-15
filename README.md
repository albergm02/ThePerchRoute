<div align="center">

# 🧠 ThePerchRoute

**Simulación en C++ de problemas clásicos de concurrencia: filósofos comensales, barbero dormilón y más**

[![C++](https://img.shields.io/badge/C%2B%2B-17+-00599C?style=flat&logo=cplusplus&logoColor=white)](https://isocpp.org/)
[![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?style=flat&logo=windows&logoColor=white)](https://www.microsoft.com/windows)
[![Build](https://img.shields.io/badge/Build-CMake-064F8C?style=flat&logo=cmake&logoColor=white)](https://cmake.org/)
[![Status](https://img.shields.io/badge/Status-Completado-1F7A8C?style=flat)]()

[Descripción](#-sobre-el-proyecto) · [Problemas](#-problemas-de-concurrencia) · [Capturas](#-capturas) · [Cómo ejecutar](#-cómo-ejecutar) · [Estrategias](#-estrategias-de-sincronización) · [Stack](#-stack-tecnológico)

</div>

---

## 📖 Sobre el proyecto

**ThePerchRoute** es una aplicación de consola en **C++** que implementa varios de los **problemas clásicos de concurrencia** de los sistemas operativos. El objetivo es entender, desde dentro, cómo se coordinan múltiples hilos que compiten por recursos compartidos, y por qué la sincronización es uno de los temas más delicados de la programación concurrente.

El proyecto no se limita a resolver un único problema: **compara varias estrategias** de sincronización para cada uno, mostrando el impacto real de cada decisión sobre el rendimiento, la equidad y la ausencia de bloqueos.

> 📌 **Proyecto académico** desarrollado para la asignatura de *Sistemas Operativos* durante la Ingeniería Informática.

---

## 🧩 Problemas de concurrencia

### 🍝 Filósofos comensales

Cinco filósofos se sientan alrededor de una mesa circular y alternan entre **pensar** y **comer**. Para comer necesitan **dos tenedores** (el de su izquierda y el de su derecha), pero solo hay cinco tenedores en total.

El desafío es implementar soluciones que eviten:
- **Deadlock**: todos los filósofos cogen el tenedor izquierdo a la vez y esperan eternamente por el derecho.
- **Starvation**: un filósofo nunca consigue comer porque los demás siempre se le adelantan.
- **Baja concurrencia**: solo un filósofo come a la vez aunque haya recursos para más.

### 💈 Barbero dormilón

Una barbería tiene un barbero, una silla de corte y un número limitado de sillas de espera. Si no hay clientes, el barbero **se duerme**. Cuando llega un cliente:
- Si el barbero está dormido, lo despierta.
- Si el barbero está ocupado y hay sillas libres, espera.
- Si no hay sillas libres, el cliente se va.

El reto es coordinar el sueño y despertar del barbero con la llegada variable de clientes.

### 📖 Lectores-escritores

Múltiples hilos compiten por un recurso compartido: los **lectores** pueden acceder simultáneamente, pero los **escritores** necesitan acceso exclusivo. El problema explora distintas políticas de prioridad (lectores, escritores o equitativa) y su impacto en la latencia.

---

## ✨ Características

- 🧵 **Gestión explícita de hilos** con `std::thread`.
- 🔒 **Sincronización variada**: mutex, semáforos, variables de condición y primitivas Win32.
- 🎯 **Múltiples estrategias por problema**: comparación entre soluciones ingenuas y soluciones robustas.
- 🖥️ **Visualización por consola** del estado de cada proceso en cada iteración.
- 🧪 **Prevención de deadlocks** mediante asimetría, arbitraje y jerarquía de recursos.
- ⚙️ **Compilación con CMake** y compatible con `g++` y MSVC.


## 🚀 Cómo ejecutar

### Requisitos

- **Windows 10/11** (usa primitivas Win32 como `CRITICAL_SECTION` y `CreateEvent`).
- **Compilador C++17 o superior**: MSVC, MinGW o g++.
- **CMake 3.15+** (opcional, si se usa el build system).

### Compilar con g++

```bash
g++ -std=c++17 -pthread main.cpp -o philosophers
./philosophers
```

### Compilar con CMake

```bash
mkdir build && cd build
cmake ..
cmake --build .
./ThePerchRoute
```

### Compilar con Visual Studio

1. Abre el archivo `.sln` (si está incluido) o crea un proyecto nuevo.
2. Añade todos los `.cpp` y `.h`.
3. Compila con `Ctrl + Shift + B` y ejecuta con `F5`.

---

## 🧠 Estrategias de sincronización

El proyecto **no se queda con una única solución**: implementa y compara varias aproximaciones a cada problema.

### Filósofos comensales

| Estrategia | Descripción | Ventajas | Inconvenientes |
| :--- | :--- | :--- | :--- |
| **Ingenua** | Cada filósofo coge el izquierdo y luego el derecho | Simple | **Provoca deadlock** |
| **Asimetría** | Los filósofos pares cogen primero el izquierdo, los impares el derecho | Rompe el ciclo de espera | No garantiza equidad |
| **Arbitraje** | Un semáforo adicional actúa como árbitro (máximo 4 filósofos a la mesa) | Sin deadlock, sencillo | Reduce concurrencia |
| **Estado + variables de condición** | Cada filósofo tiene un estado (`pensando`, `hambriento`, `comiendo`) y solo coge los tenedores si los vecinos no comen | Máxima concurrencia, sin deadlock, sin starvation | Más compleja de implementar |

### Barbero dormilón

- Uso de **variables de condición** (`std::condition_variable`) para dormir y despertar al barbero.
- Cola de espera implementada con semáforos contadores.
- Clientes rechazados notificados por consola cuando no hay sillas.

### Lectores-escritores

- Tres variantes: **prioridad lectores**, **prioridad escritores** y **equitativa**.
- Uso de `std::shared_mutex` en la versión moderna para permitir concurrencia real entre lectores.

---

## 🛠️ Stack tecnológico

| Categoría | Tecnología |
| :--- | :--- |
| **Lenguaje** | C++17 |
| **Concurrencia estándar** | `std::thread`, `std::mutex`, `std::condition_variable`, `std::counting_semaphore` |
| **Concurrencia Win32** | `CRITICAL_SECTION`, `CreateEvent`, `WaitForSingleObject` |
| **Build** | CMake, g++, MSVC |
| **IDE** | Visual Studio Code / Visual Studio |
| **Sistema** | Windows (primitivas Win32) |

---

## 📂 Estructura del proyecto

```
ThePerchRoute/
├── src/
│   ├── philosophers/
│   │   ├── naive.cpp           # Solución ingenua (deadlock)
│   │   ├── asymmetric.cpp      # Asimetría en la adquisición
│   │   ├── arbiter.cpp         # Arbitraje con semáforo
│   │   └── state-based.cpp     # Solución con variables de condición
│   ├── barber/
│   │   └── sleeping-barber.cpp
│   ├── readers-writers/
│   │   └── readers-writers.cpp
│   └── main.cpp                # Menú de selección de problema
├── docs/
│   ├── memoria.pdf             # Memoria técnica del proyecto
│   └── screenshots/
├── CMakeLists.txt
└── README.md
```


## 📚 Referencias

- [The Little Book of Semaphores — Allen B. Downey](https://greenteapress.com/wp/semaphores/)
- [Dining Philosophers Problem — Wikipedia](https://en.wikipedia.org/wiki/Dining_philosophers_problem)
- [CPP Reference: Concurrency support library](https://en.cppreference.com/w/cpp/thread)
- [Microsoft Docs: Synchronization Objects](https://learn.microsoft.com/en-us/windows/win32/sync/synchronization-objects)

---

## 👤 Autor

**Alberto García Martín**

- 🌐 Portfolio: [albertogm.dev](https://albertogm.dev)
- 💼 LinkedIn: [in/albertogarciamartin](https://www.linkedin.com/in/albertogarciamartin/)
- 🐙 GitHub: [@albergm02](https://github.com/albergm02)
- 📧 Email: alberto.g.m.0214@gmail.com

---

<div align="center">

**⭐ Si este proyecto te resulta interesante, considera darle una estrella.**

</div>
