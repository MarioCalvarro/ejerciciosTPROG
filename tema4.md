# Semántica operacional básica. Ejercicios
## Ejercicio 4.4
### Enunciado
Demostrar que si $\langle c_1, e_1, s \rangle \triangleright^k \langle c', e',
s' \rangle$, entonces se cumple que $\langle c_1 : c_2, e_1 : e_2, s \rangle
\triangleright^k \langle c':c_2, e':e_2, s' \rangle$.

### Resolución
Razonaremos por inducción sobre el tamaño de $k$. Para los casos base observamos
que ninguna de las instrucciones (por definición) modifica $c_2$ ni $e_2$, por
lo que se cumple.

Sea ahora una computación de longitud $k+1$ $\left( \langle c_1, e_1, s \rangle
\triangleright^{k+1} \langle c', e', s' \rangle \right)$ y supongamos, por hipótesis de
inducción, que la propiedad se cumple para las secuencias de longitud $k$. Si
ejecutamos la primera instrucción de $c_1$ tendremos $\langle c_1 : c_2, e_1 :
e_2, s \rangle \triangleright \langle c_1' : c_2, e_1' : e_2, s_1' \rangle$ puesto
que aplicamos el caso base. Como ahora tenemos que $\langle c_1', e_1', s
\rangle \triangleright^k \langle c', e', s' \rangle$, podemos aplicar la
hipótesis de inducción y obtener el resultado que buscábamos.

## Ejercicio 4.5 (TODO)
### Enunciado
Demostrar que si $\langle c_1 : c_2, e, s \rangle \triangleright^k \langle
\varepsilon, e'', s'' \rangle$, entonces existe una configuración $\langle
\varepsilon, e', s' \rangle$ y $k_1, k_2 \in \mathbb{N}. k_1 + k_2 =
k$ tal que:
$$
\begin{align*}
\langle c_1, e, s \rangle \triangleright^{k_1} \langle \varepsilon, e', s'
\rangle \land \langle c_2, e', s' \rangle \triangleright^{k_2} \langle \varepsilon,
e'', s'' \rangle
\end{align*}
$$

### Resolución
Razonaremos por inducción sobre la longitud de una computación $k$. Sea $k = 0$,
entonces tenemos directamente el resultado. Supongamos entonces que se cumple
el resultado para $k \le k_0$ y lo demostraremos para $k_0 + 1$. Con esto
asumimos que se cumple:
$$
\langle c_1 : c_2, e, s \rangle \triangleright^{k_0+1}\langle \varepsilon, e'',
s'' \rangle
$$
o lo que es lo mismo:
$$
\langle c_1 : c_2, e, s \rangle \triangleright \langle c_1' : c_2, e_1, s_1 \rangle \triangleright^{k_0} \langle \varepsilon, e'',
s'' \rangle
$$
donde $c_1'$ puede ser $\varepsilon$. Esto es así si consideramos que
$$
\langle c_1, e, s \rangle \triangleright \langle c_1', e_1, s_1 \rangle,
$$
aplicamos el anterior ejercicio y tenemos en cuenta el determinismo de este
lenguaje.

Si es el caso de que $c_1' = \varepsilon$, entonces ya tenemos el resultado
porque $k_1 = 1$ y $k_2 = k_0$ y podemos aplicar la hipótesis de inducción al
ser $k_2 \le k_0$.

Supongamos entonces que $c_1' \neq \varepsilon$. Podemos aplicar la hipótesis de
inducción sobre la segunda derivación, de longitud $k_0$, y obtenemos $k_1'$ y
$k_2$ tales que $k_1' + k_2 = k_0$ cumpliendo que:
$$
\langle c_1', e_1, s_1 \rangle \triangleright^{k_1'} \langle \varepsilon, e', s' \rangle \land \langle c_2, e', s' \rangle \triangleright \langle \varepsilon, e'', s'' \rangle
$$
Pero entonces con simplemente sumar un paso más a $k_1'$, obtenemos $k_1$ que
cumplirá el resultado que buscábamos:
$$
\langle c_1, e, s \rangle \triangleright \langle c_1', e_1, s_1 \rangle \triangleright^{k_1'} \langle \varepsilon, e', s' \rangle.
$$

## Ejercicio 4.6
### Enunciado
Demostrar que la semántica dada de la MA es determinista.
$$
\gamma \triangleright \gamma' \land \gamma \triangleright \gamma'' \Rightarrow
\gamma' = \gamma''
$$

### Resolución
No es posible que $\gamma'$ sea distinto de $\gamma''$ puesto que solo hay una
derivación para cada instrucción.

## Ejercicio 4.7
### Enunciado
Modificar la MA para referirse a las variables por su dirección:
- Las configuraciones serán $\langle c, e, m \rangle$ con $m \in \mathbb{Z}^*$
    tal que $m\left[ n \right]$ toma el $n$-ésimo valor de la lista $m$.

- Sustituir $\mathtt{FETCH-}x$ y $\mathtt{STORE-}x$ por $\mathtt{GET-}n$ y
    $\mathtt{PUT}-n$ donde $n \in \mathbb{N}$ representa una dirección.

Dar la semántica operacional de esta nueva MA.

### Resolución
No daremos todas las instrucciones por ser muchas equivalentes, de hecho, todas
las que no utilicen o modifiquen el estado en la anterior MA serán iguales en
la nueva (cambiando las $s$ por $m$ simplemente). Por tanto, solo tenemos que modificar las dos que nos indica el
enunciado:
$$
\begin{align*}
\langle \mathtt{GET-}x : c, e, m \rangle &\triangleright \langle
c, m\left[ x \right] : e, m \rangle\\
\langle \mathtt{STORE-}x : c, z : e, m \rangle &\triangleright \langle
c, e, m' \rangle, \text{ donde } m'\left[ n \right] = \begin{cases}
    m[n], &\text{si } n \neq x\\
    z, &\text{si } n = x 
\end{cases}\\
\end{align*}
$$

## Ejercicio 4.8
### Enunciado
Modificar la MA del anterior ejercicio para que tenga instrucciones de salto no
estructuradas:
- Las configuraciones serán $\langle pc, e, m \rangle$ donde $pc \in \mathbb{N}$
    indica el contador de programa que apunta a la siguiente instrucción a
    ejecutar en $c$ ($c\left[ pc \right]$).
- Sustituir $\mathtt{BRANCH}$ y $\mathtt{LOOP}$ por $\mathtt{LABEL-}l$,
    $\mathtt{JUMP-}l$ y $\mathtt{JUMPFALSE}-l$, representado $\mathtt{LABEL-}l$
    con $l \in \mathbb{N}$ una posición en $c$.

Dar la semántica operacional de esta nueva MA.

### Resolución
La mayoría de instrucciones (a parte de las mencionadas en el enunciado) no
tendrán un cambio sustancial respecto a la anterior MA. Por ello, solo mostraré,
a modo de ejemplo, un par de ellas:
- Si $c\left[ pc \right] = \mathtt{PUSH-}n$:
$$
\begin{align*}
\langle pc, c, e, m \rangle &\triangleright \langle pc + 1, c,
\mathcal{N}\llbracket n \rrbracket : e, m \rangle\\
\end{align*}
$$
- Si $c\left[ pc \right] = \mathtt{GET-}n$:
$$
\langle pc, c, e, m \rangle \triangleright \langle pc + 1,
c, m\left[ x \right] : e, m \rangle
$$
Y el resto de instrucciones serán igual.

Veamos ahora las instrucciones que nos pide cambiar el enunciado:
- Si $c\left[ pc \right] = \mathtt{LABEL-}l$:
    $$
    \langle pc, c, e, m \rangle \triangleright \langle pc + 1, c, e, m \rangle
    $$
    Simplemente queremos esta instrucción para que luego cuando la busquemos con
    el $\mathtt{JUMP}$ podamos saltar a ella.
- Si $c\left[ pc \right] = \mathtt{JUMP-}l$:
    $$
    \langle pc, c, e, m \rangle \triangleright \langle pc', c, e, m \rangle
    $$
    donde $pc' = \min \left\{ pc : c\left[ pc \right] = \mathtt{LABEL-}l \right\}$, es decir, saltará a la primera instrucción que sea $\mathtt{LABEL-}l$.
- Si $c\left[ pc \right] = \mathtt{JUMPFALSE-}l$ tenemos dos casos:
    $$
    \begin{align*}
    \langle pc, c, \mathbf{ff} : e, m \rangle &\triangleright \langle pc', c, e, m \rangle\\
    \langle pc, c, \mathbf{tt} : e, m \rangle &\triangleright \langle pc + 1, c, e, m \rangle\\
    \end{align*}
    $$
    donde $pc' = \min \left\{ pc : c\left[ pc \right] = \mathtt{LABEL-}l \right\}$. Es decir, que si en la cima de la pila de ejecución hay un símbolo de *falso*, se realiza el salto y, si no es así, se pasa a la siguiente instrucción.

## Ejercicio 4.11
### Enunciado
Demostrar que $\mathcal{CA}\llbracket \left( a_1 + a_2 \right) + a_3 \rrbracket
\neq \mathcal{CA}\llbracket a_1 + \left( a_2 + a_3 \right) \rrbracket$, pero que
se comportan de forma *suficientemente similar*.

### Resolución
Por definición:
$$
\begin{align*}
\mathcal{SA}\llbracket \left( a_1 + a_2 \right) + a_3 \rrbracket &=
\mathcal{SA}\llbracket a_3 \rrbracket : \mathcal{SA}\llbracket a_1 + a_2
\rrbracket : \mathtt{ADD}\\
&= \mathcal{SA}\llbracket a_3 \rrbracket : \mathcal{SA}\llbracket a_2 \rrbracket
: \mathcal{SA}\llbracket a_1 \rrbracket : \mathtt{ADD} : \mathtt{ADD}
\end{align*}
$$
Mientras que:
$$
\begin{align*}
\mathcal{SA}\llbracket a_1 + \left( a_2 + a_3 \right) \rrbracket &=
\mathcal{SA}\llbracket a_2 + a_3 \rrbracket : \mathcal{SA}\llbracket a_1
\rrbracket : \mathtt{ADD}\\
&= \mathcal{SA}\llbracket a_3 \rrbracket : \mathcal{SA}\llbracket a_2 \rrbracket
: \mathtt{ADD} : \mathcal{SA}\llbracket a_1 \rrbracket : \mathtt{ADD}
\end{align*}
$$
con lo que estrictamente no son iguales. Sin embargo, son *suficientemente
similares* puesto que la suma es asociativa y el orden en que se hagan las
operaciones no importa.

## Ejercicio 4.14
### Enunciado
Extender **WHILE** con la instrucción `repeat S until b` y dar la compilación de
esta a AM.

### Resolución
La compilación será:
$$
\mathcal{CS}\llbracket \mathtt{repeat}\ S\ \mathtt{until}\ b \rrbracket = \mathcal{CS}\llbracket S \rrbracket : \mathtt{LOOP}\left( \mathcal{CB}\llbracket ¬b \rrbracket, \mathcal{CS}\llbracket S \rrbracket \right).
$$

## Ejercicio 4.16
### Enunciado
Dar la función de compilación para las instrucciones de **WHILE** a la MA del
ejercicio 4.7. Utilizar la función $env: \mathbf{Var} \rightarrow \mathbb{N}$
que, dada una variable, devuelve su dirección de memoria.

### Resolución
Tan solo tendremos que cambiar las asignaciones y el *fetch* de las variables:
$$
\begin{align*}
    \mathcal{CA}\llbracket x \rrbracket &= \mathtt{GET-}\left( env\ x \right)\\
    \mathcal{CA}\llbracket x := a \rrbracket &= \mathcal{CA}\llbracket a \rrbracket : \mathtt{PUT-}\left( env\ x \right)
\end{align*}
$$
El resto de traducciones se mantendrán igual.

## Ejercicio 4.17
### Enunciado
Dar la función de compilación para las instrucciones de **WHILE** a la MA del
ejercicio 4.8. Garantizar la unicidad de las etiquetas incorporando el parámetro
adicional *siguiente etiqueta sin usar* ($\mathrm{sig}$).

### Resolución
De nuevo, las únicas instrucciones que cambiarán son los `if` y los `while`.
Diremos que:
- Condicional:
    $$
    \begin{align*}
    \mathcal{CA}\llbracket \mathtt{if}\ b\ \mathtt{then}\ S_1\ \mathtt{else}\
    S_2, l \rrbracket =\ &\mathcal{CB}\llbracket b \rrbracket :
    \mathtt{JUMPFALSE-}l : \mathcal{CS}\llbracket S_1, l + 2 \rrbracket :
    \mathtt{JUMP-}\left( l+1 \right)\\
    &: \mathtt{LABEL-}l : \mathcal{CS}\llbracket S_2, l+2 \rrbracket :
    \mathtt{LABEL-}\left( l+1 \right).
    \end{align*}
    $$
    Es decir, evaluamos $b$ si es falso saltamos al *label* que hemos puesto
    justo después del bloque $S_1$. Si se ejecuta $S_1$, tenemos que saltar al
    final del condicional para que no se ejecute $S_2$ también.

- Bucle:
    $$
    \begin{align*}
    \mathcal{CA}\llbracket \mathtt{while}\ b\ \mathtt{do}\ S \rrbracket =\ 
    &\mathtt{LABEL-}l :\mathcal{CB}\llbracket b \rrbracket : \mathtt{NEG} :
    \mathtt{JUMPFALSE}-\left( l+1 \right)\\
    &: \mathcal{CS}\llbracket S, l+2 \rrbracket : \mathtt{JUMP-} l : \mathtt{LABEL-}\left( l+1 \right).
    \end{align*}
    $$
    Es decir, creamos un *label* al que saltaremos una vez iteremos sobre el
    bucle y evaluamos la condición del `while`. Si es cierta, saltamos al final
    de bucle y si no, ejecutamos $S$.

## Ejercicio 4.23
### Enunciado
Supongamos que la traducción de $\mathtt{skip}$ es no generar código. ¿Complica
esto la demostración de la equivalencia entre la semántica operacional de paso
largo y la semántica derivada de la compilación a AM?

### Resolución
Lo veremos en dos partes. En primer lugar, la prueba de que si $\langle S, s
\rangle \rightarrow s'$, entonces $\langle \mathcal{CS}\llbracket S \rrbracket,
\varepsilon, s \rangle \triangleright^* \langle \varepsilon, \varepsilon, s'
\rangle$, no cambia en absoluto. Sin embargo, la demostración de la inversa
tiene que modificarse ligeramente. Ahora el caso base de $k = 0$ no es trivial.
Si denotamos por $S_n := \mathbf{skip\ ;\ skip\ ;}\ldots \mathtt{skip}$
tenemos que $\langle \mathcal{CS}\llbracket S_n \rrbracket = \varepsilon,
\varepsilon, s \rangle \triangleright^0 \langle \varepsilon, \varepsilon, s'
\rangle$. En este caso, es obvio que $s = s'$. Veamos ahora por inducción sobre
$n$ que se cumple lo que buscamos:
- $n = 1$. En este caso $S_1 = \mathtt{skip}$ por lo que solo podemos aplicar
    $\left[ \mathrm{skip}_{\mathrm{ns}} \right]$ y obtenemos:
    $$
    \langle S_1, s \rangle \rightarrow s.
    $$

- Caso inductivo. Supongamos cierta la hipótesis para $n$. Como $S_{n+1} = S_n
    \mathtt{;} \mathtt{skip}$ y $\langle \mathtt{skip}, s \rangle \rightarrow s$
    y, por HI, $\langle S_n, s \rangle \rightarrow s$, podemos aplicar $\left[ \mathrm{comp}_{\mathrm{ns}} \right]$ y obtenemos que
    $$
    \langle S_{n+1}, s \rangle \rightarrow s.
    $$
