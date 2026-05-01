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
## Clase 3 — GitHub y Conexión Segura (SSH)
 
### Git vs GitHub
 
> **Git ≠ GitHub:** Git es el motor de versiones local. GitHub es la plataforma en la nube donde se alojan y comparten esos repositorios.
 
### SSH vs HTTPS
 
| | HTTPS | SSH |
|-|-------|-----|
| Autenticación | Pide credenciales en cada operación | Se configura una vez con una key |
| Comodidad | Tedioso | Sin interrupciones |
| Recomendado | No | **Siempre usar SSH** |
 
### Configurar SSH
 
```bash
# 1. Generar la clave SSH
ssh-keygen -t ed25519 -C "tu@correo.com"
 
# 2. Copiar la clave pública
cat ~/.ssh/id_ed25519.pub
 
# 3. Verificar la conexión
ssh -T git@github.com
```
 
Luego en GitHub: **Perfil → Settings → SSH and GPG Keys → New SSH Key**, pegar el contenido copiado.
 
### Conectar repositorio local con GitHub
 
```bash
# Agregar el repositorio remoto
git remote add origin git@github.com:TuUser/TuRepo.git
 
# Renombrar la rama principal a main
git branch -M main
 
# Subir los cambios por primera vez
git push -u origin main
```
 
### Comandos de sincronización
 
```bash
git push origin <rama>          # Subir commits al remoto
git pull origin <rama>          # Bajar y fusionar cambios del remoto
git clone <url>                 # Clonar un repositorio existente
git remote set-url origin <url> # Cambiar la URL del remoto (ej. HTTPS → SSH)
```
 
---


## Clase 4 — Remotos, SSH Múltiple y Git Checkout
 
### Gestión de remotos
 
```bash
git remote -v                    # Ver URLs remotas
git remote add <apodo> <url>     # Vincular repositorio local con uno remoto
git remote set-url <apodo> <url> # Cambiar la URL del remoto
```
 
### SSH múltiple (varias cuentas de GitHub)
 
Cada cuenta de GitHub necesita su propia llave SSH. Se gestiona con el archivo `~/.ssh/config`:
 
```bash
# Generar llave con nombre distinto
ssh-keygen -t ed25519 -C "micorreo@gmail.com" -f ~/.ssh/id_trabajo
```
 
```
# ~/.ssh/config
 
# Cuenta principal
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
 
# Cuenta secundaria
Host github-trabajo
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_trabajo
```
 
| Campo | Descripción |
|-------|-------------|
| `Host` | Alias de la conexión (lo que escribes después de `git@`) |
| `HostName` | Dirección real del servidor, siempre `github.com` |
| `User` | Usuario del sistema remoto, siempre `git` en GitHub |
| `IdentityFile` | Ruta exacta a la llave privada para ese Host |
 
Al clonar, usar el Host correspondiente:
```bash
git clone git@github-trabajo:usuario/repo.git
```
 
### Git Checkout
 
`git checkout` mueve el HEAD hacia un commit o rama específica. Sirve para inspeccionar el pasado, restaurar archivos y experimentar sin tocar la rama principal.
 
```bash
git checkout <hash>   # Ir a un commit específico
git checkout main     # Volver a la rama principal
```
 
### Estado Detached HEAD
 
Ocurre cuando HEAD apunta directamente a un commit fijo (no a una rama). En este estado eres un "espectador en el pasado": puedes ver todo, pero si te vas sin crear una rama, los cambios que hayas hecho se pierden.
 
```bash
# Si quieres conservar los cambios hechos en Detached HEAD:
git checkout -b <nueva-rama>
```
 
**Buenas prácticas:**
- Asegúrate de tener todo commiteado antes de moverte al pasado
- Si vas a escribir mucho código, crea una rama desde el inicio
- No trabajes mucho tiempo en estado Detached HEAD
---



