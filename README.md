# Git — Resumen por Clases
 
---
 
## Clase 1 — Introducción a Git y Configuración Inicial
 
### ¿Qué es Git?
 
Git es un **Sistema de Control de Versiones Distribuido (VCS)**. Permite guardar archivos, mantener un historial de cambios y trabajar de manera local sin depender de internet.
 
**Origen:** Creado por Linus Torvalds en aproximadamente 2 semanas, tras un conflicto con la herramienta BitKeeper que usaba el equipo del kernel Linux
 
### Instalacion
 
1. Pagina oficial: https://git-scm.com/install/
2. Descargar por sistema operativo
3. Verificar la instalación:
```bash
git --version
```
 
### Configuración inicial
 
```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@correo.com"
git config --global core.autocrlf true   # Recomendado en Windows
```
 
> Las configuraciones siguen la jerarquía **System < Global < Local**. Las locales (sin `--global`) tienen prioridad y aplican solo al repositorio actual
 
### Archivos importantes en un repositorio
 
| Archivo | Propósito |
|---------|-----------|
| `README.md` | Documentación del proyecto |
| `.gitignore` | Lista de archivos que Git debe ignorar |
 
---
