# Sistema de Mensajería LAN en C++

## Descripción del Proyecto
Este proyecto desarrolla un sistema de chat local (LAN) multicliente utilizando C++ y la API de Sockets. Permite a múltiples usuarios registrarse, autenticarse y comunicarse en tiempo real, gestionando las conexiones y los mensajes de forma eficiente mediante estructuras de datos como Tablas Hash y Listas Enlazadas.

## Autores
* Raúl Nava Soler
* Juan Elías Antonio Ramírez
* Kevin Marzuk Jerónimo Rojano

---

## Requisitos del Sistema

Para poder compilar y ejecutar este sistema de mensajería, necesitarás los siguientes elementos:

1.  **Sistema Operativo:**
    * Un entorno tipo **UNIX**. Esto incluye:
        * **Linux** (cualquier distribución como Ubuntu, Debian, Fedora, etc.).
        * **macOS**.
        * **Windows Subsystem for Linux (WSL)** en Windows. Si usas Windows, **se recomienda encarecidamente instalar WSL** (por ejemplo, con Ubuntu) para una mejor compatibilidad con las funciones de red y compilación.

2.  **Compilador de C++:**
    * Necesitarás un compilador de C++ compatible con el estándar C++11 o superior. El más común y recomendado es **G++ (GNU C++ Compiler)**.
    * **Cómo verificar si lo tienes instalado:** Abre una terminal y escribe `g++ --version`. Si ves información de la versión, ya lo tienes.
    * **Cómo instalarlo (si no lo tienes):**
        * **En Linux (Debian/Ubuntu):**
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
    * Todas las operaciones de compilación y ejecución se realizarán a través de la terminal o línea de comandos de tu sistema operativo.

## Manual de Uso Completo (Paso a Paso para Novatos)

Este manual te guiará detalladamente a través de cada paso para poner en marcha y utilizar el sistema de mensajería, tanto el servidor como el cliente.

### Paso 0: Preparación Inicial (¡Muy Importante!)

1.  **Descarga el Proyecto:**
    * Si estás en GitHub, puedes descargar el código fuente haciendo clic en el botón verde "**Code**" y luego seleccionando "**Download ZIP**". Descomprime la carpeta en una ubicación de tu elección.
    * Alternativamente, si usas `git`, puedes clonar el repositorio:
        ```bash
        git clone [https://github.com/kevinmaar/Sistema-de-mensajer-a-LAN-sin-hilos-en-C-.git]
        cd <nombre_de_la_carpeta_del_proyecto>
        ```
        

2.  **Abre tu Terminal:**
    * En Linux o macOS, busca la aplicación "Terminal".
    * En Windows, si ya instalaste WSL, abre la aplicación "Ubuntu" (o la distribución de Linux que hayas instalado).
    * Una vez abierta, navega hasta la carpeta donde descomprimiste o clonaste el proyecto usando el comando `cd` (change directory). Por ejemplo:
        ```bash
        cd /ruta/a/tu/carpeta/del/proyecto
        ```
        (Reemplaza `/ruta/a/tu/carpeta/del/proyecto` con la ubicación real de tu proyecto).

### Parte 1: Configuración y Ejecución del Servidor

El servidor es el corazón del sistema. Es el primero que debe estar activo para que los clientes puedan conectarse.

#### 1.1. Configurar Usuarios en el Servidor (`server.cpp`)

Para que los usuarios puedan iniciar sesión en tu servidor, primero debes registrarlos dentro del código.

1.  **Abre el archivo `server.cpp`:** Puedes usar un editor de texto simple (como Notepad en Windows, TextEdit en macOS, o `nano`/`gedit`/`vs code` en Linux/WSL).
2.  **Localiza la línea 23:** Busca la función `registrarUsuarios`. Dentro de esta función, encontrarás un espacio para añadir usuarios.
3.  **Añade tus usuarios y contraseñas:** Utiliza el siguiente formato para cada par de usuario y contraseña que quieras autorizar:
    ```cpp
    // Línea 23 (aproximadamente) en server.cpp
    void registrarUsuarios(TablaHash& tabla) {
        tabla.Registrar("magic", "tira1d20"); // Ejemplo de usuario: magic, Contraseña: tira1d20
        tabla.Registrar("usuario2", "contraseña2"); // Puedes añadir más usuarios aquí
        tabla.Registrar("admin", "passwordseguro"); // Otro ejemplo
    }
    ```
    * Asegúrate de que el "usuario" y la "contraseña" estén entre comillas dobles.
    * Cada línea debe terminar con un punto y coma `;`.
    * Puedes añadir tantos usuarios como necesites.
4.  **Guarda el archivo `server.cpp`** después de hacer los cambios.

#### 1.2. Compilar el Código del Servidor

Ahora que has configurado los usuarios, necesitas transformar el código C++ en un programa ejecutable.

1.  **Abre tu Terminal** (si la cerraste, vuelve a la carpeta del proyecto).
2.  **Ejecuta el comando de compilación:** Escribe exactamente el siguiente comando y presiona `Enter`:
    ```bash
    g++ hashhead.cpp ListaEnlazada.cpp server.cpp -o server
    ```
    * **¿Qué hace este comando?**
        * `g++`: Invoca al compilador de C++.
        * `hashhead.cpp`, `ListaEnlazada.cpp`, `server.cpp`: Son los archivos fuente de tu proyecto que el compilador necesita procesar.
        * `-o server`: Le dice al compilador que cree un archivo ejecutable llamado `server`.
    * **Si todo va bien:** La terminal no mostrará ningún mensaje de error y volverá a mostrar el prompt de entrada.
    * **Si hay errores:** Revisa cuidadosamente los mensajes de error. A menudo, indican un problema de sintaxis en `server.cpp` (por ejemplo, una coma o un punto y coma faltante). Vuelve a los pasos anteriores para corregir.

#### 1.3. Compartir la Dirección IP del Servidor

Para que los clientes sepan a dónde conectarse, necesitan la dirección IP del equipo donde se está ejecutando el servidor.

1.  **En la misma Terminal donde compilaste el servidor, ejecuta el siguiente comando para obtener tu IP local:**
    ```bash
    ip a
    ```
    * **Busca tu IP:** Dentro de la salida de este comando, busca una sección que empiece con `inet` seguida de una dirección numérica. Generalmente, las IPs locales empiezan con `192.168.x.x` o `10.x.x.x`.
    * **Ejemplo de lo que podrías ver:**
        ```
        2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
            link/ether 00:00:00:00:00:00 brd ff:ff:ff:ff:ff:ff
            inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic eth0
            ...
        ```
        En este ejemplo, la IP local es `192.168.1.100`.
    * **Anota esta dirección IP.** La necesitarás para configurar los clientes.
    * **Comparte esta IP** (y los usuarios/contraseñas que configuraste) con las personas que usarán el cliente.

#### 1.4. Ejecutar el Servidor

¡Ahora el servidor está listo para funcionar!

1.  **En la misma Terminal donde compilaste el servidor, ejecuta el siguiente comando:**
    ```bash
    ./server
    ```
    * **¿Qué hace este comando?** `./` indica que el programa `server` se encuentra en el directorio actual.
    * **El servidor iniciará:** Verás mensajes en la terminal indicando que el servidor está escuchando en el puerto `25565` y que ha creado un archivo `logs.txt`.
    * **Mantén esta terminal abierta:** El servidor debe permanecer ejecutándose en esta terminal para que los clientes puedan conectarse. No la cierres ni presiones `Ctrl+C` a menos que quieras detener el servidor.

#### 1.5. Detener el Servidor

Cuando quieras apagar el servidor:

1.  **Simplemente presiona la tecla `Enter`** en la terminal donde se está ejecutando el servidor.
2.  El servidor se detendrá, desconectará a todos los usuarios y modificará el nombre del archivo de log para incluir la fecha y hora de cierre (por ejemplo, `logs_2025-06-08_21-30-00.txt`).

### Parte 2: Configuración y Ejecución del Cliente

Los clientes son los programas que los usuarios ejecutarán para conectarse al servidor y chatear.

#### 2.1. Modificar el Código del Cliente (`cliente.cpp`)

Cada cliente necesita saber a qué dirección IP de servidor debe conectarse.

1.  **Abre el archivo `cliente.cpp`** con tu editor de texto.
2.  **Dirígete a la línea 25:** Busca la línea que contiene `inet_pton`.
3.  **Coloca la dirección IP del servidor:** Reemplaza el texto dentro de las comillas dobles con la dirección IP que obtuviste en el Paso 1.3.
    ```cpp
    // Línea 25 (aproximadamente) en cliente.cpp
    inet_pton(AF_INET, "IP_DEL_SERVIDOR", &servidor.sin_addr); // Ejemplo: inet_pton(AF_INET, "192.168.1.100", &servidor.sin_addr);
    ```
    * Asegúrate de que la IP esté entre comillas dobles.
4.  **Guarda el archivo `cliente.cpp`** después de hacer los cambios.

#### 2.2. Compilar el Código del Cliente

Al igual que el servidor, el cliente necesita ser compilado.

1.  **Abre una Terminal nueva** (o usa una diferente a la del servidor si la tienes abierta).
2.  **Navega a la carpeta del proyecto** (si no lo hiciste ya).
3.  **Ejecuta el comando de compilación:** Escribe exactamente el siguiente comando y presiona `Enter`:
    ```bash
    g++ cliente.cpp -o cliente
    ```
    * `-o cliente`: Le dice al compilador que cree un archivo ejecutable llamado `cliente`.
    * **Si todo va bien:** La terminal no mostrará ningún mensaje de error. Si hay errores, revísalos y corrige en `cliente.cpp`.

#### 2.3. Ejecutar el Cliente

¡Es hora de chatear!

1.  **Asegúrate de que el servidor esté encendido y funcionando** (Paso 1.4). El cliente no podrá conectarse si el servidor no está activo.
2.  **En la Terminal donde compilaste el cliente, ejecuta el siguiente comando:**
    ```bash
    ./cliente
    ```
3.  **El cliente intentará conectarse:** Si la IP es correcta y el servidor está activo, verás un mensaje de conexión exitosa.

### Parte 3: Interacción con el Programa (¡A Chatear!)

Una vez que el cliente se ha conectado, puedes empezar a interactuar con el sistema.

#### 3.1. Autenticación

1.  Cuando el cliente se conecte al servidor, verás dos mensajes pidiéndote información:
    ```
    Usuario:
    Password:
    ```
2.  **Ingresa el `Usuario:`** y presiona `Enter`.
3.  **Ingresa el `Password:`** (la contraseña no se mostrará mientras escribes por seguridad) y presiona `Enter`.
4.  **Mensajes de Autenticación:**
    * Si las credenciales son correctas y el usuario no está ya conectado, verás un mensaje de éxito y podrás empezar a escribir.
    * Si el usuario ya está conectado, verás un mensaje indicando que la sesión ya está activa.
    * Si las credenciales son incorrectas o el usuario no existe, se te notificará que las credenciales son inválidas.

#### 3.2. Envío de Mensajes

1.  Una vez que hayas iniciado sesión exitosamente, **todo lo que escribas en la terminal del cliente y presiones `Enter` será enviado al servidor.**
2.  El servidor recibirá tu mensaje y lo retransmitirá automáticamente a **todos los demás clientes conectados y autenticados**. Verás los mensajes de otros usuarios aparecer en tu propia terminal.

#### 3.3. Desconexión del Cliente

Para salir del chat:

1.  Simplemente **cierra la ventana de la terminal del cliente.**
2.  Alternativamente, puedes presionar `Ctrl+C` o `Ctrl+Z` en la terminal del cliente.
3.  El servidor detectará la desconexión del cliente y actualizará el estado del usuario, permitiendo que ese usuario pueda volver a iniciar sesión más tarde si lo desea.

---

## Características Principales
* **Servidor de Chat Multicliente:** Capaz de gestionar múltiples conexiones de clientes simultáneamente.
* **Autenticación de Usuarios:** Utiliza una Tabla Hash para almacenar y validar las credenciales de usuario (usuario y contraseña).
* **Gestión de Clientes Conectados:** Administra el estado de los clientes en tiempo real mediante una Lista Simplemente Enlazada.
* **Comunicación en Red:** Implementado con la API de Sockets para el intercambio de mensajes entre cliente y servidor.
* **Concurrencia con `select()`:** El servidor maneja múltiples clientes de manera concurrente en un único hilo de ejecución, evitando la complejidad de la programación multihilo.
* **Transmisión de Mensajes (Broadcast):** Los mensajes enviados por un cliente son retransmitidos a todos los demás usuarios conectados.
* **Registro de Eventos:** El servidor genera un archivo `logs.txt` para registrar eventos del sistema.

## Marco Teórico y Diseño

### Estructuras de Datos y Componentes Usados
Para la construcción de este sistema, se seleccionaron cuidadosamente dos estructuras de datos principales y la API de Sockets:

* **Tabla Hash para Gestión de Usuarios:** Implementada para almacenar y validar credenciales de usuario. Destaca por su eficiencia promedio de $O(1)$ en operaciones de búsqueda e inserción. Maneja colisiones mediante sondeo lineal.
    * **Archivos:** `hashheader.hpp`, `hashhead.cpp`
    * **Funcionalidades Clave:** `Registrar`, `iniciarsesion`, `Desconectar`.

* **Lista Simplemente Enlazada para Clientes Conectados:** Utilizada para gestionar dinámicamente a los clientes activos en el servidor. Su flexibilidad permite un crecimiento y decrecimiento del tamaño en tiempo de ejecución.
    * **Archivos:** `ListaEnlazada.hpp`, `ListaEnlazada.cpp`
    * **Funcionalidades Clave:** `insertarAlFinal` ($O(1)$), `eliminarNodo` ($O(k)$), `obtenerCabeza`, `ObtenerCantidad`.

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
El proyecto ha logrado desarrollar exitosamente un sistema de mensajería funcional aplicando las estructuras de datos vistas en el curso. La Tabla Hash demostró ser eficiente para la autenticación, y la Lista Enlazada proveyó la flexibilidad necesaria para la gestión de clientes. La implementación de `select()` facilitó un manejo concurrente de clientes de forma simple y efectiva en un solo hilo.

Para el trabajo futuro, se proponen las siguientes mejoras:
1.  **Seguridad Mejorada:** Implementar el hashing de contraseñas con un "salt" antes de almacenarlas, en lugar de guardarlas en texto plano.
2.  **Persistencia de Datos:** Conectar el servidor a una base de datos para que los usuarios registrados no se pierdan al reiniciar el servidor.
3.  **Funcionalidades Adicionales:**
    * Implementar mensajes privados entre usuarios.
    * Crear canales o salas de chat temáticas.
    * Añadir un historial de mensajes que se envíe a los nuevos clientes al conectarse.
4.  **Escalabilidad:** Para soportar un número significativamente mayor de usuarios, se podría migrar de `select()` a `epoll` (en Linux) y considerar una arquitectura multihilo o basada en eventos para distribuir la carga de trabajo.

## Licencia
Este proyecto fue desarrollado en el marco de la Licenciatura en Ingeniería de Ciberseguridad e Infraestructura de Cómputo de la Universidad Veracruzana.

---
*Fecha de Creación: 9 de junio de 2025*
