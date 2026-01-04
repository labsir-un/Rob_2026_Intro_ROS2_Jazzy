<div align="center">
<picture>
    <source srcset="https://imgur.com/5bYAzsb.png" media="(prefers-color-scheme: dark)">
    <source srcset="https://imgur.com/Os03JoE.png" media="(prefers-color-scheme: light)">
    <img src="https://imgur.com/Os03JoE.png" alt="Escudo UNAL" width="350px">
</picture>

<h3>Curso de Robótica 2025-II</h3>

<h1>Introducción a ROS 2 Jazzy</h1>

<h2>Guía 01 - Instalación de ROS 2 Jazzy en Ubuntu 24.04</h2>

<h4><b>Base del curso:</b> Ubuntu 24.04 LTS (Noble) + ROS 2 Jazzy Jalisco</h4>

<h4>Pedro Fabián Cárdenas Herrera<br>
    Manuel Felipe Carranza Montenegro</h4>

<p>
  <img alt="Ubuntu 24.04 LTS" src="https://img.shields.io/badge/Ubuntu-24.04%20LTS-E95420?logo=ubuntu&logoColor=white">
  <img alt="ROS 2 Jazzy" src="https://img.shields.io/badge/ROS%202-Jazzy-22314E?logo=ros&logoColor=white">
  <img alt="Instalación" src="https://img.shields.io/badge/Instalación-apt%20(deb)-2ea44f">
</p>

</div>

<div align="justify"> 

## Introducción

ROS 2 (Robot Operating System) es una infraestructura de software modular y abierta diseñada para el desarrollo de aplicaciones robóticas. Esta guía está dirigida a estudiantes y docentes de la Universidad Nacional de Colombia, especialmente aquellos vinculados a cursos de robótica, que deseen configurar un entorno de desarrollo con **ROS 2 Jazzy Jalisco** sobre **Ubuntu 24.04 LTS (Noble Numbat)**.

> ⚠️ Importante: **ROS 2 Humble** fue diseñado para **Ubuntu 22.04**, mientras que **ROS 2 Jazzy** tiene como plataforma objetivo principal **Ubuntu 24.04**. Para evitar incompatibilidades y reducir fricción en el curso, se estandariza en **Jazzy + 24.04**.  

En esta guía encontrarás:
- Instalación de herramientas recomendadas (VS Code, Terminator).
- Instalación de ROS 2 Jazzy usando **paquetes deb (apt)**.
- Configuración del entorno (source automático).
- Prueba rápida con nodos `talker` y `listener`.
- Sugerencias de herramientas y “buenas prácticas” para que tu entorno sea estable desde el día 1.

---

## Tabla de contenidos
- [Objetivos](#objetivos)
- [Antes de empezar](#antes-de-empezar)
- [Instalación de herramientas previas](#instalación-de-herramientas-previas)
  - [1. Visual Studio Code](#1-visual-studio-code)
  - [2. Terminator (terminal recomendado)](#2-terminator-terminal-recomendado)
- [Instalación de ROS 2 Jazzy](#instalación-de-ros-2-jazzy)
  - [1. Configuración del locale](#1-configuración-del-locale)
  - [2. Configurar fuentes oficiales (apt)](#2-configurar-fuentes-oficiales-apt)
  - [3. Instalar ROS 2](#3-instalar-ros-2)
  - [4. Configurar el entorno](#4-configurar-el-entorno)
- [Probar ROS 2 (talker/listener)](#probar-ros-2-talkerlistener)
- [Primer workspace (colcon) en 2 minutos](#primer-workspace-colcon-en-2-minutos)
- [Problemas comunes](#problemas-comunes)
- [Recursos útiles](#recursos-útiles)
- [Bibliografía](#bibliografía)
- [Videos recomendados](#videos-recomendados)

---

## Objetivos

- Proporcionar una guía clara y funcional para instalar **ROS 2 Jazzy** sobre **Ubuntu 24.04**.
- Asegurar que el entorno de desarrollo quede correctamente configurado para trabajar con ROS 2.
- Introducir herramientas útiles como **Terminator** y **Visual Studio Code** para facilitar el desarrollo.
- Verificar la instalación de ROS 2 con la ejecución de nodos de ejemplo (`talker` y `listener`).
- Servir como base para futuras prácticas y proyectos de robótica en la UNAL.

---

## Antes de empezar

✅ Recomendado (para el curso):
- Ubuntu 24.04 instalado **nativo** o **dual boot**  
- Conexión a internet estable
- Usuario con permisos `sudo`

⚠️ Si estás en VM/WSL2:
- Puedes instalar ROS 2 y practicar comandos, pero simuladores gráficos (RViz/Gazebo) pueden requerir configuración adicional o ir lentos.

---

## Instalación de herramientas previas

### 1. Visual Studio Code

Visual Studio Code es un editor moderno, ligero y con extensiones útiles para desarrollo en C++, Python y ROS 2.

#### Opción A (recomendada): instalar desde repositorio oficial de Microsoft (APT)
```bash
sudo apt update
sudo apt install -y wget gpg

# Keyring (método moderno recomendado)
sudo install -d -m 0755 /etc/apt/keyrings
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | sudo tee /etc/apt/keyrings/packages.microsoft.gpg > /dev/null
sudo chmod a+r /etc/apt/keyrings/packages.microsoft.gpg

# Repo
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" \
| sudo tee /etc/apt/sources.list.d/vscode.list > /dev/null

sudo apt update
sudo apt install -y code
```

#### Opción B (simple): instalar desde el paquete `.deb`
Esta opción también es válida si prefieres el instalador descargable desde la web oficial.
> Ver el link oficial en [Bibliografía](#bibliografía).

---

### 2. Terminator (terminal recomendado)

Terminator permite dividir la terminal en múltiples paneles. Es muy útil para ejecutar varios nodos ROS simultáneamente.

```bash
sudo apt update
sudo apt install -y terminator
```

---

## Instalación de ROS 2 Jazzy

### 1. Configuración del locale

Asegúrate de tener un locale que soporte UTF-8.

```bash
locale

sudo apt update
sudo apt install -y locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8

# Aplica a la sesión actual
export LANG=en_US.UTF-8

locale
```

---

### 2. Configurar fuentes oficiales (apt)

ROS 2 no viene en los repositorios por defecto; se debe agregar su repo oficial.

#### 2.1 Habilitar Universe
```bash
sudo apt install -y software-properties-common
sudo add-apt-repository universe
```

#### 2.2 Agregar clave GPG de ROS
```bash
sudo apt update
sudo apt install -y curl
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key \
  -o /usr/share/keyrings/ros-archive-keyring.gpg
```

#### 2.3 Agregar repositorio de ROS 2
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] \
http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" \
| sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

---

### 3. Instalar ROS 2

1) Actualiza el sistema:
```bash
sudo apt update
sudo apt upgrade -y
```

2) Instala la versión completa (recomendada para el curso):
```bash
sudo apt install -y ros-jazzy-desktop
```

3) Instala herramientas de desarrollo útiles (recomendado):
```bash
sudo apt install -y ros-dev-tools
```

---

### 4. Configurar el entorno

Para usar ROS 2, debes “sourcear” su entorno.

#### Para la sesión actual:
```bash
source /opt/ros/jazzy/setup.bash
```

#### Para que quede automático en cada terminal (recomendado):
```bash
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

Verifica:
```bash
ros2 --help
```

---

## Probar ROS 2 (talker/listener)

Esta prueba lanza un nodo emisor y un nodo receptor.

### Terminal 1
```bash
source /opt/ros/jazzy/setup.bash
ros2 run demo_nodes_cpp talker
```

### Terminal 2
```bash
source /opt/ros/jazzy/setup.bash
ros2 run demo_nodes_py listener
```

Si todo está bien, verás mensajes publicados y recibidos continuamente.

---

## Primer workspace (colcon) en 2 minutos

> Esto te deja listo para compilar paquetes propios en el curso.

```bash
# 1) Crea workspace
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws

# 2) Build vacío (verifica que colcon funciona)
colcon build

# 3) Source del workspace (cada vez que lo uses)
source install/setup.bash
```

Recomendado: agrega también el `source` del workspace al final de tu `~/.bashrc` (solo cuando ya estés trabajando activamente con ese workspace):
```bash
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
```

---

## Problemas comunes

### 1) “Estoy en Ubuntu 22.04 / otra versión”
ROS 2 Jazzy está orientado a Ubuntu 24.04 como plataforma objetivo. Si estás en otra versión, considera usar la distro ROS acorde (por ejemplo Humble en 22.04) o instalar desde fuente (más avanzado).  

### 2) Errores de clave/GPG al hacer `apt update`
- Verifica que el archivo de keyring exista y tenga permisos de lectura.
- Repite los pasos de key + repo (sección [2.2](#22-agregar-clave-gpg-de-ros) y [2.3](#23-agregar-repositorio-de-ros-2)).

### 3) “ros2: command not found”
Casi siempre es porque no se hizo `source`:
```bash
source /opt/ros/jazzy/setup.bash
```

### 4) Simulación/GUI lenta en VM
En VM, activa aceleración 3D, asigna más RAM/CPU y considera instalar ROS nativo si usarás RViz/Gazebo intensivamente.

---

## Recursos útiles

- Documentación oficial ROS 2 Jazzy (instalación):  
  https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html

- REP 2000 (plataformas objetivo de cada distro ROS 2):  
  https://www.ros.org/reps/rep-2000.html

- ROS Index (paquetes):  
  https://index.ros.org/packages/

- Comunidad ROS (Discourse):  
  https://discourse.ros.org/

---

## Bibliografía

Formato IEEE (enlaces incluidos para consulta)

[1] ROS 2 Documentation, “Ubuntu (deb packages) — Jazzy,” 2024. Disponible: https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html  
[2] ROS 2 Documentation, “Installation — Jazzy,” 2024. Disponible: https://docs.ros.org/en/jazzy/Installation.html  
[3] Open Robotics, “REP 2000 — ROS 2 Releases and Target Platforms,” 2024–2025. Disponible: https://www.ros.org/reps/rep-2000.html  
[4] Ubuntu Releases, “Ubuntu 24.04 (Noble) Downloads,” 2024–2025. Disponible: https://releases.ubuntu.com/noble  
[5] Visual Studio Code Docs, “Visual Studio Code on Linux,” 2021–2025. Disponible: https://code.visualstudio.com/docs/setup/linux  
[6] Microsoft Learn, “Linux Software Repository for Microsoft Products,” May 2025. Disponible: https://learn.microsoft.com/en-us/linux/packages  

---

## Videos recomendados

> Estos videos son opcionales y sirven como apoyo visual. Prioriza siempre la documentación oficial.

- Instalación ROS 2 Jazzy en Ubuntu 24.04 (walk-through): https://www.youtube.com/watch?v=zaKCYGf6k08  
- Instalación ROS 2 Jazzy en Ubuntu 24.04 (alternativo): https://www.youtube.com/watch?v=ZGds6NuZLzo  
- VS Code en Ubuntu (instalación y uso básico): https://www.youtube.com/watch?v=NX8SHmkuLn4  

</div>
