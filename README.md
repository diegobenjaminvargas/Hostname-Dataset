# Hostname Dataset

## Resumen
Esta práctica del módulo "Programación Python (avanzado)" sirivió para solidificar y confirmar el conocimiento adquirido en el curso. El objetivo consistió en crear un Dataset, mediante un programa que genere nombres de servidores. Además, para probar mis habilidades adquiridas mediante otras librerías, se solicitó crear un DataFrame de Pandas y generar una serie de gráficos con Matoplotlib. ¡Fue una práctica exitosa, que mereció un 10 de calificación!

## Instrucciones de la práctica
La práctica se divide en varios ejercicios que agregaban diferentes cantidades de puntuación a la calificación final. No era obligatorio realizar todos y se podía hacer una entrega parcial, realizando sólo los puntos que el alumno fuera capaz de hacer y considerando aprobada si la nota era igual o mayor a 5 puntos. Los ejercicios a realizar y sus puntuaciones eran los siguientes:

  1. Importar todas las librerías necesarias. (+0.15 puntos)
  2. Inicializar algunas variables que después se modificarían. (+0.15 puntos)
  3. Crear una función para generar los *hostnames* en base a unas reglas. (+1.5 puntos):
     - La función se ha de llamar `set_hostnames`y debe recibir un parámetro llamado `number_of_hosts` de tipo `int` que represente el número de hosts que queremos generar.
     - El *hostname* debe estar compuesto por un total de 8 caracteres alfanuméricos, las letras siempre en mayúsculas.
     - El primer caracter debe indicar el sistema operativo siendo `L` para `Linux`, `S`para `Solaris`, `A` para `AIX` y `H` para `HP-UX`. La proporción aproximada de sistemas operativos debe ser:
       - **Linux**: 40%
       - **Solaris**: 30%
       - **AIX**: 20%
       - **HP-UX**: 10%
     - El segundo caracter debe indicar el entorno, siendo `D` para `Development`, `I` para `Integration`, `T` para `Testing`, `S` para `Staging` y `P` para `Production`. La proporción aproximada de entornos debe se:
       - **Development**: 10%
       - **Integration**: 10%
       - **Testing**: 25%
       - **Staging**: 25%
       - **Production** 30% 
     - Los tres siguientes caracteres deben indicar el país, siendo `NOR` para `Norway`, `FRA` para `France`, `ITA` para `Italy`, `ESP` para `Spain`, `DEU` para `Germany` e `IRL` para `Ireland`. La proporción aproximada de países debe ser:
        - **Norway**: 6%
       - **France**: 9%
       - **Italy**: 16%
       - **Spain**: 16%
       - **Germany**: 23%
       - **Ireland**: 30%
     - Por último 3 dígitos que indiquen el número de nodo que ya existe para un mismo sistema operativo, entorno y país. El valor debe ser incremental, comenzando en `001` y con un valor máximo de `999`.
  4. Crear una función para obtener el nombre del SO. (+0.5 puntos)
  5. Crear una función para obtener el nombre del entorno. (+0.5 puntos)
  6. Crear una función para obtener el nombre del país. (+0.5 puntos)
  7. Crear una función para generar un DataFrame de Pandas. (+1 punto)
  8. Crear el DataFrame con la función anterior, pasando como argumento el entero 1500. (+0.2 puntos) 
  9. Guardar el DataFrame generado en un fichero CSV. (+0.5 puntos)
  10. Generar un único gráfico agrupando para cada país (`country`) los entornos (`environment`). (+0.5 puntos)
  11. Crear una figura con 4 gráficos en una malla de 2 filas y 2 columnas. (+4.5 puntos).
      - En la esquina superior izquierda debe aparecer un gráfico cuyo título sea `Type of OS grouped by country`. Debe ser de barras horizontales que representen una agrupación (`groupby`) por cada país de los sistemas operativos que tiene.
      - En la esquina superior derecha debe aparecer un gráfico cuyo título sea `Total Operating Systems`. Debe representar la cantidad total de sistemas operativos que hay en el DataFrame. Debe ser de tipo tarta (`pie`).
      - En la esquina inferior izquierda debe aparecer un gráfico cuyo título sea `Total hosts by country`. Debe ser un gráfico de barras horizontales que representen la cantidad total de hosts por cada país.
      - En la esquina inferior derecha debe aparecer un gráfico cuyo título sea `Hosts by country grouped by environment`. Debe representar un agrupación de hosts que hay por cada país y entorno. Debe ser de tipo barras.

## Herramientas utilizadas
- Jupyter Notebook, a través de [Anaconda](https://www.anaconda.com/anaconda-navigator)

## Aprendizajes de la práctica
### Función `zfill()`
En el ejercicio 3 de la práctica, que consistió en crear una función para generar los *hostnames*, para asignar los últimos 3 dígitos que indica el número de nodo que ya existe para un mismo sistema operativo, entorno y país, acudí a un método que no conocía antes: [`zfill()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.zfill.html). Éste es de la librería Pandas y sirve para anexar a la izquierda de un objeto *string* el número de caracteres '0' que se deseen. De esta manera, utilizando previamente el método `count()` para contar el número de veces que se repetía el valor alfabético, se pudieron generar 3 dígitos que fueran incrementando cada vez que se repitiera el valor alfabético y dando por resultado un *hostname* único. Ilustro a continuación el código dentro de la función `set_hostnames`, habiendo declarado las variables `os`, `environments`, `countries` para anexarlas en la lista `alpha_group`:
```python
for i in range(number_of_hosts): # `number_of_hosts` es el número de nombres de servidores que el usuario desea generar
        country = random.choice(countries)
        hostname = random.choice(os) + random.choice(environments)
        hostname += country
        alpha_group.append(hostname)
        hostname += str(alpha_group.count(hostname)).zfill(3) # Una vez anexando 'hostname' a 'alpha_group' y usando el método `count()`, se convierte a objeto `str`y se aplica el método `zfill()` para generar los 3 dígitos únicos
        hostnames.append(hostname) # Por último, se termina de anexar todos los nombres a la lista vacía de la variable 'hostnames' declarada previamente
```

### Variable global
Aprendí que normalmente, cuando se crea una variable dentro de una función, ésta es local y sólo se puede utilizar dentro de esa función. Pero en este caso, como era importante utilizar y manipular la variable `df` que debía contener el DataFrame con los datos de los *hostnames*, se debía declarar desde un inicio y fuera de la función que generaría el DataFrame. Para eso, desde el ejercicio 2 se declaró la variable `df` con el valor `None`, para indicar que esa variable permanecería vacía por el momento. Creando la función `set_dataframe` se llamó a dicha variable utilizando la palabra clave `global`. Debajo se ilustra de mejor manera:
```python
def set_dataframe(count: int) -> None:
    global df # Aquí es donde se llama a la variable 'df' dentro de la función
    
    set_hostnames(count)
    
    for hostname in hostnames:
        dataset.append({
            'hostname': hostname,
            'os': get_os(hostname),
            'environment': get_environment(hostname),
            'country': get_country(hostname),
            'node': int(hostname[-3:])
        })
        
    df = pd.DataFrame(dataset) # Al final es utilizada para almacenar el DataFrame de Pandas
```
### Color degradado en gráfico de barras utilizando la librería Seaborn
Para hacer que un gráfico de barra luciera más atractivo a la vista, me atreví a experimentar con [Seaborn](https://seaborn.pydata.org/), una librería de visualización de datos basada en Matplotlib, que ofrece una interfaz para crear gráficos más estéticos. 

Al generar el 'subplot' del ejercicio 11, titulado `Total hosts by country`, generé una nueva paleta de color verde basada en el conteo total de países de los *hostnames*. Almacenando el total de los países usados en el DataFrame en la variable `num_colors`, utilicé ésta como un atributo en la función `light_palette()` de Seaborn, que ayuda a crear una secuencia de color que va desde el tono más claro hasta el más oscuro. En el código debajo, muestro cómo generé el gráfico:

```python
num_colors = len(country_counts_sorted) # Se almacena en la variable el total de países del DataFrame (1500)
green_palette = sns.light_palette("green", n_colors=num_colors) # Se crea una nueva variable que almacenará la nueva paleta de color verde usando la función de Seaborn
sns.barplot(x=hosts_sorted, y=countries_sorted, ax=axs[1,0], palette = green_palette, orient='h') # Se genera el gráfico, donde se asigna al atributo 'palette' la variable que contiene la nueva paleta de color
```
Como resultado obtuve este gráfico que ayuda a visualizar de manera más atractiva el incremento del número de países:


## Conclusiones
Fue una práctica entretenida que, a pesar de hacerme reforzar todos los fundamentos de Python aprendidos en el módulo de Programación Básica, también me invitó a investigar y aprender por mi propia cuenta para optimizar y entregar un mejor resultado. 
