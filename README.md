# Krylov · tablero en vivo (minería)

Página estática que muestra cómo va Krylov contra el plan del pivote a minería (decisión D-37). Está en https://silent-math.github.io/krylov-panel/ y es un solo archivo, `index.html`, sin compilación.

Lee del navegador, directo de `platanus-hack/platanus-hack-26-cl-team-5` (rama `main`):

- `estado/TABLERO.md`, `WORKPLAN.md` y los tres `estado/<rol>.md`;
- `motor/artefactos/<motor>/sello.json` de `espacial`, `deformacion`, `cinetica`, `desgaste` y `sismico` (si no existe, dice «sin sello todavía»);
- los últimos commits, por la API de GitHub.

Pregunta por el último commit cada 90 segundos y solo vuelve a leer los archivos cuando cambió. No usa servidor ni guarda nada. No reemplaza a `estado/TABLERO.md`: lo dibuja.

## Qué muestra

1. **Cuentas regresivas:** producto terminado (sáb 18:00), tope de redes (sáb 20:30), entrega (dom 08:00), pitch (dom 09:30), el próximo hito y hace cuánto se actualizó el tablero. Debajo, hasta tres alertas.
2. **Las cinco decisiones:** por cada caso, sus seis etapas (datos, simulador, método lento, red, sello, en la app), quién responde hoy esa tarjeta y lo medido en su `sello.json`. Avisa cuando el tablero y el sello no coinciden.
3. **Entrenamiento:** qué entrena cada MacBook ahora y el calendario de las dos rondas.
4. **Los tres computadores:** semáforo, en qué está cada uno y qué sigue.
5. **Plan del sábado:** las tareas de `WORKPLAN.md` (P-21, A-21, S-21 en adelante) de 12:00 a 21:00, con su estado; también como tabla. Debajo, la tarde y el domingo.
6. **Últimos commits** y el tablero completo.

Mientras `main` tenga el plan anterior dice «Esperando el pivote en main» y no se rompe.

## Qué formato espera de cada archivo

**`estado/TABLERO.md`**

- `**Actualizado:** sáb 21-nov 12:40 · …` (toma la primera hora; «dom» suma un día).
- `## Los cinco casos`: una tabla con las columnas `Caso | Motor | Datos | Simulador | Método lento | Red | Sello | En la app | Responde`, una fila por caso (`leyes`, `taludes`, `flotacion`, `flota`, `sismicidad`). Cada etapa empieza con `listo`, `en curso`, `pendiente`, `no pasa` o `cortado` (puede seguir una nota). `Responde` es `red`, `precalculado`, `ejemplo` o `nada`. El orden de las filas es el orden en pantalla.
- `## Semáforo`: tabla `Rol | Estado | Nota`; el rol empieza con `producto`, `amortizacion` o `seguimiento`, y el estado lleva 🟢, 🟡 o 🔴.
- `## Siguiente tarea …`: una viñeta por computador que empieza con `**PC 1 · producto:**`.

**`WORKPLAN.md`**

- Tareas: filas `| P-21 | 12:30 | 40 | Tarea | Entregable | Listo cuando |`. Solo se dibujan las de número 21 o más. La hora es el tope; `dom 06:30` vale para el domingo.
- Hitos: `**H4 · sábado 14:30 · ¿Pregunta?**` (también `**H4 · 14:30**`) y `**Tope de redes · sábado 20:30.**`. H0 a H3 se ignoran.

**`estado/<rol>.md`**

- `Persona: Nombre · …` en el encabezado.
- `## Hecho`: una viñeta por cosa hecha. **Una tarea cuenta como hecha si su id va en el sujeto de la línea**, antes del primer punto o dos puntos: `- sáb 13:05 · A-23 terminada: …`. Delante puede ir el día, la fecha y la hora en cualquier formato (`sáb 12:40 ·`, `09:55:17`, `sáb 21-nov 13:05:`) y también `Sesión B:`. Valen dos ids (`A-24 y A-25 terminadas`) y rangos (`S-30 a S-32`). Si la hora anotada es anterior al tope, sale «hecha antes de su hora».
- `## En curso`: el id en el sujeto de la línea está en curso; lo que va después de «Sigue:», «Siguiente» o «Después» es la siguiente.
- `## Bloqueos`: cualquier viñeta que no sea `(ninguno)` se muestra como alerta roja.
- `## Pedidos a otros`: se listan los tres últimos.
- `## Entrenamiento en curso` (PC 2 y PC 3): tabla de dos columnas con la fila `Motor` (el id entre comillas invertidas, por ejemplo `` `cinetica` ``; `(ninguno…)` si no hay nada), la fila `Hora de inicio` y, en el comando, `--hasta HH:MM`.

**`motor/artefactos/<motor>/sello.json`**: se leen `pasa` (o `sbc_pasa`), `ensayo`, `simulaciones_entrenamiento`, `entrenamiento_min`, `cobertura_90`, `latencia_ms`, `mcmc_s_por_dataset` y `maquina`. Un sello con `ensayo: true` no cuenta como aprobado.

**Commits**: el rol de cada uno se deduce del mensaje (`estado producto …`, `ronda …`, `tablero …`, `H4 …`, `pivote …`, `P-21: …`) y, si no, del último rol que usó ese autor.

Lo que está fijo en `index.html` y hay que cambiar a mano si el plan cambia: las cuatro fechas de las cuentas regresivas, el calendario de rondas (`RONDAS`), la lista de la tarde y del domingo (`AGENDA`) y la pregunta de cada caso (`CASOS_INFO`).

## Opciones en la dirección

| Qué | Para qué |
|---|---|
| `#token=…` | Un token de GitHub de solo lectura, una vez; queda en ese navegador. Sin token la API da 60 consultas por hora por red, y la del evento se comparte. Si se agota, la página pasa sola a «modo sin API» (lee `main` con hasta 5 minutos de atraso) |
| `?tema=oscuro` o `?tema=claro` | Fija el tema (también está el botón «tema») |
| `?ahora=15:45` o `?ahora=dom-07:10` | Fija el reloj, para ensayar cómo se verá a otra hora |
| `?base=/ruta` | Lee los archivos de otra carpeta servida desde el mismo origen, sin GitHub. Para probar antes de un push: `python3 -m http.server` en la carpeta que contiene este repositorio y el clon, y abrir `…/krylov-panel/index.html?base=/<carpeta-del-clon>` |

## Cómo se publica

`git push` a `main` de `Silent-math/krylov-panel`; GitHub Pages lo sirve en uno o dos minutos. Las funciones que leen los archivos están entre `// PARSE-START` y `// PARSE-END` y no tocan la página: se pueden probar con node contra los archivos del repositorio.
