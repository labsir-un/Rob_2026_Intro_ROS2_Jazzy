<div align="center">
<picture>
    <source srcset="https://imgur.com/5bYAzsb.png" media="(prefers-color-scheme: dark)">
    <source srcset="https://imgur.com/Os03JoE.png" media="(prefers-color-scheme: light)">
    <img src="https://imgur.com/Os03JoE.png" alt="Escudo UNAL" width="350px">
</picture>

<h3>Curso de Robótica 2026-I</h3>

<h1>Introducción a ROS 2 Jazzy</h1>

<h2>Instalación de ROS 2 Jazzy en Ubuntu 24.04</h2>

<h4>Pedro Fabián Cárdenas Herrera<br>
    Manuel Felipe Carranza Montenegro</h4>

<p>
  <img alt="Ubuntu 24.04 LTS" src="https://img.shields.io/badge/Ubuntu-24.04%20LTS-E95420?logo=ubuntu&logoColor=white">
  <img alt="ROS 2 Jazzy" src="https://img.shields.io/badge/ROS%202-Jazzy-22314E?logo=ros&logoColor=white">
  <img alt="Target" src="https://img.shields.io/badge/Target-Desktop%20Install-2ea44f">
</p>

</div>

<div align="justify"> 

## Tabla de contenidos
- [Introducción](#introducción)
- [Objetivos](#objetivos)
- [Antes de instalar](#antes-de-instalar)
- [Instalación de herramientas previas](#instalación-de-herramientas-previas)
  - [1. Visual Studio Code](#1-visual-studio-code)
  - [2. Terminator (terminal recomendado)](#2-terminator-terminal-recomendado)
- [Instalación de ROS 2 Jazzy (binarios .deb)](#instalación-de-ros-2-jazzy-binarios-deb)
  - [1. Configuración del locale (UTF-8)](#1-configuración-del-locale-utf-8)
  - [2. Habilitar Universe + repositorios de ROS 2](#2-habilitar-universe--repositorios-de-ros-2)
  - [3. Instalar ROS 2 Jazzy](#3-instalar-ros-2-jazzy)
  - [4. Configurar el entorno](#4-configurar-el-entorno)
- [Prueba rápida (talker / listener)](#prueba-rápida-talker--listener)
- [Herramientas recomendadas para desarrollo](#herramientas-recomendadas-para-desarrollo)
- [Checklist de verificación](#checklist-de-verificación)
- [Recursos útiles](#recursos-útiles)
- [Bibliografía](#bibliografía)

---

## Introducción

ROS 2 (Robot Operating System) es una infraestructura de software modular y abierta diseñada para el desarrollo de aplicaciones robóticas. Esta guía está dirigida a estudiantes y docentes de la Universidad Nacional de Colombia, especialmente aquellos vinculados a cursos de robótica, que deseen configurar un entorno de desarrollo con **ROS 2 Jazzy Jalisco** sobre **Ubuntu 24.04 LTS (Noble Numbat)**.

En esta guía se detallan paso a paso los comandos necesarios para:
- Preparar el sistema (locale, repositorios y herramientas base).
- Instalar ROS 2 desde binarios (`apt`).
- Configurar el entorno (sourcing y `~/.bashrc`).
- Ejecutar una prueba mínima con nodos de ejemplo (talker/listener).

> Nota importante: **ROS 2 Humble** se diseñó principalmente para Ubuntu 22.04. Para el curso (Ubuntu 24.04), la recomendación es **ROS 2 Jazzy** por compatibilidad directa y repos oficiales de paquetes.

---

## Objetivos

- Proporcionar una guía clara y funcional para instalar **ROS 2 Jazzy** sobre **Ubuntu 24.04**.
- Asegurar que el entorno de desarrollo quede correctamente configurado para trabajar con ROS 2.
- Introducir herramientas útiles como **Terminator** y **Visual Studio Code** para facilitar el desarrollo.
- Verificar la instalación mediante la ejecución de nodos de ejemplo (**talker** y **listener**).
- Servir como base para futuros proyectos de desarrollo robótico en la Universidad Nacional de Colombia.

---

## Antes de instalar

### Requisitos recomendados (para ROS + simulación ligera)
- **CPU:** 4 núcleos (mín.) / 6+ recomendado
- **RAM:** 8 GB (mín.) / 16 GB recomendado
- **Disco:** 60 GB libres (mín.) / 100 GB recomendado
- **Internet:** recomendado (repositorios, dependencias)

### Buenas prácticas
- Ejecuta primero una actualización general del sistema:
```bash
sudo apt update && sudo apt upgrade -y
```
- Reinicia si el sistema lo sugiere (especialmente tras actualizaciones de kernel).

---

## Instalación de herramientas previas

### 1. Visual Studio Code

Visual Studio Code es un editor moderno y ligero, con extensiones útiles para ROS 2, Python y C++.

✅ **Opción recomendada (repositorio oficial, auto-actualizaciones)**

```bash
sudo apt update
sudo apt install -y wget gpg

wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo install -D -o root -g root -m 644 microsoft.gpg /usr/share/keyrings/microsoft.gpg
rm -f microsoft.gpg

cat <<'EOF' | sudo tee /etc/apt/sources.list.d/vscode.sources > /dev/null
Types: deb
URIs: https://packages.microsoft.com/repos/code
Suites: stable
Components: main
Architectures: amd64,arm64,armhf
Signed-By: /usr/share/keyrings/microsoft.gpg
EOF

sudo apt update
sudo apt install -y code
```

✅ **Extensiones sugeridas (para ROS)**
- **ROS** (Microsoft)  
- **Python**  
- **C/C++**  
- **YAML**  
- **Remote - SSH** (si vas a trabajar con robots/raspberries remotas)

> Tip: en Ubuntu 24.04, VS Code también está disponible como Snap; el repositorio apt suele ser preferido en entornos académicos por control de versiones.

---

### 2. Terminator (terminal recomendado)

Terminator permite dividir la terminal en múltiples paneles. Es muy útil para ejecutar varios nodos o comandos ROS simultáneamente.

```bash
sudo apt install -y terminator
```

Atajos útiles:
- Dividir horizontal: `Ctrl + Shift + O`
- Dividir vertical: `Ctrl + Shift + E`
- Cambiar entre paneles: `Ctrl + Shift + Flechas`

---

## Instalación de ROS 2 Jazzy (binarios .deb)

### 1. Configuración del locale (UTF-8)

ROS 2 necesita un entorno con soporte UTF-8.

```bash
locale  # verifica si ya tienes UTF-8

sudo apt update && sudo apt install -y locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verifica cambios
```

---

### 2. Habilitar Universe + repositorios de ROS 2

1) Habilita el repositorio *Universe*:

```bash
sudo apt install -y software-properties-common
sudo add-apt-repository universe
```

2) Agrega repositorios de ROS 2 usando el paquete oficial **ros2-apt-source**  
(Este método es más “moderno” porque gestiona la llave y el repo con un paquete actualizable.)

```bash
sudo apt update && sudo apt install -y curl

export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest \
| grep -F "tag_name" | awk -F\" '{print $4}')

curl -L -o /tmp/ros2-apt-source.deb \
"https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"

sudo dpkg -i /tmp/ros2-apt-source.deb
```

(Opcional, pero recomendado si vas a compilar paquetes)
```bash
sudo apt update && sudo apt install -y ros-dev-tools
```

---

### 3. Instalar ROS 2 Jazzy

Actualiza repos y el sistema:

```bash
sudo apt update
sudo apt upgrade -y
```

✅ **Instalación Desktop (recomendada):** incluye ROS, RViz, demos y tutoriales
```bash
sudo apt install -y ros-jazzy-desktop
```

---

### 4. Configurar el entorno

Para la sesión actual:
```bash
source /opt/ros/jazzy/setup.bash
```

Para que se cargue en cada terminal (recomendado):
```bash
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

> Si en el futuro instalas más de una distro de ROS 2, evita “hardcodear” varias en `~/.bashrc` sin control. Mantén una sola activa o usa perfiles/aliases.

---

## Prueba rápida (talker / listener)

1) En una terminal:
```bash
source /opt/ros/jazzy/setup.bash
ros2 run demo_nodes_cpp talker
```

2) En otra terminal:
```bash
source /opt/ros/jazzy/setup.bash
ros2 run demo_nodes_py listener
```

Deberías ver al **talker** publicando y al **listener** recibiendo mensajes. Esto valida que tanto C++ como Python funcionan correctamente.

---

## Herramientas recomendadas para desarrollo

### Instalar utilidades comunes
```bash
sudo apt install -y git python3-pip python3-venv
```

### Crear un workspace mínimo (colcon)
```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
colcon build
```

Cargar el workspace:
```bash
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

> Nota: Si no tienes `colcon`, normalmente viene con `ros-dev-tools` (o puedes instalar `python3-colcon-common-extensions`).

---

## Checklist de verificación

Ejecuta esto y revisa que todo sea coherente:

```bash
printenv | grep -E "ROS_DISTRO|ROS_VERSION"
ros2 --version
ros2 pkg list | head
```

Resultado esperado:
- `ROS_DISTRO=jazzy`
- `ROS_VERSION=2`
- `ros2 --version` devuelve una versión válida
- `ros2 pkg list` muestra paquetes

---

## Recursos útiles

- Documentación oficial ROS 2 Jazzy (instalación en Ubuntu):  
  https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html

- REP 2000 (plataformas objetivo por release de ROS 2):  
  https://www.ros.org/reps/rep-2000.html

- VS Code en Linux (repo oficial / método apt):  
  https://code.visualstudio.com/docs/setup/linux

- Comunidad ROS (Discourse):  
  https://discourse.ros.org/

### Videos recomendados (opcionales)
> Sugerencia: prioriza canales oficiales, cursos universitarios o creadores reconocidos.

- Instalación de ROS 2 Jazzy (tutorial general, paso a paso):  
  https://www.youtube.com/results?search_query=install+ros+2+jazzy+ubuntu+24.04

- Talker/Listener y primeros comandos ROS 2:  
  https://www.youtube.com/results?search_query=ros2+jazzy+talker+listener

---

## Bibliografía

[1] Open Robotics, “Ubuntu (deb packages) — ROS 2 Jazzy Documentation,” 2024–2026. Disponible: https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html

[2] Microsoft, “Visual Studio Code on Linux,” Visual Studio Code Documentation, 2024–2026. Disponible: https://code.visualstudio.com/docs/setup/linux

[3] Canonical, “Ubuntu 24.04 (Noble Numbat) Downloads,” Ubuntu Releases, 2024–2026. Disponible: https://releases.ubuntu.com/noble

[4] Open Robotics, “REP 2000 — ROS 2 Releases and Target Platforms,” ROS Enhancement Proposals, 2024–2026. Disponible: https://www.ros.org/reps/rep-2000.html

[5] Ask Ubuntu Community, “How do I install Terminator?,” Ask Ubuntu, 2016. Disponible: https://askubuntu.com/questions/829045/how-do-i-install-terminator

</div>
