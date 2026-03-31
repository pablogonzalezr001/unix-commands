🚨 ESCENARIO 1: API caída (puerto no responde)
🎯 Problema

Tu API en :8080 no responde

🧠 Paso 1: ¿está corriendo el proceso?
ps aux | grep java

👉 Si NO aparece:

❌ la app está caída → revisa logs

🧠 Paso 2: revisar logs
tail -n 100 app.log

o en vivo:

tail -f app.log
🧠 Paso 3: ¿puerto abierto?
lsof -i :8080

👉 Si no hay nada:

la app nunca levantó

🧠 Paso 4: probar local
curl http://localhost:8080
💥 Diagnóstico típico
Exception en logs
Puerto ocupado
Crash por config
🚨 ESCENARIO 2: CPU al 100% 🔥
🎯 Problema

Servidor lentísimo

🧠 Paso 1: identificar proceso
top

o mejor:

ps aux --sort=-%cpu | head
🧠 Paso 2: investigar proceso
ps -fp <PID>
🧠 Paso 3: logs del proceso
tail -f app.log
💥 Posibles causas
loop infinito
query pesada
memory leak
🔥 Acción rápida
kill -9 <PID>

👉 (solo si es crítico)

🚨 ESCENARIO 3: Memoria llena
🎯 Problema

App se cae (OOM)

🧠 Paso 1: revisar memoria
free -m
🧠 Paso 2: procesos por memoria
ps aux --sort=-%mem | head
🧠 Paso 3: revisar GC (Java)
jstat -gc <PID>
💥 Diagnóstico
memory leak
heap mal configurado
🚨 ESCENARIO 4: Disco lleno 💣
🎯 Problema

“No space left on device”

🧠 Paso 1: verificar
df -h
🧠 Paso 2: encontrar culpable
du -ah / | sort -rh | head -n 20
🧠 Paso 3: logs gigantes
ls -lh *.log
🔥 Solución rápida
truncate -s 0 app.log
🚨 ESCENARIO 5: Muchas requests / ataque
🎯 Problema

Servidor saturado

🧠 Paso 1: IPs activas
netstat -an | grep :80 | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr
💥 Resultado:
200 192.168.1.10
50 10.0.0.2
🔥 Acción
bloquear IP
rate limiting
🚨 ESCENARIO 6: App funciona local pero no en server
🧠 Debug checklist
1. Puerto abierto
ss -tulnp
2. Firewall
iptables -L
3. Curl interno
curl localhost:8080
4. Curl externo
curl <ip>:8080
💥 Diagnóstico típico
firewall
binding localhost vs 0.0.0.0
🚨 ESCENARIO 7: Kubernetes (nivel pro 🔥)
🎯 Problema

Pod no levanta en Kubernetes

🧠 Paso 1: ver pods
kubectl get pods
🧠 Paso 2: describir pod
kubectl describe pod <pod>
🧠 Paso 3: logs
kubectl logs <pod>
💥 Errores comunes
CrashLoopBackOff
ImagePullBackOff
config error
🚨 ESCENARIO 8: Imagen no despliega (GitOps)
🎯 Problema

Subiste nueva imagen pero no se refleja

🧠 Paso 1: revisar Helm
kubectl get deployment
🧠 Paso 2: revisar imagen
kubectl describe deployment

👉 Ver:

image: repo/app:OLD_TAG
💥 Diagnóstico

❗ No actualizaste values.yaml

🔥 Solución

Actualizar:

image:
  tag: NEW_SHA

👉 commit → despliegue

🧠 Cómo responder en entrevista (CLAVE)

Siempre sigue este orden:

1. Observabilidad
logs
métricas
2. Identificación
proceso
recurso
3. Diagnóstico
hipótesis
4. Acción
fix o workaround
🏆 Resumen brutal

👉 En producción SIEMPRE revisas:

ps
top
grep
tail
df
du
lsof
netstat
kubectl