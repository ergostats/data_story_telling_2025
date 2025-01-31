[[Temario]]

# Presentacion 1 

- valores atomicos 
- vectores 
- propiedades de los vectores 
- reglas de cohercios

# Presentacion 2 

**Lo que debe incluirse**

- Como funciona los indices en R para vector y para data.frames
- Operaciones: vectoriales y de vector contra escalar

**Lo que hay**

- Extraer elemensot de un vector 
- factores
- listas
- tablas o data.frames
- Extraccion de elementos de una tabla 
- Extraccion de varias columnas y filas 

# Temario de la clase

---
# Conoce tus `building block`
---
## Vectores atómicos

Un vector no es más que una secuencia de elementos de un tipo de datos determinado.

Inicialmente existen 5 tipos de vectores atómicos en R:

- **Doubles**

Valores numéricos con decimales. Ej. Resultado de división.

```{r}
x <- 3.1416
class(x)  # Output: "numeric" (en R, "double" se agrupa como "numeric")
```

- **Integers**

Números enteros. Requieren la letra `L` al final para diferenciarlos de los "doubles". Ej. Número de personas.

```{r}
y <- 5L
class(y)  # Output: "integer"
```

- **Characters**

Cadenas de texto. Ej. "Hola Mundo"

```{r}
z <- "Hola, R"
class(z)  # Output: "character"
```

- **Logical**

Valores `TRUE` (verdadero) o `FALSE` (falso). Ej. Preguntas de `Si` o `No`. 

```{r}
a <- 3 > 2
print(a)    # Output: TRUE
class(a)    # Output: "logical"
```

- **Factor**
Variables categóricas con niveles predefinidos. Ej. Frío, caliente.

```{r}
colores <- factor(c("rojo", "azul", "azul", "verde"))
print(colores)  # Output: rojo azul azul verde | Levels: azul rojo verde
class(colores)  # Output: "factor"
```

---

## Vectores 

Es la colección de **varios atómicos** de la **misma clase**. 

* R emplea el comando `c(...)` para juntar elementos de un mismo tipo en un vector:

```{r, eval=FALSE}
c(35, 42, 21, 18)
```

* En R se utiliza el símbolo `<-` para asignar un vector (de lado derecho del símbolo) a un nombre (del lado izquierdo del símbolo). A este símbolo se lo conoce como **asignación**:

```{r, eval=FALSE}
edades <- c(35, 42, 21, 18)
```

* Los vectores que se creen tras la asignación van a guardarse en el `Global Environment`.

---

## Los vectores tienen propiedades: 

Los atributos son **metadatos** (información adicional) que se pueden adjuntar a un objeto en R.

- **No modifican los datos originales**, pero definen cómo se comporta el objeto o cómo se interpreta.
- Son clave para estructuras como matrices, factores, fechas, o data frames.

##### Atributos comunes en vectores 

1. `names`: Etiquetas para los elementos de un vector.

```{r}
vector <- c(a = 1, b = 2, c = 3)
names(vector)  # Output: "a" "b" "c"
```

2. `dim`: Transforma un vector en una **matriz** o **array** (asignando dimensiones).

```{r}
vector <- 1:6
dim(vector) <- c(2, 3)  # Crea una matriz 2x3
```

3. `class`: Se refiere al tipo de valor atómico que tiene.

```{r}
v <- c(1, 2, 3)
class(v)  # "numeric"
```


4. `length`: Muestra el número de elementos que tiene un vector.

```{r}
mi_vector <- c(10, 20, 30, 40, 50)
length(mi_vector)   # Respuesta: 5 
```

> Para conocer todos los atributos de un vector o elemento de R podemos usar la función `attributes(...)` sin embargo, **los atómicos no tienen atributos**. Por ello la función devuelve el valor `NULL`

---

## Diagnóstico de datos 

En R tenemos tres funciones clave para entender tus datos, pero cada una responde una pregunta distinta:

1. `class()` -> **¿Qué soy?**

Indica la **clase** del objeto, que define su comportamiento en R (por ejemplo, cómo se imprimen, grafican o manipulan los datos).

```{r}
x <- 5L
class(x)  # "integer"

y <- factor(c("a", "b"))
class(y)  # "factor"
```

2. `typeof()` -> **¿Cómo me almacena R?**

Muestra **cómo guarda R el dato en memoria**.

- Un caso especial es `factor` que en realidad son **números enteros con etiquetas** (ej: 1=rojo, 2=azul).

```{r}
x <- 5L
typeof(x)  # "integer" 

y <- factor(c("a", "b"))
typeof(y)  # "integer"
```

3. `str()` -> **¿Qué hay dentro de mi?**

Combina información de `class()`, `typeof()`, y atributos.

```{r}
z <- factor(c("a", "b"))
str(z)
# Output: Factor w/ 2 levels "a","b": 1 2
```


---

## Matrices

Son estructuras de datos **bidimensionales** (filas y columnas) que almacenan valores del **mismo tipo** (todos numéricos, todos caracteres, etc.)

**Características**

- **Homogeneidad**: Todos los elementos deben ser del mismo tipo (ej: solo números o solo texto).
- **Dimensiones**: Se definen por filas (`nrow`) y columnas (`ncol`). 
- **Creación**: Se construyen con la función `matrix()`.

**Ejemplo**

```{r}
# Crear una matriz de 3 filas y 2 columnas
matriz <- matrix(data = 1:6, nrow = 3, ncol = 2)
print(matriz)
```

**Funciones útiles**

- `dim()`: Obtener dimensiones (filas, columnas).

```
dim(matriz)  # Output: 3 2
```

- `rownames()` y `colnames()`: Asignar nombres a filas/columnas.

```{r}
rownames(matriz) <- c("Fila1", "Fila2", "Fila3")
colnames(matriz) <- c("ColA", "ColB")
```

---
## Reglas de coerción

A pesar de que los vectores almacenan elementos del mismo tipo, se puede unir o concatenar vectores de diferente tipo. Cuando esto ocurre, los datos se van a convertir de un tipo de datos en otro de acuerdo a las siguientes reglas de coerción:

<img src = "https://i.imgur.com/QrmSoIc.png" width = "600">

---
## Listas y tablas

El material visto hasta el momento ha sido de **vectores**. Sin embargo, estos solo pueden almacenar elementos de un mismo tipo y que cuando intentamos unir elementos de distinto tipo en un solo vector se aplicarán las **reglas de coerción**. Para dar solución a esta problemática están las listas y tablas

### Listas

Una lista en R se define como una colección indexada de cualquier tipo de elemento que se puede generar en R (entre ellos los vectores). 

Para crear una lista usamos el comando `list()` y dentro de esta podemos declarar cualquier objeto de R.

---

## Ejemplo de uso de las listas
Metadatos son todas las otras especificaciones que no están dentro de la base de datos pero son necesarias para entender cómo fue construida esta. **Ejemplo:** nombres de las variables, fecha de descarga, tamaño de la base de datos, formato de la base.

```{r, eval=F}
metadatos <- list("35 mb",           #caracter: tamaño de la base
                  1:200,             #enteros: índice de filas
                  c(TRUE, FALSE),    #lógico
                  (1:4)/2,           #numérico decimal
                  as.Date("03/07/2020", 
                          format="%d/%m/%y"))    #fecha

metadatos

str(metadatos)         #Muestra los elementos que tiene

```

> En este caso, hemos almacenado elementos de distintos tipos y tamaños en una lista metadatos pero no hemos definido un nombre específico para cada elemento.
---

## Listas 

En la lista que hemos llamado `metadatos` vemos una colección de 5 vectores. El primero es de tipo `caracter`, el segundos de tipo `entero`, el tercero de tipo `lógico`, el cuarto de tipo `numérico` y el quinto es de tipo `Date`.

## Listas nombradas:
En una lista podemos asignar nombres a los elementos que la componen. Esto permite un acceso más fácil a los elementos de una lista.

```{r, eval=F}
metadatos2 <- list(`tamaño de la base`="35 mb",          
                  `fecha descarga`=as.Date("03/07/2020", 
                          format="%d/%m/%y")) 

names(metadatos2)             #Muestra los nombres de (...)

attributes(metadatos2)          
```

---
## Extraer los elementos de un vector que esté dentro de la lista

Se aplica una lógica similar a la extracción de elementos de un vector al utilizar una posición o un vector de posiciones.

.pull-left[
* Si se conoce la **posición** del vector deseado se usa el **doble corchete** `[[]]`. 
```{r, eval=F}
metadatos2[[1]]
```
]

.pull-right[
* Si se conoce el nombre del vector deseado se usa el **símbolo de dólar** `$`
```{r, eval=F}
metadatos2$`tamaño de la base`
```
]

### Extraer un elemento como lista

Utilizo un sólo corchete `[]`
```{r, eval=F}
metadatos2[1]
```

---

## Tablas o data.frames

En R las tablas se conocen como `data.frames`.
De manera específica, son **listas** con nombres que albergan vectores de un **mismo tamaño** y que entre todos componen una **tabla de datos** con un tema específico. 

En los **data.frames** las **filas u observaciones** corresponden a una unidad (personas, unidades de salud, carros, plantas) y las **columnas o variables** constituyen características de estas unidades. 

```{r, eval=T}
head(iris, 2)
```

.pull-left[
```{r, eval=T}
class(iris)
```
]

.pull-right[
> `iris` es un `data.frame` que viene por default en el environment de R. Úsalo para hacer tus pruebas de código.
]

---
## Ejemplo de un data.frame

Los objetos de clase `data.frame` tienen atributos importantes. Por ejemplo un `data.frame` tiene **dimensiones** (filas y columnas que se pueden contabilizar con la función `dim()`).

```{r, eval=T}
df <- data.frame(x = 1:3, 
                 y = c("a", "b", "c"))
```

.pull-left[
```{r}
attributes(df)
```
]

.pull-right[
```{r}
dim(df)
```
]

---

## Extracción de elementos de una tabla

Dado que el `data.frame` o  `tabla` tiene dimenciones, para la extracción de elementos se necesita de dos indicadores: fila y columna, de la forma:

```{r, eval=F}
tabla[indicador_fila, indicador_columna]

# Ejemplo 1: extracción del elemento que está en la primero fila y segunda columna de la tabla:
df[1,2]

# Ejemplo 2: extracción de la tercera fila de la tabla:
df[3,]

# Ejemplo 3: extracción de la segunda columna de la tabla:
df[,2]
```
>* **Ej 2:** El contador de columnas está **vacio**, esto indica que queremos la tercera fila y todas las columnas de la tabla.
* **Ej:3:** El contador de filas está **vacio**, esto indica que queremos la segunda columna y todas las filas de la tabla.

---

## Formas de extracción

* Mediante la posición
```{r,eval=F}
df[,2]
```
* Usando el nombre del vector
```{r,eval=F}
df[,"y"]
```
* Usando el símbolo:`$`
```{r,eval=F}
df$y
```
* Usando dos corchetes `[[]]`
  * Mediante la posición
  * Usando el nombre del vector

```{r,eval=F}
df[[2]]        #este sirve sólo para columnas
df[["y"]]
```

---
## Extracción de varias columnas y filas

Cuando extraemos más de un elemento se debe usar la función concatenar `c(...,...,...)`. 
La estructura es la siguiente:

```{r,eval=F}
tabla[c(conjunto de filas), c(conjunto de columnas)]
```

En este caso, también existen varias formas de extracción:

.pull-left[
* Mediante la posición
```{r,eval=F}
df[c(1,3),c(1,2)]
```
* Usando el nombre del vector
```{r,eval=F}
df[,c("x","y")]
```
]

.pull-right[
* Usando una mezcla de los dos anteriores
```{r,eval=F}
df[c(1,3),c("x","y")]
```
]

---

## Operaciones entre vectores 

### Ejecución elemento por elemento 

En **R** las operaciones matemáticas y lógicas en vectores y matrices se aplican **individualmente** a cada elemento, sin necesidad de usar bucles explícitos.

```{r}
# Vector numérico
x <- c(1, 2, 3, 4)

# Multiplicación elemento por elemento
resultado <- x * 2
print(resultado)

# Cada elemento de `x` se multiplicó por 2
```

<img src = "https://rstudio-education.github.io/hopr/images/hopr_0103.png">
### Regla de Reciclaje

Si los vectores **tienen tamaños diferentes**, R **recicla** el elemento del objeto más pequeño hasta igualar longitudes:

```{r}
vector <- c(1, 2, 3, 4)
escalar <- 10
vector * escalar  # 10, 20, 30, 40 (10 se recicla 4 veces)
```


<img src = "https://rstudio-education.github.io/hopr/images/hopr_0104.png">
