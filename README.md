# Experimento A/B en página de inicio

En este proyecto analicé un experimento A/B que comparaba dos versiones de una landing page (A y B). La idea era revisar los datos y recomendar cuál versión conviene implementar.

## Sobre los datos

Usé el archivo `landing_experiment.csv`, que tiene 40,000 usuarios que vieron una de las dos páginas durante enero de 2026 (del 1 al 28). Cada fila es un usuario distinto.

Las columnas son:

- `user_id`: identificador del usuario
- `date`: fecha en la que vio la página
- `landing`: versión de la página (A o B)
- `region`: región del usuario
- `dispositivo`: Mobile o Desktop
- `traffic_source`: canal por el que llegó (Organic, Ads, Email o Referral)
- `user_type`: Nuevo o Recurrente
- `converted`: 1 si convirtió, 0 si no
- `gasto`: cuánto gastó (0 si no convirtió)

## Qué quería responder

- ¿Los datos son confiables para sacar conclusiones?
- ¿Qué página tiene mayor gasto promedio entre quienes compraron?
- ¿Qué página convierte más?
- ¿La fuente de tráfico tiene relación con la conversión?
- ¿El tipo de usuario tiene relación con la conversión?

## Cómo lo hice

Primero revisé la calidad de los datos y después apliqué pruebas estadísticas con un nivel de significancia de 0.05:

1. **Revisión de datos:** nulos, duplicados, categorías y valores raros.
2. **Gasto promedio (A vs B):** prueba t de Student, solo con usuarios que convirtieron.
3. **Tasa de conversión (A vs B):** prueba Z de proporciones.
4. **Fuente de tráfico vs conversión:** chi-cuadrado de independencia.
5. **Tipo de usuario vs conversión:** chi-cuadrado de independencia.
6. **Gráficos** para apoyar los resultados de las variables categóricas.
7. **Conclusiones y recomendaciones** para el negocio.

### Revisión de los datos

- No hay valores nulos y todos los `user_id` son únicos.
- Los grupos están balanceados: 19,982 usuarios en A y 20,018 en B.
- No encontré errores en las categorías ni gastos negativos, y el gasto solo es mayor a 0 cuando el usuario convirtió.
- Tuve que convertir la columna `date` a tipo fecha.

## Resultados

**Tasa de conversión:** la página B convierte más. B tiene 15.96% y A tiene 12.57%, una diferencia de 3.38 puntos porcentuales. La diferencia es significativa (p ≈ 3.8e-22).

**Gasto promedio:** entre los usuarios que convirtieron, el gasto promedio en B fue de 68.75 y en A de 61.09, es decir 7.66 más. También es una diferencia significativa (p ≈ 1.1e-20).

**Fuente de tráfico:** sí hay relación con la conversión (p = 0.034), pero el valor está cerca del límite y las diferencias entre canales son pequeñas. Email (14.99%) y Ads (14.74%) convierten un poco mejor que Referral (13.88%) y Organic (13.79%). Organic es el canal que más conversiones aporta, pero solo porque trae más usuarios.

**Tipo de usuario:** no encontré relación con la conversión (p = 0.474). Los usuarios nuevos convierten al 14.36% y los recurrentes al 14.09%, prácticamente lo mismo.

## Conclusiones y recomendaciones

- Recomiendo **implementar la página B**, porque convierte más y sus clientes gastan más en promedio. Como los usuarios se asignaron al azar y los grupos están balanceados, la mejora en conversión se puede atribuir a la página.
- Se puede **reforzar Email y Ads**, que tienen mejor tasa de conversión. Como las diferencias son pequeñas, conviene revisar los costos de cada canal antes de mover presupuesto.
- **No tiene mucho sentido segmentar por tipo de usuario**, porque no hay diferencia en la conversión.
- Después de implementar B, sería bueno **seguir midiendo** conversión y gasto unas semanas para confirmar que el resultado se mantiene.

## Limitaciones

- El análisis de gasto solo incluye a quienes convirtieron, así que no puedo afirmar que B sea la causa directa de que gasten más. Solo veo que están asociados.
- Las pruebas dicen que B funciona mejor, pero no explican qué parte del diseño o del mensaje lo logra.
- Los usuarios no fueron asignados al azar a la fuente de tráfico ni al tipo de usuario, entonces esos resultados son relaciones y no causas.

## Archivos del proyecto

- `S9 Version_Student_Proyecto_Landing_Experiment.ipynb`: notebook con todo el análisis
- `landing_experiment.csv`: datos del experimento
- `README.md`: este archivo

Nota: en el notebook cargo los datos desde `/datasets/landing_experiment.csv`. Si lo corres en tu computador, hay que cambiar esa ruta.

## Herramientas

Python, pandas, scipy, statsmodels, seaborn, matplotlib y Jupyter Notebook.

## Cómo correrlo

```bash
pip install pandas scipy statsmodels seaborn matplotlib jupyter
jupyter notebook "S9 Version_Student_Proyecto_Landing_Experiment.ipynb"
```

Después solo hay que ejecutar las celdas en orden.
