# Demo Blue-Green

1. `docker compose up -d`
2. Visitar `http://localhost:8080` (Blue v1).
3. Probar Green desde la red del proxy: `docker compose exec proxy wget -qO- http://green/`
4. Editar `nginx/default.conf` y cambiar `proxy_pass http://blue:80;` por `proxy_pass http://green:80;`.
5. Validar: `docker compose exec proxy nginx -t`
6. Recargar sin reiniciar contenedor: `docker compose exec proxy nginx -s reload`
7. Visitar `http://localhost:8080` (Green v2).
8. Rollback: volver a `blue`, validar y recargar.
9. Limpieza: `docker compose down`.

Notas: El ejemplo usa dos contenedores de Nginx como servidores estáticos para minimizar requisitos. En un sistema real Blue y Green serían dos versiones independientes de la aplicación, con pruebas de salud y preparación, compatibilidad de datos y gestión de sesiones. No se garantiza cero interrupciones para conexiones persistentes.
