# unix-commands
🧪 EJERCICIO 1: Logs (MUY COMÚN)
🎯 Problema

Tienes un archivo app.log y quieres:

Contar cuántas veces aparece la palabra ERROR

✅ Solución básica
grep -c "ERROR" app.log
🧠 Paso a paso
grep → busca texto
"ERROR" → patrón
-c → cuenta ocurrencias
🔥 Nivel entrevista (mejor respuesta)
cat app.log | grep "ERROR" | wc -l

👉 Y explicas:

grep filtra
wc -l cuenta líneas

💥 Bonus:

grep -i "error" app.log | wc -l

👉 ignora mayúsculas

🧪 EJERCICIO 2: Top errores únicos
🎯 Problema

Encuentra los errores más frecuentes en un log

✅ Solución
grep "ERROR" app.log | sort | uniq -c | sort -nr
🧠 Paso a paso
grep ERROR → filtra errores
sort → agrupa iguales
uniq -c → cuenta
sort -nr → ordena mayor a menor
💥 Output esperado
45 NullPointerException
20 TimeoutException
5 DBConnectionError
🧪 EJERCICIO 3: Encontrar archivos grandes
🎯 Problema

Encuentra los archivos más grandes en el sistema

✅ Solución
du -ah / | sort -rh | head -n 10
🧠 Explicación
du -ah → tamaños
sort -rh → ordena por tamaño
head → top 10
🧪 EJERCICIO 4: Proceso que consume CPU
🎯 Problema

Tu servidor está lento, identifica el proceso

✅ Solución
top
🧠 Nivel entrevista
ps aux --sort=-%cpu | head

👉 muestra los más pesados

🧪 EJERCICIO 5: Puerto ocupado
🎯 Problema

Tu app no levanta en puerto 8080

✅ Solución
lsof -i :8080
🧠 Luego:
kill -9 <PID>
🧪 EJERCICIO 6: Buscar texto en múltiples archivos
🎯 Problema

Encuentra "Exception" en todos los archivos

✅ Solución
grep -r "Exception" .
💥 Nivel pro
grep -rn "Exception" .

👉 incluye número de línea

🧪 EJERCICIO 7: Script simple (MUY común)
🎯 Problema

Crear script que cuente errores en todos los logs

✅ Solución
#!/bin/bash

for file in *.log; do
  echo "$file: $(grep -c ERROR $file)"
done
🧠 Explicación
loop archivos
grep -c cuenta
imprime resultado
🧪 EJERCICIO 8: Filtrar IPs únicas
🎯 Problema

Tienes logs con IPs, muestra las únicas

✅ Solución
awk '{print $1}' access.log | sort | uniq
💥 Nivel pro
awk '{print $1}' access.log | sort | uniq -c | sort -nr

👉 frecuencia de IPs

🧪 EJERCICIO 9: Espacio en disco
🎯 Problema

El disco está lleno

✅ Solución
df -h
🧠 Luego:
du -sh *
🧪 EJERCICIO 10: Pipeline completo (nivel CoderPad 🔥)
🎯 Problema

Encuentra los errores más comunes en logs y ordénalos

✅ Solución
cat app.log | grep ERROR | awk '{print $2}' | sort | uniq -c | sort -nr
🧠 Paso a paso
grep ERROR → filtra
awk → extrae campo
sort → agrupa
uniq -c → cuenta
sort -nr → ranking
⚠️ Tips clave para entrevista
🔥 1. Siempre explica mientras escribes

No solo pongas comandos

🔥 2. Empieza simple → mejora

Ejemplo:

grep ERROR
luego pipeline completo
🔥 3. Usa pipes (CRÍTICO)

👉 Es lo que más evalúan

🏆 Resumen brutal

Si dominas:

grep
awk
sort
uniq
ps
top
du
lsof