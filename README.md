# 💬 Sistema de Mensajería LAN

Proyecto desarrollado en el marco de la experiencia educativa **Ciberseguridad e Infraestructura de Cómputo**. Este sistema permite la comunicación local entre múltiples usuarios mediante sockets TCP y manejo eficiente de estructuras de datos en C++.

---

### 👨‍💻 Autores

- Raúl Nava Soler  
- Juan Elías Antonio Ramírez  
- Kevin Marzuk Jerónimo Rojano  

📅 **Fecha de entrega:** 9 de junio de 2025  

---

## 📑 Índice

1. [Introducción y Planteamiento del Problema](#1-introducción-y-planteamiento-del-problema)  
2. [Marco Teórico](#2-marco-teórico-estructuras-y-componentes-usados)  
   - [2.1. Tabla Hash para Gestión de Usuarios](#21-tabla-hash-para-gestión-de-usuarios)  
   - [2.2. Lista Simplemente Enlazada para Clientes Conectados](#22-lista-simplemente-enlazada-para-clientes-conectados)  
   - [2.3. Sockets y Concurrencia con select()](#23-sockets-y-concurrencia-con-select)  
3. [Diseño de la Solución](#3-diseño-de-la-solución)  
4. [Manual de Uso](#4-manual-de-uso)  
5. [Análisis de Complejidad](#5-análisis-de-complejidad)  
6. [Conclusiones y Trabajo Futuro](#6-conclusiones-y-trabajo-futuro)  
7. [Referencias](#7-referencias)  
8. [Anexos](#8-anexos)  
9. [Resultados](#9-resultados)  

---

## 1. 🧠 Introducción y Planteamiento del Problema

La comunicación entre procesos es esencial en entornos distribuidos. Este sistema simula un entorno de chat en red local donde múltiples usuarios pueden registrarse, iniciar sesión y enviar mensajes a otros participantes conectados.

---

## 2. 🧩 Marco Teórico (Estructuras y Componentes Usados)

### 2.1. Tabla Hash para Gestión de Usuarios

- **Archivos:**  
  - `hashheader.hpp`  
  - `hashhead.cpp`

- **Funciones clave:**  
  - `funcionHash()`  
  - `Registrar()`  
  - `iniciarsesion()`  
  - `Desconectar()`  

Esta tabla gestiona usuarios y contraseñas, permitiendo autenticación segura y control de sesiones activas.

---

### 2.2. Lista Simplemente Enlazada para Clientes Conectados

- **Archivos:**  
  - `ListaEnlazada.hpp`  
  - `ListaEnlazada.cpp`

- **Funciones clave:**  
  - `insertarAlFinal()`  
  - `eliminarNodo()`  
  - `obtenerCabeza()`  
  - `ObtenerCantidad()`  

Permite administrar en tiempo real los clientes conectados al servidor.

---

### 2.3. Sockets y Concurrencia con select()

- **Cliente (`cliente.cpp`)**  
  Uso de `select()` para escuchar entrada del usuario y mensajes del servidor simultáneamente.

- **Servidor (`server.cpp`)**  
  - `crearSocketServidor()`  
  - `aceptarCliente()`  
  - `manejarMensajes()`  
  - Bucle principal con `select()` para gestión multicliente sin multihilo.

---

## 3. 🧪 Diseño de la Solución

- **Diagramas UML:**  
  Muestran relaciones entre clases y estructuras.

- **Diagramas de flujo del servidor:**  
  Ilustran la lógica del servidor desde la conexión hasta la transmisión de mensajes.

---

## 4. 🛠 Manual de Uso

### 1. Modificar `server.cpp`
En la línea 23, registra los usuarios y contraseñas válidos.

### 2. Compilar el servidor
```bash
g++ server.cpp ListaEnlazada.cpp hashhead.cpp -o servidor
