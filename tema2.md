# Semántica operacional básica. Ejercicios
## Ejercicio 2.8
### Enunciado
Extender el lenguaje **WHILE** con la instrucción `for`:

    for x := a1 to a2 do S

Definir la relación $\rightarrow$ para esta construcción.

### Resolución
Veamos en primer lugar la definición cuando se entra en el bucle:
$$
\left[ \mathrm{for}_{\mathrm{ns}}^{\mathrm{tt}} \right] := \frac{\langle S,
s\left[ x \mapsto \mathcal{A}\llbracket a_1 \rrbracket s\right] \rangle
\rightarrow s', \langle \mathtt{for}\ x := x + 1\ \mathtt{to}\ a_2\ \mathtt{do}\
S, s' \rangle \rightarrow s''}{\langle \mathtt{for}\ x := a_1\ \mathtt{to}\ a_2\
\mathtt{do}\ S, s \rangle \rightarrow s''},\\ \text{ si } \mathcal{B}\llbracket
a_1 \le a_2 \rrbracket s = \mathbf{tt}.
$$
Y ahora el caso en que no se cumpla la condición:
$$
\left[ \mathrm{for}_{\mathrm{ns}}^{\mathrm{ff}} \right] := \langle \mathtt{for}\
x := a_1\ \mathtt{to}\ a_2\ \mathtt{do}\ S, s \rangle \rightarrow s, \text{ si }
\mathcal{B}\llbracket a_1 \le a_2 \rrbracket s = \mathbf{ff}
$$
El caso de interés es cuando se cumple la condición. Los aspectos más
destacables son los siguientes:
- En las premisas tenemos, en primer lugar, que el estado $s'$ se obtiene de
    ejecutar $S$ sobre el estado original $s$ después de aplicar la asignación a
    $x$ con $a_1$.

- La definición no puede ser composicional puesto que tenemos que capturar la
    «iteratividad» de la instrucción. Por eso, en las premisas tenemos un `for`,
    pero con la peculiaridad de que le asignamos una unidad más de su valor en
    el estado $s'$. Esto es una decisión de diseño que podría hacerse de
    distinta manera, pero así aseguramos que el bucle acabe en algún
    momento.

## Ejercicio 2.10
### Enunciado
Demostrar que la construcción

    repeat S until b

Es semánticamente equivalente a `S; while ¬b do S`.

### Resolución
En primer lugar, la definición para la semántica de paso largo del `repeat` es:
$$
\left[ \mathrm{repeat}_{\mathrm{ns}}^{\mathrm{tt}} \right] := \frac{\langle S, s
\rangle \rightarrow s'}{\langle
\mathtt{repeat}\ S\ \mathtt{until}\ b, s \rangle \rightarrow s'}, \text{ si }
\mathcal{B}\llbracket b \rrbracket s' = \mathbf{tt}\\

\left[ \mathrm{repeat}_{\mathrm{ns}}^{\mathrm{ff}} \right] := \frac{\langle S, s
\rangle \rightarrow s', \langle \mathtt{repeat}\ S\ \mathtt{until}\ b, s' \rangle
\rightarrow s''}{\langle \mathtt{repeat}\ S\ \mathtt{until}\ b, s \rangle \rightarrow
s''}, \text{ si } \mathcal{B}\llbracket b \rrbracket s' = \mathbf{ff}\\
$$
Ahora veremos la equivalencia usando inducción sobre el árbol de derivación. Sea
$s \in \mathbf{State}$.
- En primer lugar, supongamos que $\langle \mathtt{repeat}\ S\ \mathtt{until}\
    b, s \rangle \rightarrow s'$. Queremos ver ahora que $\langle S\ \mathtt{;}\
    \mathtt{while}\ ¬b\ \mathtt{do}\ S, s \rangle \rightarrow s'$, o lo que es lo
    mismo (por definición de composición), $\langle S, s \rangle \rightarrow
    s_0$ y $\langle \mathtt{while}\ ¬b\ \mathtt{do}\ S, s_0 \rangle \rightarrow
    s'$. La primera premisa la tenemos directamente por definición de `repeat`.
    Ahora distinguimos casos:
    - Si $\mathcal{B}\llbracket b \rrbracket s_0 = \mathbf{tt}$ quiere decir que
        $s_0 \equiv s'$ y que, por definición del `while` y
        $\mathcal{B}\llbracket ¬b \rrbracket s_0 = \mathbf{ff}$, $\langle
        \mathtt{while}\ ¬b\ \mathtt{do}\ S, s_0 \rangle \rightarrow s_0 \equiv
        s'$. Con esto, para este caso se cumple la implicación.
    - Si ahora $\mathcal{B}\llbracket b \rrbracket s_0 = \mathbf{ff}$, tenemos
        que, para demostrar $\langle \mathtt{while}\ ¬b\ \mathtt{do}\ S, s_0 \rangle \rightarrow
        s'$, necesitamos ver que $\langle S, s_0 \rangle \rightarrow s_1$ y que
        $\langle \mathtt{while}\ ~b\ \mathtt{do}\ S, s_1 \rangle \rightarrow
        s'$ o lo que es lo mismo, $\langle S\ \mathtt{;}\ \mathtt{while}\ ¬b\
        \mathtt{do}\ S, s_0 \rangle \rightarrow s'$. Sin embargo, por la definición
        de `repeat` tenemos $\langle \mathtt{repeat}\ S\ \mathtt{until}\ b, s_0
        \rangle \rightarrow s'$ con lo que aplicando la hipótesis de inducción
        tenemos el resultado.
- Se ahora suponemos la otra hipótesis, $\langle S\ \mathtt{;}\
    \mathtt{while}\ ¬b\ \mathtt{do}\ S, s \rangle \rightarrow s'$ simplemente
    tendremos que invertir los pasos dados en el anterior apartado para sacar el
    resultado final.

## Ejercicio 2.11
### Enunciado
Definir la semántica de la expresiones aritméticas $\mathcal{A}$ de forma
similar a la semántica operacional de paso largo.

Demostrar que es equivalente a la anteriormente definida.

### Resolución
Definición:
- Constantes:
    $$
    \langle n, s \rangle \rightarrow_{\mathrm{Aexp}} \mathcal{N}\llbracket n \rrbracket
    $$
- Variables:
    $$
    \langle x, s \rangle \rightarrow_{\mathrm{Aexp}} s\ x
    $$
- Sumas:
    $$
    \frac{\langle a_1, s \rangle \rightarrow_{\mathrm{Aexp}} z_1, \langle a_2, s \rangle \rightarrow_{\mathrm{Aexp}} z_2}{\langle a_1 + a_2, s \rangle \rightarrow_{\mathrm{Aexp}} z},\ \text{ con } z_1 + z_2 = z
    $$
- Restas:
    $$
    \frac{\langle a_1, s \rangle \rightarrow_{\mathrm{Aexp}} z_1, \langle a_2, s \rangle \rightarrow_{\mathrm{Aexp}} z_2}{\langle a_1 - a_2, s \rangle \rightarrow_{\mathrm{Aexp}} z},\ \text{ con } z_1 - z_2 = z
    $$
- Productos:
    $$
    \frac{\langle a_1, s \rangle \rightarrow_{\mathrm{Aexp}} z_1, \langle a_2, s \rangle \rightarrow_{\mathrm{Aexp}} z_2}{\langle a_1 * a_2, s \rangle \rightarrow_{\mathrm{Aexp}} z},\ \text{ con } z_1 * z_2 = z
    $$

Para la demostración usaremos inducción estructural sobre $\mathbf{Aexp}$. El
caso compuesto (las operaciones) lo haremos genérico ya que son iguales.
Empezamos con los casos base. Por simple inspección y definición de
$\mathcal{A}$, son equivalentes. Veamos el caso inductivo. Utilizaremos $\times$
como cualquiera de los operadores. Por hipótesis de inducción, suponemos que
$z_1 \equiv \mathcal{A}\llbracket a_1 \rrbracket s$ y $z_2 \equiv
\mathcal{A}\llbracket a_2 \rrbracket s$. Con esto, $z = \mathcal{A}\llbracket
a_1 \rrbracket s \times \mathcal{A}\llbracket a_2 \rrbracket s$, pero esto
último es, por definición de $\mathcal{A}$, igual a $\mathcal{A}\llbracket a_1
\times a_2 \rrbracket s$ con lo que tenemos el resultado.

## Ejercicio 2.17
### Enunciado
Dar una semántica de paso corto para la instrucción `repeat`.
### Resolución
Tenemos la siguiente definición:
$$
\left[ \mathrm{repeat}_{\mathrm{sos}} \right] := \langle \mathtt{repeat}\ S\
\mathtt{until}\ b, s \rangle \Rightarrow \langle S\ \mathtt{;}\ \mathtt{if}\ b\
\mathtt{then\ skip\ else}\ \left( \mathtt{repeat}\ S\ \mathtt{until}\ b \right), s \rangle
$$

## Ejercicio 2.18
### Enunciado
Dar una semántica de paso corto para la construcción `for`.
### Resolución
Tenemos la siguiente definición:
$$
\left[ \mathrm{for}_{\mathrm{sos}} \right] := \langle \mathtt{for}\ x := a_1\
\mathtt{to}\ a_2\ \mathtt{do}\ S, s \rangle \Rightarrow\\ \langle \mathtt{if}\ a_1
\le a_2\ \mathtt{then}\ \left( S\ \mathtt{;}\ \mathtt{for}\ x := x + 1\
\mathtt{to}\ a_2\ \mathtt{do}\ S \right)\ \mathtt{else}\ \mathtt{skip}, s
\rangle
$$
Es decir, si el valor de $x$ tras la asignación del inicio del `for` es inferior
al valor de la expresión límite, ejecutamos una iteración. Para la siguiente
iteración comenzamos sumando uno a $x$.

## Ejercicio 2.23
### Enunciado
Mostrar la equivalencia semántica (paso corto) entre las siguientes parejas de
sentencias:
1. $S\ \mathtt{;}\ \mathtt{skip}$ y $S$.
2. $\mathtt{while}\ b\ \mathtt{do}\ S$ y $\mathtt{if}\ b\ \mathtt{then}\ \left( S\ \mathtt{;}\ \mathtt{while}\ b\ \mathtt{do}\ S \right)\ \mathtt{else}\ \mathtt{skip}$.
3. $S_1\ \mathtt{;}\ \left( S_2\ \mathtt{;}\ S_3 \right)$ y $\left( S_1\ \mathtt{;}\ S_2 \right)\ \mathtt{;}\ S_3$

### Resolución
Tenemos que demostrar para cada caso que:
$$
\langle S_1, s \rangle \Rightarrow^* \gamma \Leftrightarrow \langle S_2, s \rangle \Rightarrow^* \gamma,
$$
o que si uno tiene una secuencia infinita de derivación, el otro también.
1. Sea $s \in \mathbf{State}$. Razonemos por inducción sobre la longitud de la
   secuencia. 
   - **Caso base**: El caso $k = 0$ no se puede dar directamente,
       pero el caso $k = 1$ solo podrá salir de $\left[
       \mathrm{comp}_{\mathrm{sos}}^2 \right]$, es decir, $\langle S\
       \mathtt{;}\ \mathtt{skip}, s \rangle \Rightarrow \langle \mathtt{skip},
       s' \rangle \Rightarrow s'$. Como se da esto, también se darán las
       premisas de este derivación, es decir, $\langle S, s \rangle \Rightarrow
       s'$ con lo que tenemos el resultado buscado.
   - **Caso inductivo**: Sea $k > 1$, es decir, $\langle S\ \mathtt{;}\
       \mathtt{skip}, s \rangle \Rightarrow^k \gamma$. Como $k > 1$, solo
       podemos aplicar $\left[ \mathrm{comp}_{\mathrm{sos}}^1 \right]$, es
       decir, $\langle S\ \mathtt{;}\ \mathtt{skip}, s \rangle \Rightarrow
       \langle S'\ \mathtt{;}\ \mathtt{skip}, s' \rangle$. Con esto ahora
       tendremos que $\langle S'\ \mathtt{;}\ \mathtt{skip}, s' \rangle
       \Rightarrow^{k-1} \gamma$, con lo que podemos aplicar la hipótesis de
       inducción y ver que $\langle S', s' \rangle \Rightarrow^{k-1} \gamma$.
       Sin embargo, al aplicar la anterior derivación, también serán ciertas las
       hipótesis, es decir, $\boxed{\langle S, s \rangle \Rightarrow \langle S',
       s' \rangle} \Rightarrow^{k-1} \gamma$ con lo que tendremos el resultado.

2. Trivialmente lo vemos por la definición del `while`.

3. Sea $s \in \mathbf{State}$. Razonaremos por inducción sobre la longitud de la
   cadena.
   - **Caso base**. Si $k = 1$, solo podemos aplicar la derivación $\left[
       \mathrm{comp}_{\mathrm{sos}}^2 \right]$ con lo que tenemos que $\langle
       S_1\ \mathtt{;}\ \left( S_2\ \mathtt{;}\ S_3 \right), s \rangle
       \Rightarrow \langle S_2\ \mathtt{;}\ S_3, s' \rangle$ y que $\langle S, s
       \rangle \Rightarrow s'$. Pero, a su vez, si se cumple esto, tenemos que
       $\langle S_1\ \mathtt{;}\ S_2, s \rangle \Rightarrow \langle S_2, s'
       \rangle$ con lo que $\langle \left( S_1\ \mathtt{;}\ S_2 \right)\
       \mathtt{;}\ S_3, s \rangle \Rightarrow \langle S_2\ \mathtt{;}\ S_3, s'
       \rangle$

   - **Caso inductivo**. Si $k > 1$, tenemos que aplicar $\left[
       \mathrm{comp}_{\mathrm{sos}}^1 \right]$ con lo que tenemos $\langle S_1\
       \mathtt{;}\ \left( S_2\ \mathtt{;}\ S_3 \right), s \rangle \Rightarrow
       \langle S_1'\ \mathtt{;}\ \left( S_2\ \mathtt{;}\ S_3 \right), s'
       \rangle$ y la premisa $\langle S_1, s \rangle \Rightarrow \langle S_1',
       s' \rangle$. Como antes, con esta premisa aplicada a la otra sentencia,
       tenemos que $\langle \left( S_1\ \mathtt{;}\ S_2 \right)\ \mathtt{;}\
       S_3, s \rangle \Rightarrow \langle \left( S_1'\ \mathtt{;}\ S_2 \right)\
       \mathtt{;}\ S_3, s' \rangle$. Sin embargo, como la longitud de la
       secuencia para llegar a $\gamma$ desde $\langle S_1'\ \mathtt{;}\ \left(
       S_2\ \mathtt{;}\ S_3 \right), s' \rangle$ es $k - 1$, podemos aplicar la
       hipótesis de inducción, obtenemos que $S_1'\ \mathtt{;}\ \left(
       S_2\ \mathtt{;}\ S_3 \right)$ y $\left( S_1'\ \mathtt{;}\ S_2 \right)\
       \mathtt{;}\ S_3$ son equivalentes y tenemos el resultado que buscábamos.
