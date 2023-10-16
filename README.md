# Genéticos y 8 reinas 

------------
![Portada](https://github.com/monseher00/Geneticos_y_8_reinas/assets/92997919/b79e1b25-9fe8-4ae1-aef7-14319da2db4b)

Lizet Monserrath Hernández Reyes.

Matrícula: S19014453.

------------
**Table of Contents**

[TOCM]

[TOC]

### Introducción

**“Las 8 reinas”**es uno de los problemas clásicos de la inteligencia artificial, en particular, en el estudio y desarrollo de algoritmos genéticos. Consiste en encontrar una solución para la colocación aleatoria de 8 reinas en un tablero de ajedrez, de manera que ninguna reina amenace a las demás. Aunque en apariencia no representa gran complejidad, el ejercicio plantea desafíos importantes debido a las restricciones implícitas que rigen la disposición de las reinas: ninguna de ellas puede estar en la misma fila, columna o diagonal que otra reina. 

El ejercicio se plantea desde una visión e implementación de algoritmos genéticos, los cuales son una técnica de optimización inspirada en la evolución biológica que se ha utilizado con éxito en una amplia variedad de problemas de búsqueda y optimización. La relación entre el problema de las 8 reinas y los algoritmos genéticos radica en cómo estos últimos pueden emplearse como una herramienta efectiva para encontrar soluciones aproximadas al problema. Al modelar la evolución de posibles soluciones utilizando conceptos como selección de padres, cruza, mutación y reemplazo, los algoritmos genéticos pueden abordar el desafío de encontrar una disposición válida de las reinas en el tablero de ajedrez. En otras palabras, el ejercicio permite imitar el proceso evolutivo, permitiendo que las soluciones prometedoras sean seleccionadas, recombinadas y ajustadas gradualmente para acercarse a una disposición de reinas que cumpla con todas las restricciones planteadas. 

### Lenguaje implementado y bibliotecas

- Python;
 - Matplotlib.
 

### Tabla de datos

| Dato  |  Técnica |
| ------------ | ------------ |
|  Inicialización | Aleatoria  |
| Selección de padres  | Ruleta  |
|  Cruza | Mapeo parcial  |
|  Mutación | Inserción  |
|  Reemplazo | Brecha generacional  |

#### Inialización

Para la inicilización aleatoria de las reinas en el tablero de ajedrez, se creará una lista llamada "tablero" la cual representa, en efecto, el tablero de ajedrez tradicional. Para representar los espacios vacíso se utilizará la siguiente representación:
- Espacios vacíos, es decir, sin pieza: ceros (0). 
- Las reinas se representarán con unos (1). 

**Para la construcción del código importamos el módulo random para generar números aleatorios.**

#### Selección de padres

El método empleado en el código ilustra cómo seleccionar dos padres de la población en función de sus aptitudes utilizando un rango de probabilidad.  Se utiliza la técnica de la ruleta, la cual involucra calcular el fitness individual, ordenar los individuos, calcular la probabilidad acumulada y seleccionar al individuo que alcanza el número aleatorio $n_r$. 

#### Cruza

Se emplea la técnica del mapeo parcial para realizar la cruza entre dos individuos representados como vectores. Esta técnica implica una recombinación de permutación.
En esta implementación, el "mapa" representa el mapeo que se aplicará para realizar la cruza entre los dos padres. Se eligen dos puntos de corte aleatorios en los padres y luego se aplica el mapeo parcial para generar dos hijos.

#### Mutación

En la mutación por inserción, se elige aleatoriamente un gen y se inserta en una posición diferente en el cromosoma. 

De manera general, se toma un individuo como entrada, copia el individuo para no modificar el original y luego selecciona dos posiciones aleatorias distintas en el cromosoma. Luego, toma un gen de la primera posición y lo inserta en la segunda posición.

#### Reemplazo

La técnica de "becha generacional" es un método de reemplazo en algoritmos genéticos donde toda la población existente es reemplazada por la descendencia generada en una nueva generación.

### Código

#### Creación del tablero de ajedrez

```python:
# Creación del tablero de ajadrez
tablero = [[0 for i in range(8)] for i in range(8)]

# Colocar una reina en cada fila del tablero
for fila in range(8):
    columna = random.randint(0, 7)
    tablero[fila][columna] = 1

for fila in tablero:
    print(fila)
	
```

#### Selección de padres
```python:
def seleccion_padres(poblacion, fitness):
    suma_fitness = sum(fitness)
	
    probabilidades = [fit / suma_fitness for fit in fitness]

    prob_acumulada = [sum(probabilidades[:i + 1]) for i in range(len(probabilidades))]

    n_r = random.random()

    seleccionado = None
    for i in range(len(prob_acumulada)):
        if n_r <= prob_acumulada[i]:
            seleccionado = poblacion[i]
            break

    return seleccionado

```

#### Cruza
```python:
def cruza(mapa, padre1, padre2):
    punto_corte1 = random.randint(0, len(padre1) - 1)
    punto_corte2 = random.randint(punto_corte1, len(padre1))

    hijo1 = list(padre1)
    hijo2 = list(padre2)

    for i in range(punto_corte1, punto_corte2):
        hijo1[i] = padre2[i]
        hijo2[i] = padre1[i]

    for i in range(len(padre1)):
        if punto_corte1 <= i < punto_corte2:
            continue
        gen = padre1[i]
        while gen in hijo1[punto_corte1:punto_corte2]:
            index = padre1.index(gen)
            gen = padre2[index]
        hijo1[i] = gen

    for i in range(len(padre2)):
        if punto_corte1 <= i < punto_corte2:
            continue
        gen = padre2[i]
        while gen in hijo2[punto_corte1:punto_corte2]:
            index = padre2.index(gen)
            gen = padre1[index]
        hijo2[i] = gen

    return hijo1, hijo2
```

#### Mutación
```python:
def mutacion_insercion(individuo):
    hijo_mutado = list(individuo)

    pos1, pos2 = random.sample(range(len(hijo_mutado)), 2)

    gen = hijo_mutado.pop(pos1)

    hijo_mutado.insert(pos2, gen)

    return hijo_mutado
```
#### Reemplazo
```python:
def reemplazo_generacional(poblacion_actual, descendencia):
    poblacion_actual = descendencia
    return poblacion_actual
```

#### Código completo



### Gráfica de convergencia

Alguno de las intentos: 

![image](https://github.com/monseher00/Geneticos_y_8_reinas/assets/92997919/ab47bad7-e88d-4bb3-8cf1-fb25253d06e2)


### Conclusiones

El desafío asociado al problema de las 8 reinas es significativo en el contexto de la programación de algoritmos genéticos y la inteligencia artificial. Este problema clásico presenta la tarea de hallar una disposición en la que ocho reinas en un tablero de ajedrez de 8x8 no se amenacen mutuamente. Este ejercicio, en particular, sirve como una plataforma para explorar y comprender conceptos fundamentales dentro de la programación de algoritmos genéticos.

En primer lugar, este ejercicio exige la representación de soluciones mediante cromosomas, un aspecto crucial en la programación de algoritmos genéticos. Los cromosomas se convierten en representaciones de posibles soluciones al problema y evolucionan a lo largo de generaciones sucesivas.

Asimismo, se aplican operadores genéticos como la selección, cruza y mutación para orquestar la evolución de la población hacia soluciones de mayor calidad. En este contexto, la función de evaluación de aptitud desempeña un papel fundamental al medir la idoneidad de las soluciones, específicamente, cuántas reinas están en situación de amenaza mutua.

La capacidad de resolver exitosamente este problema demuestra la eficacia de los algoritmos genéticos para optimizar soluciones y converger hacia la solución deseada. En este caso, la meta es encontrar la disposición de reinas en el tablero sin conflictos interreinas.
