# Máquina Virtual Linux – Configuración, Wine y Ejecución de Juegos

Guía completa para configurar Docker, optimizar tu máquina virtual Linux y ejecutar juegos de Windows usando Wine.

---

## 📋 Tabla de Contenidos

- [Instalación y Configuración de Docker](#instalación-y-configuración-de-docker)
- [Instalación de Wine](#instalación-de-wine)
- [Descargar y Ejecutar Juegos](#descargar-y-ejecutar-juegos)
- [Solución de Problemas](#solución-de-problemas)

---

## ⚙️ Instalación y Configuración de Docker

### Paso 1: Verificar espacio disponible

Antes de comenzar, asegúrate de tener suficiente espacio en tu sistema:

```bash
df -h
```

**Cómo interpretar la salida:**  
Este comando muestra el uso de espacio en disco. Observa la columna **Available** para saber cuánto espacio libre tienes. Necesitarás al menos 20-30 GB disponibles.

---

### Paso 2: Crear directorio para Docker

Crea un directorio temporal donde Docker almacenará sus datos:

```bash
sudo mkdir -p /tmp/docker-data
sudo chmod 755 /tmp/docker-data
```

**¿Por qué este directorio?**  
Docker necesita un lugar para almacenar imágenes y contenedores. Usar `/tmp/docker-data` optimiza el espacio disponible en entornos limitados.

---

### Paso 3: Configurar Docker

Edita el archivo de configuración de Docker:

```bash
sudo nano /etc/docker/daemon.json
```

Agrega exactamente este contenido:

```json
{
  "data-root": "/tmp/docker-data",
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

**¿Qué hace esta configuración?**

- **data-root**: Cambia dónde Docker guarda sus datos.
- **log-driver**: Define el formato de los registros.
- **log-opts**: Limita el tamaño de los archivos de registro para evitar que llenen el disco.

---

### Paso 4: Reiniciar servicios

Reinicia Docker para aplicar los cambios:

```bash
sudo systemctl restart docker
```

Verifica que Docker esté funcionando correctamente:

```bash
sudo systemctl status docker
```

Si ves el mensaje `active (running)`, Docker está correctamente configurado.

---

## 🍷 Instalación de Wine

Wine permite ejecutar programas y juegos de Windows (.exe) en Linux.

### Paso 1: Actualizar el sistema

Primero, actualiza todos los paquetes:

```bash
sudo apt update && sudo apt upgrade -y
```

---

### Paso 2: Instalar Wine

Agrega soporte para arquitectura de 32 bits e instala Wine:

```bash
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install wine wine32 wine64 -y
```

Verifica la instalación:

```bash
wine --version
```

---

### Paso 3: Instalar herramientas útiles (opcional pero recomendado)

Para mejor compatibilidad con juegos, instala winetricks:

```bash
sudo apt install winetricks -y
```

---

## 🎮 Descargar y Ejecutar Juegos con Wine

### Paso 1: Descargar tu juego

Descarga el archivo ejecutable (.exe) de tu juego desde una fuente confiable y guárdalo en tu carpeta de descargas o en tu carpeta personal.

---

### Paso 2: Extraer archivos comprimidos (si es necesario)

Si tu juego viene comprimido en `.zip` o `.tar`:

**Para archivos ZIP:**
```bash
sudo apt install unzip -y
unzip nombre_del_archivo.zip
```

**Para archivos TAR:**
```bash
tar -xf nombre_del_archivo.tar
```

---

### Paso 3: Ejecutar el juego

1. Abre una terminal en la carpeta del juego:
   ```bash
   cd ~/Descargas/NombreDelJuego
   ```

2. Localiza el archivo `.exe` del juego.

3. Ejecuta el juego con Wine:
   ```bash
   wine juego.exe
   ```

El juego debería abrirse en tu máquina virtual Linux.

---

## 🛠️ Solución de Problemas

### El juego no se ejecuta o es muy lento

Prueba configurar Wine para usar arquitectura de 32 bits:

```bash
WINEARCH=win32 WINEPREFIX=~/.wine32 wine juego.exe
```

---

### Configurar opciones gráficas

Abre la configuración de Wine para ajustar gráficos y compatibilidad:

```bash
winecfg
```

Aquí puedes cambiar la versión de Windows emulada, ajustar resolución y configurar controladores.

---

### Instalar librerías adicionales para juegos

Algunos juegos necesitan librerías específicas (DirectX, .NET Framework, etc.):

```bash
winetricks vcrun2019 dotnet48
```

Consulta la documentación del juego específico para saber qué librerías necesita.

---

### El juego funciona pero con problemas de audio

Prueba reinstalar los componentes de audio:

```bash
winetricks sound=pulseaudio
```

---

## 💡 Consejos Finales

Para optimizar tu experiencia con Wine y juegos en Linux, considera lo siguiente:

- Asegúrate de tener suficiente espacio en disco (mínimo 50 GB para varios juegos).
- Actualiza regularmente tu sistema y Wine.
- Algunos juegos pueden necesitar configuración adicional; consulta la comunidad de ProtonDB o WineHQ.
- Usa contenedores Docker para aislar diferentes configuraciones de Wine si ejecutas múltiples juegos.
- Considera usar Proton (versión de Wine optimizada para Steam) si prefieres una alternativa más moderna.

---

## ✅ Resumen

Con esta guía podrás:

- Verificar y optimizar el espacio de tu máquina virtual.
- Configurar Docker correctamente para máquinas con recursos limitados.
- Instalar Wine y todas las herramientas necesarias.
- Ejecutar juegos y programas de Windows en Linux sin problemas.

---

## 📞 Soporte

Si encuentras problemas o tienes dudas, puedes:

- Revisar la [documentación oficial de Wine](https://www.winehq.org/)
- Consultar [ProtonDB](https://protondb.com/) para juegos específicos
- Abrir un issue en este repositorio

¡Buena suerte y que disfrutes jugando en Linux! 🎮
