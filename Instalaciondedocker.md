
# Instalación de Docker en Windows, macOS y Ubuntu

<p align="center">
  <img src="https://locusit.se/wp-content/uploads/2024/08/MongoDB.png"
       alt="MongoDB y Docker"
       width="600">
</p>

<p align="center">
<b>Guía de instalación de Docker Desktop y Docker Engine para entornos de desarrollo con MongoDB</b><br>
Windows • macOS • Ubuntu • Linux
</p>

---

## Descripción

Docker es una plataforma que permite crear, distribuir y ejecutar aplicaciones en contenedores. En el ecosistema MongoDB, Docker simplifica la creación de entornos de desarrollo, laboratorios, pruebas y despliegues consistentes entre diferentes sistemas operativos.

### Beneficios de utilizar Docker con MongoDB

- Despliegue rápido de instancias MongoDB.
- Ambientes reproducibles para desarrollo y pruebas.
- Fácil administración de versiones.
- Integración con Docker Compose.
- Portabilidad entre Windows, macOS y Linux.
- Aislamiento de servicios y dependencias.

---


<!-- 
# Instalación de Docker en Windows, macOS y Ubuntu

> Guía de instalación de Docker Desktop y Docker Engine para entornos de desarrollo con MongoDB, contenedores y aplicaciones modernas.

 -->

---

# Introducción

Docker es una plataforma que permite crear, distribuir y ejecutar aplicaciones en contenedores. Su uso es fundamental para el desarrollo moderno, facilitando la portabilidad entre ambientes de desarrollo, pruebas y producción.

---

# Arquitectura General de Docker

![Arquitectura Docker](https://docs.docker.com/get-started/images/docker-architecture.webp)

---

# Requisitos Previos

Antes de instalar Docker, verifique los siguientes requisitos:

| Sistema Operativo | Requisito                         |
| ----------------- | --------------------------------- |
| Windows           | Windows 10/11 64 bits             |
| Windows (WSL2)    | WSL2 habilitado                   |
| Windows (Hyper-V) | Virtualización habilitada en BIOS |
| macOS             | Intel o Apple Silicon (M1/M2/M3)  |
| Ubuntu            | Usuario con permisos sudo         |
| Linux             | Kernel 3.10 o superior            |

---

# Instalación en Windows con WSL2 (Recomendada)

La integración con WSL2 ofrece mejor rendimiento y compatibilidad para desarrolladores.

## Paso 1: Descargar Docker Desktop

Descargue Docker Desktop desde:

https://www.docker.com/products/docker-desktop/

---

## Paso 2: Instalar WSL2

Abra PowerShell como administrador:

```powershell
wsl --install
```

Reinicie el equipo al finalizar.

---

## Paso 3: Activar el motor WSL2

Abra Docker Desktop y navegue a:

```text
Settings
 └── General
      └── Use the WSL 2 based engine
```

Active la opción:

✅ Use the WSL 2 based engine

---

## Paso 4: Configurar integración con WSL

Navegue a:

```text
Settings
 └── Resources
      └── WSL Integration
```

Active:

✅ Enable integration with my default WSL distro

---

### Referencia Visual

![Docker WSL2](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQR9f35bG5yUpU9iUhiF4T1J5JqZzorURyaBpr50eBu7vJiw8-62FT43I4&s=10)

---

## Verificar la instalación

```bash
docker --version
docker run hello-world
```

Resultado esperado:

```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

---

# Instalación en Windows utilizando Hyper-V

## Requisitos

* Windows 10 Pro o Enterprise
* Windows 11 Pro o Enterprise
* Virtualización habilitada en BIOS

---

## Habilitar Hyper-V

Abra PowerShell como administrador:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

Reinicie el equipo.

---

## Instalar Docker Desktop

Descargue Docker Desktop:

https://www.docker.com/products/docker-desktop/

---

### Referencia Visual

![Hyper-V](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQodvSJ8uCakx1A4Rqde9_c7BASXFNnXBCxNnBmielPYA&s=10)

---

# Instalación en macOS

Docker Desktop ofrece soporte para equipos Intel y Apple Silicon.

---

## Verificar arquitectura

En macOS:

```text
Apple Menu
 └── About This Mac
```

Verifique si utiliza:

* Intel
* Apple Silicon (M1, M2, M3)

---

## Descargar Docker Desktop

https://www.docker.com/products/docker-desktop/

---

## Instalar Rosetta (Apple Silicon)

```bash
softwareupdate --install-rosetta
```

---

## Instalar Docker

1. Abra el archivo descargado.
2. Arrastre Docker hacia Applications.
3. Ejecute Docker Desktop.

---

### Referencia Visual

![Docker macOS](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSoqB5z_RITmFJPhfSxpFEPRhUrq9KYrqaB1cz0gbYfgQ&s=10)

---

## Verificación

```bash
docker --version
docker run hello-world
```

---

# Instalación en Ubuntu

También puede consultar la documentación oficial:

https://docs.docker.com/engine/install/ubuntu/

---

## Actualizar repositorios

```bash
sudo apt-get update
```

---

## Instalar dependencias

```bash
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

---

## Crear directorio para llaves GPG

```bash
sudo mkdir -p /etc/apt/keyrings
```

---

## Descargar llave oficial de Docker

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
| sudo gpg --dearmor \
-o /etc/apt/keyrings/docker.gpg
```

---

## Agregar repositorio oficial

```bash
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" \
| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

---

## Actualizar repositorios nuevamente

```bash
sudo apt-get update
```

---

## Instalar Docker Engine

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

---

## Ejecutar prueba

```bash
sudo docker run hello-world
```

---

### Referencia Visual

![Docker Ubuntu](https://www.docker.com/wp-content/uploads/2022/03/Moby-logo.png)

---

# Ejecutar Docker sin sudo (Opcional)

Agregar el usuario al grupo docker:

```bash
sudo usermod -aG docker $USER
```

Cerrar sesión y volver a iniciar.

Validar:

```bash
docker ps
```

---

# Instalación en Otras Distribuciones Linux

## CentOS

https://docs.docker.com/engine/install/centos/

## Debian

https://docs.docker.com/engine/install/debian/

## Fedora

https://docs.docker.com/engine/install/fedora/

---

# Comandos Básicos de Docker

## Ver versión

```bash
docker --version
```

## Información del motor

```bash
docker info
```

## Listar imágenes

```bash
docker images
```

## Listar contenedores activos

```bash
docker ps
```

## Listar todos los contenedores

```bash
docker ps -a
```

## Ejecutar contenedor de prueba

```bash
docker run hello-world
```

---

# Solución de Problemas Comunes

| Problema                             | Solución                            |
| ------------------------------------ | ----------------------------------- |
| Docker no inicia en Windows          | Verificar WSL2 o Hyper-V            |
| Error de virtualización              | Habilitar VT-x o AMD-V en BIOS      |
| Permiso denegado en Linux            | Agregar usuario al grupo docker     |
| Docker Desktop consume mucha memoria | Ajustar Resources en Docker Desktop |
| Error al ejecutar contenedores       | Reiniciar servicio Docker           |

---

# Validación Final

Ejecute:

```bash
docker run hello-world
```

Si obtiene un resultado similar al siguiente:

```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

La instalación fue completada exitosamente.

---

# Referencias Oficiales

* https://www.docker.com/
* https://docs.docker.com/
* https://docs.docker.com/desktop/
* https://docs.docker.com/engine/install/
* https://learn.microsoft.com/windows/wsl/

---

# Conclusión

Docker proporciona una plataforma uniforme para ejecutar aplicaciones mediante contenedores en Windows, macOS y Linux.

**Recomendaciones:**

* Windows: Docker Desktop + WSL2
* macOS: Docker Desktop
* Ubuntu/Linux: Docker Engine

Una vez instalado correctamente, Docker estará listo para utilizarse con MongoDB, PostgreSQL, SQL Server, Redis, NGINX y cualquier otro servicio basado en contenedores.
