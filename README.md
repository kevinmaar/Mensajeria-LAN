# Ciberseguridad e Infraestructura de Cómputo  
## Sistema de Mensajería LAN sin Hilos en C++  

**Raúl Nava Soler**  
**Juan Elías Antonio Ramírez**  
**Kevin Marzuk Jerónimo Rojano**  
**9 de junio de 2025**  

---

## Índice

1. [Introducción y Planteamiento del Problema](#1-introducción-y-planteamiento-del-problema)  
2. [Marco Teórico (Estructuras y Componentes Usados)](#2-marco-teórico-estructuras-y-componentes-usados)  
   - [2.1. Tabla Hash para Gestión de Usuarios](#21-tabla-hash-para-gestión-de-usuarios)  
   - [2.2. Lista Simplemente Enlazada para Clientes Conectados](#22-lista-simplemente-enlazada-para-clientes-conectados)  
   - [2.3. Sockets y Concurrencia con select()](#23-sockets-y-concurrencia-con-select)  
3. [Diseño de la Solución](#3-diseño-de-la-solución)  
   - [3.1. Diagramas de clases UML](#31-diagramas-de-clases-uml)  
   - [3.2. Diagramas de flujo del servidor](#32-diagramas-de-flujo-del-servidor)  
4. [Manual de Uso](#4-manual-de-uso)  
5. [Análisis de Complejidad](#5-análisis-de-complejidad)  
6. [Conclusiones y Trabajo Futuro](#6-conclusiones-y-trabajo-futuro)  
7. [Referencias](#7-referencias)  
8. [Anexos](#8-anexos)  
9. [Resultados](#9-resultados)  

---

## 1. Introducción y Planteamiento del Problema

En el mundo de la computación distribuida, la comunicación entre procesos es fundamental.  
Los sistemas de mensajería o chat son un ejemplo clásico de esta necesidad, donde múltiples usuarios deben poder intercambiar información en tiempo real de manera eficiente y ordenada.

Este proyecto implementa un servidor de chat multicliente y su cliente en C++, utilizando estructuras de datos fundamentales para la gestión de usuarios y la comunicación en red.  
Simula un entorno de chat real donde los usuarios pueden registrarse, iniciar sesión y enviar mensajes tipo *broadcast*.

---

## 2. Marco Teórico (Estructuras y Componentes Usados)

### 2.1. Tabla Hash para Gestión de Usuarios

Se implementó una Tabla Hash para almacenar y validar las credenciales de los usuarios.  
Se eligió por su eficiencia en operaciones de búsqueda e inserción: **O(1)** promedio.

**Archivos de Implementación:**  
- `hashheader.hpp`  
- `hashhead.cpp`

**Componentes Clave:**  
- `struct Celda`: Guarda contraseña, usuario, estado y si está en línea.  
- `funcionHash`: Calcula índice usando suma de ASCII + módulo.  
- `Registrar`: Inserta nuevo usuario; evita duplicados y usa sondeo lineal.  
- `iniciarsesion`: Autentica; retorna códigos según estado.  
- `Desconectar`: Cambia estado a offline para permitir nuevo inicio.

---

### 2.2. Lista Simplemente Enlazada para Clientes Conectados

Usada para gestionar dinámicamente los clientes activos.

**Archivos de Implementación:**  
- `ListaEnlazada.hpp`  
- `ListaEnlazada.cpp`

**Componentes Clave:**  
- `struct Cliente`: Contiene socket, mensaje, usuario, contraseña, estado.  
- `class Nodo`: Nodo de lista enlazada con puntero al siguiente.  
- `insertarAlFinal`: Agrega cliente al final (**O(1)**).  
- `eliminarNodo`: Elimina cliente (**O(n)**).  
- `obtenerCabeza()`: Devuelve nodo inicial para recorrer lista.  
- `ObtenerCantidad()`: Número total de clientes conectados.

---

### 2.3. Sockets y Concurrencia con `select()`

Comunicación mediante API de sockets bajo TCP/IP.

**Cliente (`cliente.cpp`)**  
- Usa `select()` para escuchar simultáneamente entrada de usuario y mensajes del servidor.

**Servidor (`server.cpp`)**  
- `crearSocketServidor`: Crea y escucha en puerto 25565.  
- `aceptarCliente`: Autenticación vía Tabla Hash y agrega a la lista.  
- `manejarMensajes`: Itera sobre lista y gestiona mensajes.  
- Bucle principal con `select()` para manejar múltiples conexiones sin hilos.

---

## 3. Diseño de la Solución

### 3.1. Diagramas de clases UML

Representa relaciones entre clases.  
(*Ver figura 9.1*)

### 3.2. Diagramas de flujo del servidor

Ilustra la lógica del bucle principal del servidor.  
(*Ver figuras 9.2.1 y 9.2.2*)

---

## 4. Manual de Uso

### 1. Modificar `server.cpp`

Editar línea 23 para registrar usuarios autorizados.

### 2. Compilación de servidor

Usar terminal con compilador `g++`. Entorno UNIX (Linux/macOS/WSL).  

```bash
g++ server.cpp -o servidor
