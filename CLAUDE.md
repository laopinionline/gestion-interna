# Panel de Gestion Interna — Grupo La Opinion

Panel interno de gestion del Grupo La Opinion de Pergamino. Single-page app (~8500 lineas) con inline JS/CSS, Firebase backend.

## Decisión estratégica

El panel tiene fecha de muerte: Capote absorbe el sistema de tareas. Solo se arregla lo que duele al equipo, no se pule lo que va a morir.

## Regla critica de deploy

El cron `/usr/local/bin/actualizar-panel.sh` en el VPS (186.148.233.122) baja `index.html` de GitHub cada 5 minutos y PISA lo que haya en produccion. Flujo obligatorio:

1. Editar `index.html` aca (local, `~/gestion-interna`)
2. `git add index.html && git commit -m "descripcion" && git push`
3. El cron despliega en menos de 5 min a https://opinion.vm.itecnis.com

Si alguien edita directo en el VPS sin pushear a GitHub, el cron lo revierte.

## Patron de escritura a Firestore

Toda escritura sigue este patron (post 2026-06-12):
1. Mutacion optimista local + render inmediato
2. `_markLocalChange('coleccion', docId)` para que onSnapshot no revierta
3. `db.collection().doc().update/set()` individual por documento
4. `.then()` muestra toast de exito solo tras confirmacion
5. `.catch()` muestra `toast('Error al guardar','err')` visible al usuario

Las funciones nucleares `guardarEnFirestore()` y `guardarYSincronizar()` fueron ELIMINADAS. No recrearlas.

## Contexto obligatorio

Al inicio de cada conversacion, leer estos archivos del vault de Obsidian:

- `/Users/imac/Desktop/OBSIDIAN/PERGA-STUDIO-BRAIN/02-CONTEXTO/LAOPINION/Panel-LaOpinion.md`
- `/Users/imac/Desktop/OBSIDIAN/PERGA-STUDIO-BRAIN/01-AGENTES/LAOPINION/Panel-Changelog.md`
- `/Users/imac/Desktop/OBSIDIAN/PERGA-STUDIO-BRAIN/05-INFRAESTRUCTURA-TECH/Panel-Reconexion.md`
- `/Users/imac/Desktop/OBSIDIAN/PERGA-STUDIO-BRAIN/00-SISTEMA/TODO-PROXIMA-SESION.md`
