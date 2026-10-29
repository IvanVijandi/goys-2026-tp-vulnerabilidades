# Entregable — TP PHP2Shell / Node2Shell

**Grupo:** Grupo 06 · **Fork del grupo:** `https://github.com/IvanVijandi/goys-2026-tp-vulnerabilidades` · **Rama:** `grupo-06`
**Fecha:** jueves 29/10/2026

| Integrante | Usuario de GitHub | Rol en el TP |
|---|---|---|
| VIJANDI IVAN | `IvanVijandi` | Inventario |
| NICOLAS VALDES | _completar_ | Cambios/parche |
| FACUNDO OTERO | _completar_ | Detección |
| FRANCO PIETRANTUONO | _completar_ | Contención/comunicación |

> Roles sugeridos: inventario, cambios/parche, detección, contención/comunicación.
> Todos tienen que poder explicar su parte y una parte ajena.

---

## 1. Inventario

| Activo | Versión | ¿Expuesto a Internet? | ¿Procesa contenido no confiable? | ¿Afectado? | Evidencia |
|--------|---------|:---------------------:|:--------------------------------:|:----------:|-----------|
| Portal PHP | PHP 8.3.35 (NTS, built Oct 21 2026), Apache 2.4 con SAPI mod_php (`mpm_prefork`) | Sí | Sí | Sí | `php -v` devuelve `PHP 8.3.35`; `apache2ctl -M` muestra `php_module (shared)`; `ini_get('session.upload_progress.enabled')` devuelve `1` |
| API Node.js | Node.js v22.23.3 con Brotli 1.1.0 nativo del runtime, Express 5.3.0 | Sí | Sí | Sí | `node -p "process.version + ' / brotli ' + process.versions.brotli"` devuelve `v22.23.3 / brotli 1.1.0`; `npm ls --all` confirma ausencia de paquetes externos de brotli |
| Servicio Python | Python 3.11.18, Brotli 1.0.9 (PyPI) | No | Sí (condicional) | Sí | `python -c "import sys, brotli; print('python', sys.version.split()[0], 'brotli', brotli.__version__)"` devuelve `python 3.11.18 brotli 1.0.9` |

**Supuestos de exposición:**
1. **Red y perimetral:** En el laboratorio Docker, ningún servicio publica puertos al host (`ports: []`) y conviven en la red interna `labnet` (`internal: true`). En un escenario de producción (TransacRed S.A.), se asume un proxy reverso / gateway perimetral que expone el Portal PHP (`web01`) a Internet sobre HTTPS y enruta peticiones hacia la API Node.js (`api01`).
2. **SAPI y procesamiento pre-aplicación en PHP:** Al usar Apache con `mod_php`, la inicialización del request y el mecanismo de sesiones `session.upload_progress` son procesados directamente por el worker antes de que cualquier código de aplicación o capa de autenticación se ejecute, habilitando el vector RCE de PHP2Shell sin credenciales.
3. **Dependencias del Runtime vs Paquetes Externos en Node.js:** En la API Node.js, `npm audit` no reporta vulnerabilidades ya que el decodificador Brotli (v1.1.0) se encuentra compilado directamente en el runtime de Node.js v22.23.3 y no en el árbol de `node_modules`.
4. **Servicio Python:** Servicio backend interno confinado a la red `labnet` sin exposición directa a usuarios externos.

---

## 2. Priorización y decisión

| Activo | CVSS | Explotación real | Exposición | Criticidad | Decisión y plazo |
|--------|:----:|:----------------:|:----------:|:----------:|------------------|
| | | | | | |
| | | | | | |
| | | | | | |

**Justificación:** _¿por qué ese orden? ¿qué evidencia nueva cambiaría la decisión?_

---

## 3. Mitigación, parche y reversión

| Fecha/hora | Activo | Cambio | Antes | Después | Reversión |
|------------|--------|--------|-------|---------|-----------|
| | | | | | |
| | | | | | |

**Prueba funcional mínima:** _¿qué verificaron para saber que la app sigue viva?_

**Qué no se pudo probar:** _declararlo explícitamente._

---

## 4. Detección y caza

**Hipótesis:** _una frase: proceso, padre, horario, evidencia esperada._

**Hallazgos en `audit.log`:**

| Línea / timestamp | Host | Padre → hijo | ¿Anómalo? | Por qué |
|-------------------|------|--------------|:---------:|---------|
| | | | | |
| | | | | |

**¿Hay compromiso?** _sí/no/indeterminado, con evidencia._

**Regla propuesta para Node.js:** _qué proceso observa, qué condición y qué falso
negativo podría tener._

**Limitaciones del log:** _usuario, IP de origen, retención, ruido, etc._

---

## 5. Contención

| Control | Configuración | Evidencia | ¿Funciona? |
|---------|---------------|-----------|:----------:|
| Red sin salida | `internal: true` | | |
| Usuario sin privilegios | | | |
| Filesystem read-only | | | |
| Capabilities mínimas | | | |
| Límites de memoria/PIDs | | | |
| Rechazo de Brotli innecesario | | | |

**Prueba de salida a Internet:** _resultado en los tres servicios._

---

## 6. Conclusión (5 líneas)

_¿Qué harían distinto en las primeras 2 horas?_

---

## 7. Checklist antes de entregar

- [ ] El laboratorio arrancó y los 3 servicios quedaron `healthy`.
- [x] Inventario con evidencia (versión, SAPI, configuración).
- [ ] Matriz con decisión y plazo.
- [ ] Parche aplicado y verificado; reversión documentada.
- [ ] Al menos 3 controles de contención probados.
- [ ] `deteccion.md` completo.
- [ ] `informe.md` individual completo.
- [ ] Sin claves, tokens ni placeholders.
- [ ] Repo pusheado y PR abierto.

Checklist completa: [`../docs/checklist-entrega.md`](../docs/checklist-entrega.md).
