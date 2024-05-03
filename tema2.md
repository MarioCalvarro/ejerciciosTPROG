# Semántica operacional básica. Ejercicios
## Ejercicio 2.8
### Enunciado
Extender el lenguaje **WHILE** con la instrucción `for`:

    for x := a1 to a2 do S

Definir la relación $\rightarrow$ para esta construcción.

### Resolución
Veamos en primer lugar la definición:
$$
\left[ \mathrm{for}_{\mathrm{ns}}^{\mathrm{tt}} \right] := \frac{\langle S, s\left[ x \mapsto
\mathcal{A}\llbracket a_1 \rrbracket s\right] \rangle \rightarrow s', \langle \mathtt{for}\ x := x\ \mathtt{to}\ a_2\ \mathtt{do}\ S, s'\left[ x \mapsto \mathcal{A}\llbracket x + 1 \rrbracket s \right]
\rangle \rightarrow s''}{\langle \mathtt{for}\ x := a_1\ \mathtt{to}\ a_2\ \mathtt{do}\ S, s
\rangle \rightarrow s''},\\ \text{ si } \mathcal{B}\llbracket a_1 \le a_2
\rrbracket s = \mathbf{tt}
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

- La definición no puede ser composición puesto que tenemos que capturar la
    «iteratividad» de la instrucción. Por eso, en las premisas tenemos un `for`,
    pero con un par de peculiaridades:
    - La asignación de la $x$ es sobre sí misma para que no se haga cada
        iteración.
    - En vez de ser el estado $s'$ directamente, tenemos que hemos sustituido el
        valor de $x$ por ella misma más $1$ para capturar que el bucle tiene un
        número limitado de iteraciones.

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
