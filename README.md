# Sistema de Mensajería LAN sin Hilos en C++

## Descripción del Proyecto
Este proyecto implementa un sistema de mensajería local (LAN) multicliente basado en sockets, desarrollado en C++. El objetivo principal es simular un entorno de chat en tiempo real donde múltiples usuarios pueden registrarse, iniciar sesión y enviar mensajes que son transmitidos a todos los demás participantes conectados. [cite_start]La solución se enfoca en la aplicación de estructuras de datos fundamentales para la gestión eficiente de usuarios y la comunicación en red.

## Características Principales
* [cite_start]**Servidor de Chat Multicliente:** Capaz de gestionar múltiples conexiones de clientes simultáneamente.
* [cite_start]**Autenticación de Usuarios:** Utiliza una Tabla Hash para almacenar y validar las credenciales de usuario (usuario y contraseña).
* [cite_start]**Gestión de Clientes Conectados:** Administra el estado de los clientes en tiempo real mediante una Lista Simplemente Enlazada.
* [cite_start]**Comunicación en Red:** Implementado con la API de Sockets para el intercambio de mensajes entre cliente y servidor.
* [cite_start]**Concurrencia con `select()`:** El servidor maneja múltiples clientes de manera concurrente en un único hilo de ejecución, evitando la complejidad de la programación multihilo.
* [cite_start]**Transmisión de Mensajes (Broadcast):** Los mensajes enviados por un cliente son retransmitidos a todos los demás usuarios conectados.
* [cite_start]**Registro de Eventos:** El servidor genera un archivo `logs.txt` para registrar eventos.

## Tecnologías Usadas
* **C++:** Lenguaje de programación principal.
* [cite_start]**API de Sockets:** Para la comunicación en red (TCP/IP).
* **Estructuras de Datos Personalizadas:**
    * [cite_start]**Tabla Hash:** Para la gestión y autenticación de usuarios (complejidad promedio $O(1)$ para inserción y búsqueda).
    * [cite_start]**Lista Simplemente Enlazada:** Para la gestión dinámica de clientes conectados (complejidad $O(1)$ para inserción al final, $O(k)$ para eliminación y recorrido).
* [cite_start]**`select()`:** Para la concurrencia asíncrona de E/S en el servidor y cliente.

## Requisitos
Para compilar y ejecutar el sistema, se necesita:
* [cite_start]Un compilador de C++ (como `g++`).
* [cite_start]Un entorno tipo UNIX (Linux, macOS o WSL en Windows).

## Manual de Uso

### 1. Modificar `server.cpp`
Abre el archivo `server.cpp`. Dirígete a la línea 23 y registra los usuarios y contraseñas autorizadas para conectar al servidor con el siguiente formato:

```cpp
23 void registrarUsuarios (TablaHash& tabla) {
24    tabla.Registrar("USUARIO", "CONTRASEÑA"); // Ejemplo: tabla.Registrar("magic", "tira1d20");
}
