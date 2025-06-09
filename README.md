# Sistema de Mensajería LAN en C++

## Descripción del Proyecto
Este proyecto desarrolla un sistema de chat local (LAN) multicliente utilizando C++ y la API de Sockets. Permite a múltiples usuarios registrarse, autenticarse y comunicarse en tiempo real, gestionando las conexiones y los mensajes de forma eficiente mediante estructuras de datos como Tablas Hash y Listas Enlazadas.

## Autores
* Kevin Marzul Jeronimo Rojano
* Raúl Nava Soler
* Juan Elías Antonio Ramírez

---

## Requisitos del Sistema

Para compilar y ejecutar este sistema de mensajería, necesitarás los siguientes elementos:

1.  **Sistema Operativo:**
    * Un entorno tipo **UNIX**, incluyendo **Linux**, **macOS**, o **Windows Subsystem for Linux (WSL)**.

2.  **Compilador de C++:**
    * **G++ (GNU C++ Compiler)** compatible con C++11 o superior es el recomendado.
    * Puedes verificarlo con `g++ --version` en tu terminal.
    * Para instalarlo (si no lo tienes):
        * **En sistemas basados en Debian/Ubuntu:**
            ```bash
            sudo apt update
            sudo apt install build-essential
            ```
        * **En macOS (con Xcode Command Line Tools):**
            ```bash
            xcode-select --install
            ```
        * **En WSL (Ubuntu):**
            ```bash
            sudo apt update
            sudo apt install build-essential
            ```

3.  **Acceso a Terminal/Línea de Comandos:**
    * Todas las operaciones se realizarán a través de la terminal de tu sistema.

## Manual de Uso

Este manual te guiará a través de los pasos para configurar y utilizar el sistema de mensajería (servidor y cliente).

### Paso 0: Preparación Inicial

1.  **Descarga o Clona el Proyecto:**
    * Descarga el ZIP desde GitHub o clona el repositorio:
        ```bash
        git clone https://github.com/kevinmaar/Mensajeria-LAN.git
        ```
        ```bash
        cd Mensajeria-LAN
        ```

### Parte 1: Configuración y Ejecución del Servidor

El servidor es el primero que debe estar activo para que los clientes puedan conectarse.

#### 1.1. Configurar Usuarios en el Servidor (`server.cpp`)

Para que los usuarios puedan iniciar sesión, debes registrarlos en el código.

1.  **Abre `server.cpp`** con tu editor de codigo.
2.  **Localiza la función `registrarUsuarios`** (aproximadamente en la línea 23).
3.  **Añade usuarios y contraseñas** usando el siguiente formato:
    ```cpp
    // Línea 23 (aproximadamente) en server.cpp
    void registrarUsuarios(TablaHash& tabla) {
        tabla.Registrar("magic", "tira1d20"); // Usuario: magic, Contraseña: tira1d20
        tabla.Registrar("usuario2", "contraseña2");
        tabla.Registrar("admin", "passwordseguro");
    }
    ```

#### 1.2. Compilar el Código del Servidor

**En tu Terminal** (en la carpeta del proyecto), ejecuta:
    ```bash
    g++ hashhead.cpp ListaEnlazada.cpp server.cpp -o server
    ```
    * Esto compila los archivos fuente y crea un ejecutable llamado `server`.
    * Si hay errores, revísalos y corrige en `server.cpp`.

#### 1.3. Obtener la Dirección IP del Servidor

Los clientes necesitan la IP de este equipo para conectarse.

1.  **En la misma Terminal**, ejecuta:
    ```bash
    ip a
    ```
    * Busca la dirección `inet` (ej. `192.168.1.100`) en la salida, generalmente asociada a tu interfaz de red activa.
    * **Anota esta IP y compártela** con los usuarios del cliente.

#### 1.4. Ejecutar el Servidor

1.  **En la misma Terminal**, ejecuta:
    ```bash
    ./server
    ```
    * Verás mensajes indicando que el servidor está escuchando en el puerto 25565.
    * **Mantén esta terminal abierta**; el servidor debe permanecer ejecutándose.

#### 1.5. Detener el Servidor

1.  Presiona **Enter** en la terminal donde se está ejecutando el servidor.
2.  El servidor se detendrá y modificará el nombre del archivo de log para incluir la fecha y hora de cierre.

### Parte 2: Configuración y Ejecución del Cliente

Los clientes son los programas que los usuarios ejecutarán para conectarse y chatear.

#### 2.1. Modificar el Código del Cliente (`cliente.cpp`)

Cada cliente necesita la dirección IP del servidor.

1.  **Abre `cliente.cpp`** con tu editor de codigo.
2.  **Dirígete a la línea 25** y busca la línea que contiene `inet_pton`.
3.  **Coloca la dirección IP del servidor** que obtuviste en el Paso 1.3:
    ```cpp
    // Línea 25 (aproximadamente) en cliente.cpp
    inet_pton(AF_INET, "IP_DEL_SERVIDOR", &servidor.sin_addr); // Ejemplo: inet_pton(AF_INET, "192.168.1.100", &servidor.sin_addr);
    ```

#### 2.2. Compilar el Código del Cliente

1.  **Abre una Terminal nueva** y navega a la carpeta del proyecto.
2.  **Ejecuta el comando de compilación:**
    ```bash
    g++ cliente.cpp -o cliente
    ```
    * Esto crea un ejecutable llamado `cliente`.

#### 2.3. Ejecutar el Cliente

1.  **Asegúrate de que el servidor esté encendido y funcionando** (Paso 1.4).
2.  **En la Terminal donde compilaste el cliente**, ejecuta:
    ```bash
    ./cliente
    ```
3.  Si la IP es correcta y el servidor está activo, verás un mensaje de conexión exitosa.

### Parte 3: Interacción con el Programa

Una vez que el cliente se ha conectado, puedes empezar a interactuar.

#### 3.1. Autenticación

1.  Al conectarse, se te pedirá:
    ```
    Usuario:
    Password:
    ```
2.  **Ingresa el Usuario** y presiona Enter.
3.  **Ingresa el Password** y presiona Enter.
4.  Recibirás un mensaje de éxito si las credenciales son correctas y el usuario no está ya conectado.

#### 3.2. Envío de Mensajes

1.  Una vez autenticado, **todo lo que escribas y presiones Enter será enviado al servidor.**
2.  El servidor retransmitirá tu mensaje a **todos los demás clientes conectados y autenticados**. Verás los mensajes de otros usuarios aparecer en tu terminal.

#### 3.3. Desconexión del Cliente

Para salir del chat:

1.  Simplemente **cierra la ventana de la terminal del cliente.**
2.  Alternativamente, puedes presionar `Ctrl+C` o `Ctrl+Z` en la terminal del cliente.
3.  El servidor detectará la desconexión y actualizará el estado del usuario.

---

## Características Principales
* **Servidor de Chat Multicliente:** Capaz de gestionar múltiples conexiones simultáneamente.
* **Autenticación de Usuarios:** Utiliza una Tabla Hash para almacenar y validar credenciales.
* **Gestión de Clientes Conectados:** Administra el estado de los clientes en tiempo real mediante una Lista Simplemente Enlazada.
* **Comunicación en Red:** Implementado con la API de Sockets.
* **Concurrencia con `select()`:** El servidor maneja múltiples clientes en un único hilo, simplificando la programación concurrente.
* **Transmisión de Mensajes (Broadcast):** Los mensajes se retransmiten a todos los demás usuarios conectados.
* **Registro de Eventos:** El servidor genera un archivo `logs.txt`.

## Marco Teórico y Diseño

### Estructuras de Datos y Componentes Usados
Para la construcción de este sistema, se seleccionaron cuidadosamente dos estructuras de datos principales y la API de Sockets:

* **Tabla Hash para Gestión de Usuarios:** Implementada para almacenar y validar credenciales de usuario. Destaca por su eficiencia promedio de $O(1)$ en operaciones de búsqueda e inserción. Maneja colisiones mediante sondeo lineal.
    * **Archivos:** `hashheader.hpp`, `hashhead.cpp`
    * **Funcionalidades Clave:** `Registrar`, `iniciarsesion`, `Desconectar`.

* **Lista Simplemente Enlazada para Clientes Conectados:** Utilizada para gestionar dinámicamente a los clientes activos en el servidor. Su flexibilidad permite un crecimiento y decrecimiento del tamaño en tiempo de ejecución.
    * **Archivos:** `ListaEnlazada.hpp`, `ListaEnlazada.cpp`
    * **Funcionalidades Clave:** `insertarAlFinal` $O(1)$, `eliminarNodo` $O(k)$, `obtenerCabeza`, `ObtenerCantidad`.

* **Sockets y Concurrencia con `select()`:** La comunicación en red se gestiona a través de la API de Sockets. El servidor utiliza un bucle `while(true)` con `select()` para monitorear simultáneamente nuevas conexiones y mensajes de clientes existentes, logrando concurrencia en un solo hilo.
    * **Archivos:** `cliente.cpp`, `server.cpp`
    * **Funcionalidades Clave:** `crearSocketServidor`, `aceptarCliente`, `manejarMensajes`.

## Análisis de Complejidad
El análisis de complejidad es crucial para entender el rendimiento del sistema:

* **Tabla Hash (`TablaHash`):**
    * **Inserción (`Registrar`) y Búsqueda (`iniciarsesion`):** En el caso promedio, la complejidad es $O(1)$. En el peor de los casos (alta colisión con sondeo lineal), la complejidad es $O(N)$, donde $N$ es la capacidad de la tabla.
* **Lista Enlazada (`Lista`):**
    * **Inserción (`insertarAlFinal`):** La complejidad es constante, $O(1)$, gracias a un puntero a la cola.
    * **Eliminación (`eliminarNodo`):** Requiere una búsqueda previa, lo que resulta en una complejidad lineal, $O(k)$, donde $k$ es el número de clientes conectados.
    * **Recorrido (para broadcast en `manejarMensajes`):** Se deben visitar todos los nodos, lo que implica una complejidad de $O(k)$.

El rendimiento general del servidor en cada ciclo del bucle principal está dominado por el recorrido de la lista de clientes para el broadcast de mensajes, lo que le confiere una complejidad de $O(k)$. Dado que el número de clientes (`MAX_CLIENTES`) está limitado a 10, el rendimiento es excelente para la escala del problema.

## Conclusiones y Trabajo Futuro
El proyecto ha logrado desarrollar exitosamente un sistema de mensajería funcional sin el uso de multiples hilos. La Tabla Hash demostró ser eficiente para la autenticación, y la Lista Enlazada proveyó la flexibilidad necesaria para la gestión de clientes. La implementación de `select()` facilitó un manejo concurrente de clientes de forma simple y efectiva en un solo hilo.

Para el trabajo futuro, se proponen las siguientes mejoras:
1.  **Seguridad Mejorada:** Implementar el hashing de contraseñas con un "salt" antes de almacenarlas, en lugar de guardarlas en texto plano.
2.  **Persistencia de Datos:** Conectar el servidor a una base de datos para que los usuarios registrados no se pierdan al reiniciar el servidor.
3.  **Funcionalidades Adicionales:**
    * Implementar mensajes privados entre usuarios.
    * Añadir un historial de mensajes que se envíe a los nuevos clientes al conectarse.
4.  **Escalabilidad:** Para soportar un número significativamente mayor de usuarios, se podría migrar de `select()` a `epoll` (en Linux) y considerar una arquitectura multihilo o basada en eventos para distribuir la carga de trabajo.

## Licencia
Este proyecto fue desarrollado en el marco de la Licenciatura en Ingeniería de Ciberseguridad e Infraestructura de Cómputo de la Universidad Veracruzana.

---
*Fecha de Creación: 9 de junio de 2025*
