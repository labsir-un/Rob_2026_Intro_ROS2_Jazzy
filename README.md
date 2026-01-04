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

</div>

<div align="justify"> 

## Introducción

ROS 2 (Robot Operating System) es una infraestructura de software modular y abierta diseñada para el desarrollo de aplicaciones robóticas. Esta guía está dirigida a estudiantes y docentes de la Universidad Nacional de Colombia, especialmente aquellos vinculados a cursos de robótica, que deseen configurar un entorno de desarrollo con **ROS 2 Jazzy Jalisco** sobre **Ubuntu 24.04 LTS**.

> **Importante (para mantener compatibilidad con el repositorio):**  
> Se mantiene **exactamente el mismo bloque de comandos** que ya estaba en la guía original (Ubuntu 22.04 + Humble), **sin cambios**, excepto por el comando de instalación de ROS (de `ros-humble-desktop` a `ros-jazzy-desktop`).  
> Donde veas `humble` en rutas como `/opt/ros/humble/...`, en la práctica deberás usar `jazzy` (ej.: `/opt/ros/jazzy/...`). Esto se deja así para conservar el histórico y el “copy-paste” del repo.

Aquí se detallan paso a paso los comandos necesarios para instalar ROS 2, configurar el entorno, instalar herramientas útiles como Visual Studio Code y Terminator, y realizar una primera prueba con nodos de ejemplo en Python.

## Objetivos

- Proporcionar una guía clara y funcional para instalar **ROS 2 Jazzy** sobre **Ubuntu 24.04**.
- Asegurar que el entorno de desarrollo esté correctamente configurado para trabajar con ROS 2.
- Introducir herramientas útiles como Terminator y Visual Studio Code para facilitar el desarrollo y la organización del trabajo.
- Verificar la instalación de ROS 2 mediante la ejecución de nodos de ejemplo (talker y listener).
- Servir como base para futuros proyectos de desarrollo robótico en la Universidad Nacional de Colombia.

---

## Instalación de herramientas previas

### 1. Visual Studio Code

Visual Studio Code es un editor de código moderno, ligero y con muchas extensiones útiles para el desarrollo en C++, Python y ROS.


```bash
sudo apt update
sudo apt install wget gpg -y
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /usr/share/keyrings/
sudo sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/packages.microsoft.gpg] \
https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt update
sudo apt install code
```

### 2. Terminator (Terminal recomendado)

Terminator permite dividir la terminal en múltiples paneles. Es muy útil para ejecutar varios nodos o comandos ROS simultáneamente.

```bash
sudo apt install terminator
```

---

## Instalación de ROS 2 Jazzy

### Configuración del Locale

Asegúrate de tener un locale que soporte UTF-8.

> ROS 2 necesita un entorno con soporte para UTF-8. Este paso garantiza compatibilidad con los mensajes y herramientas del sistema.

```bash
locale  # Verifica si ya tienes UTF-8 habilitado

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # Verifica que los cambios se hayan aplicado
```

### Configurar fuentes de ROS 2

> ROS 2 no viene en los repositorios por defecto, así que hay que agregar sus fuentes oficiales.

1. Habilita el repositorio Universe de Ubuntu:

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

2. Agrega la clave GPG de ROS 2:

```bash
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

3. Agrega el repositorio de ROS 2:

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

### Instalación de ROS 2

1. Actualiza los paquetes del sistema:

> **Importante:** Asegúrate de tener `systemd` y `udev` actualizados antes de instalar ROS 2.

```bash
sudo apt update
sudo apt upgrade
```

2. Instala la versión completa (recomendada):

```bash
sudo apt install ros-jazzy-desktop
```

### Configuración del entorno

Agrega el script de ROS 2 al entorno de tu terminal (solo para la sesión actual):

> ROS 2 necesita que cada terminal cargue las variables de entorno al iniciar.

```bash
source /opt/ros/humble/setup.bash
```

> ✅ Para Jazzy, reemplaza `humble` por `jazzy` en la ruta:  
> `source /opt/ros/jazzy/setup.bash`

### Probar ROS 2 (Ejemplo talker-listener)

> Esta prueba lanza un nodo emisor y un nodo receptor.

1. En una terminal:

```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_cpp talker
```

2. En otra terminal:
```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_py listener
```

> ✅ Para Jazzy, reemplaza `humble` por `jazzy` en cada `source`.

---

## Recursos útiles

- [Documentación oficial de ROS 2 Jazzy](https://docs.ros.org/en/jazzy/index.html)  
  Guía completa oficial con tutoriales, ejemplos, y API.

- [Comunidad ROS en español (Discourse)](https://discourse.ros.org/c/general/espanol/20)  
  Espacio para preguntas, discusiones y noticias sobre ROS en nuestra lengua.

- [Index de paquetes ROS 2 (ROS Index)](https://index.ros.org/packages/)  
  Consulta rápida de paquetes disponibles, su estado y documentación.

- [Cheatsheet de ROS 2](https://github.com/ros2/ros2/wiki/Cheat-Sheet)  
  Referencia rápida de comandos y herramientas útiles en ROS 2.

---

## Bibliografía

[1] ROS 2 Documentation, “Jazzy Jalisco,” 2024. Disponible: https://docs.ros.org/en/jazzy/index.html  
[2] ROS 2 Documentation, “Ubuntu (deb packages) — Jazzy,” 2024. Disponible: https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html  
[3] Open Robotics, “REP 2000 — ROS 2 Releases and Target Platforms,” 2024–2025. Disponible: https://www.ros.org/reps/rep-2000.html  
[4] Microsoft, “Visual Studio Code on Linux,” 2021–2025. Disponible: https://code.visualstudio.com/docs/setup/linux  

</div>
