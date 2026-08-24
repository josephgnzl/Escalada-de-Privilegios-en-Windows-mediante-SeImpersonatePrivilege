# Escalada de Privilegios en Windows mediante SeImpersonatePrivilege

<p align="center">
  <img src="https://img.shields.io/badge/Active%20Directory-Pentesting-red" alt="AD Pentesting">
  <p align="center">  <img src="https://img.shields.io/badge/josephgnzl-Pentesting-red" alt="AD Pentesting"></p>
</p>

## Descripción

Este repositorio documenta el proceso de escalada de privilegios en un sistema Windows mediante el abuso de `SeImpersonatePrivilege`, desde su identificación hasta la obtención y validación de privilegios elevados.

## Objetivo

Querer comprender cómo `SeImpersonatePrivilege` puede convertirse en un vector de escalada de privilegios y cómo identificar y aprovechar esta condición durante una evaluación de seguridad.

## Metodología

```text
Enumeración
    ↓
Identificación de SeImpersonatePrivilege
    ↓
Análisis del entorno
    ↓
Selección de técnica
    ↓
Explotación
    ↓
Validación
```
## ¿Qué es SeImpersonatePrivilege?

`SeImpersonatePrivilege` es un privilegio de Windows que permite a un proceso **impersonar el contexto de seguridad de un cliente autenticado**.

En términos sencillos:

```text
Cliente
   │
   │ Autenticación
   ▼
Servicio
   │
   │ Impersonation
   ▼
Contexto del cliente
```

## ¿Por qué nos interesa en Red Team?

Porque determinados servicios de Windows y cuentas de servicio pueden tener SeImpersonatePrivilege habilitado. Si conseguimos controlar un proceso que posee este privilegio, en determinadas condiciones podemos abusar de mecanismos de Windows para obtener un token con mayores privilegios.


