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
# Clase 2 — Estados de Git y Buenas Prácticas de Commits
 
### Los 3 estados de Git
 
```
Directorio de trabajo → (git add) → Stage area → (git commit) → Repositorio local
```
 
| Estado | Zona | Descripción |
|--------|------|-------------|
| Modified | Directorio de trabajo | Cambios locales sin registrar. Archivos *Untracked* (nuevos) o *Modified* (cambiados). |
| Staged | Stage area | Cambios seleccionados listos para el próximo commit. |
| Committed | Repositorio local | Cambios guardados en el historial con un hash único. |
 
### Comandos de gestión de estados
 
```bash
git add <archivo>              # Pasar un archivo al stage
git add .                      # Stagear todos los archivos
git restore <archivo>          # Descartar cambios locales (irreversible)
git restore --staged <archivo> # Sacar un archivo del stage
git commit -m "mensaje"        # Confirmar cambios
git reset --soft HEAD~1        # Deshacer el último commit (cambios vuelven al stage)
```
 
### Buenas prácticas de commits
 
#### Commits atómicos
Cada commit debe representar **un único cambio lógico, pequeño y completo**. Varios commits pequeños son mejor que uno grande con todo mezclado.
 
#### Formato del mensaje
 
```
<tipo>: <descripción breve>
```
 
- Máximo **50 caracteres** en el título
- **Sin punto final** ni puntos suspensivos
- Verbo **imperativo en inglés** (Add, Fix, Remove, Change...)
#### Prefijos semánticos
 
| Prefijo | Cuándo usarlo |
|---------|---------------|
| `feat` | Nueva funcionalidad para el usuario |
| `fix` | Corrección de bug |
| `docs` | Cambios en documentación |
| `refactor` | Reestructuración de código sin cambio funcional |
| `style` | Formato, espacios, tabulaciones (sin impacto funcional) |
| `test` | Tests nuevos o refactorización de tests |
| `build` | Cambios en sistema de build o despliegue |
| `perf` | Mejora de rendimiento |
| `ci` | Cambios en integración continua |
 
**Ejemplos:**
```bash
git commit -m "feat: add login form validation"
git commit -m "fix: resolve null pointer on user profile"
git commit -m "docs: update README with SSH setup guide"
```
 
---
