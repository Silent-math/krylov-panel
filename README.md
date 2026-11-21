# krylov-panel

Tablero interno del equipo. Un solo `index.html`, servido con GitHub Pages.

Lee `estado/*.md`, `WORKPLAN.md` y los commits del repositorio del equipo desde el navegador.

- **Límite de GitHub.** Sin token, la API da 60 consultas por hora *por red*, y la red del evento se comparte con otros equipos. Cuando se agota, el tablero pasa solo a «modo sin API»: sigue leyendo los estados y el plan desde `main` (con hasta 5 min de atraso) y deja de mostrar commits nuevos hasta que el límite se repone.
- **Para no depender del límite:** abrir una vez `https://silent-math.github.io/krylov-panel/#token=<token>` con un token de solo lectura de contenido público. Queda guardado en ese navegador y no se sube a ninguna parte.
