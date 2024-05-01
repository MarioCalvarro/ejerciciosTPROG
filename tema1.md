# El lenguaje WHILE. Ejercicios
## Ejercicio 1.4
### Enunciado
Siendo la sintaxis para $n$

    n ::= 0 | 1 | 0 n | 1 n

¿Se puede definir $\mathcal{N}$ correctamente?
### Resolución
La semántica $\mathcal{N}$ para esta sintaxis se podría obtener trivialmente
utilizando la misma que para el caso en que los constructores compuestos
tuviesen el orden inverso. De esta forma, lo único que cambiaría sería que "la
lectura" del número se haría de izquierda a derecha. Si intuitivamente pensamos
en $n$ como un número binario, diríamos que los bits menos significativos son
los de la izquierda a diferencia de lo usual (que son los de la derecha).

Sin embargo, si queremos mantener el convenio de lectura de estos numerales,
debemos hacer uso de una función auxiliar. Esta se deberá definir
composicionalmente y simplemente es la longitud:
$$
\mathrm{long}: \mathrm{Num} \rightarrow \mathbb{Z}\\
\mathrm{long}\left( 0 \right) = \mathrm{long}\left( 1 \right) = 1\\
\mathrm{long}\left( 0\ n \right) = \mathrm{long}\left( 1\ n \right) = 1 + \mathrm{long}\left( n \right)
$$
En definitiva, la función semántica será:
$$
\mathcal{N}: \mathrm{Num} \rightarrow \mathbb{Z}\\
\mathcal{N}\llbracket 0 \rrbracket = 0\\
\mathcal{N}\llbracket 0 \rrbracket = 1\\
\mathcal{N}\llbracket 0\ n \rrbracket = \mathcal{N}\llbracket n \rrbracket\\
\mathcal{N}\llbracket 1\ n \rrbracket = 2^{\mathrm{long}\left( n \right)} + \mathcal{N}\llbracket n \rrbracket
$$

## Ejercicio 1.10
### Enunciado
Probar que la definición dada para la función semántica de expresiones booleanas
es total.
$$
\mathcal{B}: \mathbf{Bexp} \rightarrow \left( \mathbf{State} \rightarrow
\mathbf{T} \right)
$$
### Resolución
Para que esta función semántica sea total, simplemente necesitamos que para toda
expresión booleana, $b$, y todo estado posible, $s$, existe un solo $t \in
\mathbf{T}$ tal que $\mathcal{B}\llbracket b \rrbracket s = t$. Lo probaremos
por inducción estructural.
- Casos base:
    - Si $b \equiv \mathrm{true}$, entonces $\forall s \in \mathbf{State}$ tenemos
      que $\mathcal{B}\llbracket b \rrbracket s = \mathrm{tt}$.
    - Si $b \equiv \mathrm{false}$, entonces $\forall s \in \mathbf{State}$ tenemos
      que $\mathcal{B}\llbracket b \rrbracket s = \mathrm{ff}$.
    - Si $b \equiv a_1 = a_2$, entonces tenemos solo dos casos posibles:
        - Si $\mathcal{A}\llbracket a_1 \rrbracket s = \mathcal{A}\llbracket a_2
          \rrbracket s \Rightarrow \mathcal{B}\llbracket b \rrbracket s = \mathrm{tt}$.
        - Si $\mathcal{A}\llbracket a_1 \rrbracket s \neq \mathcal{A}\llbracket a_2
          \rrbracket s \Rightarrow \mathcal{B}\llbracket b \rrbracket s = \mathrm{ff}$.
    - Si $b \equiv a_1 \le a_2$, entonces tenemos solo dos casos posibles:
        - Si $\mathcal{A}\llbracket a_1 \rrbracket s \le \mathcal{A}\llbracket a_2
          \rrbracket s \Rightarrow \mathcal{B}\llbracket b \rrbracket s = \mathrm{tt}$.
        - Si $\mathcal{A}\llbracket a_1 \rrbracket s > \mathcal{A}\llbracket a_2
          \rrbracket s \Rightarrow \mathcal{B}\llbracket b \rrbracket s = \mathrm{ff}$.

  Con lo que los casos base se cumplen.

- Casos inductivos:
    - Si $b \equiv ¬ b_0$ y suponemos que se cumple que $\mathcal{B}\llbracket
      b_0 \rrbracket s,\ \forall s \in \mathbf{State}$ tiene un solo valor,
      entonces solo tenemos dos posibilidades:
        - Si $\mathcal{B}\llbracket b_0 \rrbracket s = \mathrm{tt} \Rightarrow \mathcal{B}\llbracket b \rrbracket s = \mathrm{ff}$.
        - Si $\mathcal{B}\llbracket b_0 \rrbracket s = \mathrm{ff} \Rightarrow \mathcal{B}\llbracket b \rrbracket s = \mathrm{tt}$.
    - Si $b \equiv b_0 \land b_1$ y suponemos que se cumple que $\mathcal{B}\llbracket
      b_i \rrbracket s,\ \forall s \in \mathbf{State}, i \in \left\{ 0, 1 \right\}$ tiene un solo valor,
      entonces solo tenemos dos posibilidades:
        - Si $\mathcal{B}\llbracket b_0 \rrbracket s = \mathrm{tt} \text{ y } \mathcal{B}\llbracket b_1 \rrbracket s = \mathrm{tt} \Rightarrow \mathcal{B}\llbracket b \rrbracket s = \mathrm{tt}$.
        - Si $\mathcal{B}\llbracket b_0 \rrbracket s = \mathrm{ff} \text{ ó } \mathcal{B}\llbracket b_1 \rrbracket s = \mathrm{ff} \Rightarrow \mathcal{B}\llbracket b \rrbracket s = \mathrm{ff}$.

  Con lo que también se cumple para los casos recursivos (ya que los operadores
  son funciones completas).

## Ejercicio 1.11
### Enunciado
Dar una función semántica $\mathcal{B}$ para la siguiente categoría sintáctica
$\mathbf{Bexp}'$:
    
    b ::= true | false | a1 = a2 | a1 != a2 | a1 <= a2 | a1 >= a2
        | a1 < a2 | a1 > a2 | ¬b | b1 and b2 | b1 or b2
        | b1 implies b2 | b1 if and only if b2

Demostrar que para todo $b' \in \mathbf{Bexp}'$, existe un $b \in \mathbf{Bexp}$
tal que son equivalentes.
### Resolución
En primer lugar, extendemos de manera natural y composicionalmente la función
semántica de $\mathbf{Bexp}$ para que cubra todo $\mathbf{Bexp}'$. Los términos
comunes no los repetiremos:
$$
\mathcal{B}: \mathbf{Bexp}' \rightarrow \left( \mathbf{State} \rightarrow
\mathbf{T} \right)\\
\mathcal{B}\llbracket a_1 \neq a_2 \rrbracket s = \begin{cases}
    \mathbf{tt},\ \text{ si } \mathcal{A}\llbracket a_1 \rrbracket s \neq \mathcal{A}\llbracket a_1 \rrbracket s\\
    \mathbf{ff},\ \text{ c.c.}
\end{cases}\\

\mathcal{B}\llbracket a_1 \ge a_2 \rrbracket s = \begin{cases}
    \mathbf{tt},\ \text{ si } \mathcal{A}\llbracket a_1 \rrbracket s \ge \mathcal{A}\llbracket a_1 \rrbracket s\\
    \mathbf{ff},\ \text{ c.c.}
\end{cases}\\

\mathcal{B}\llbracket a_1 < a_2 \rrbracket s = \begin{cases}
    \mathbf{tt},\ \text{ si } \mathcal{A}\llbracket a_1 \rrbracket s < \mathcal{A}\llbracket a_1 \rrbracket s\\
    \mathbf{ff},\ \text{ c.c.}
\end{cases}\\

\mathcal{B}\llbracket a_1 > a_2 \rrbracket s = \begin{cases}
    \mathbf{tt},\ \text{ si } \mathcal{A}\llbracket a_1 \rrbracket s > \mathcal{A}\llbracket a_1 \rrbracket s\\
    \mathbf{ff},\ \text{ c.c.}
\end{cases}\\

\mathcal{B}\llbracket b_1 \lor b_2 \rrbracket s = \begin{cases}
    \mathbf{tt},\ \text{ si } \mathcal{A}\llbracket b_1 \rrbracket s = \mathbf{tt} \text{ ó } \mathcal{A}\llbracket b_2 \rrbracket s = \mathbf{tt}\\
    \mathbf{ff},\ \text{ c.c.}
\end{cases}\\

\mathcal{B}\llbracket b_1 \Rightarrow b_2 \rrbracket s = \begin{cases}
    \mathbf{ff},\ \text{ si } \mathcal{A}\llbracket b_1 \rrbracket s = \mathbf{tt} \text{ y } \mathcal{A}\llbracket b_2 \rrbracket s = \mathbf{ff}\\
    \mathbf{tt},\ \text{ c.c.}
\end{cases}\\

\mathcal{B}\llbracket b_1 \Leftrightarrow b_2 \rrbracket s = \begin{cases}
    \mathbf{ff},\ \text{ si } \mathcal{A}\llbracket b_1 \rrbracket s = \mathbf{tt} \text{ y } \mathcal{A}\llbracket b_2 \rrbracket s = \mathbf{ff} 
    \text{ ó } \mathcal{A}\llbracket b_2 \rrbracket s = \mathbf{tt} \text{ y } \mathcal{A}\llbracket b_1 \rrbracket s = \mathbf{ff}
    \\
    \mathbf{tt},\ \text{ c.c.}
\end{cases}\\
$$

Para demostrar ahora la equivalencia tenemos que buscar expresiones de
$\mathbf{Bexp}$ equivalentes a las de $\mathbf{Bexp}'$ por inducción
estructural. Para las que están en común será directo. Empezamos por los casos
base:
- Si $b' \equiv a_1 \neq a_2$ tomamos $b \equiv ¬\left( a_1 = a_2 \right)$.
- Si $b' \equiv a_1 \ge a_2$ tomamos $b \equiv ¬\left[ \left( a_1 \le a_2 \right) \land ¬\left( a_1 = a_2 \right)\right]$.
- Si $b' \equiv a_1 < a_2$ tomamos $b \equiv \left( a_1 \le a_2 \right) \land ¬\left( a_1 = a_2 \right)$.
- Si $b' \equiv a_1 > a_2$ tomamos $b \equiv ¬\left( a_1 \le a_2 \right)$.

Veamos ahora los casos inductivos. Asumimos que se cumple para los
constituyentes $b_1$ y $b_2$:
- Si $b' \equiv b_1 \lor b_2$ tomamos $b \equiv ¬\left( ¬b_1 \land ¬b_2 \right)$ (De Morgan).
- Si $b' \equiv b_1 \Rightarrow b_2$ tomamos $b \equiv ¬\left( b_1 \land ¬b_2 \right)$.
- Si $b' \equiv b_1 \Leftrightarrow b_2$ tomamos $b \equiv ¬\left( b_1 \land ¬b_2 \right) \land ¬\left( ¬b_1 \land b_2 \right)$.

Para probarlo de forma completamente rigurosa sería necesario calcular de forma
exacta la función semántica de estos términos y ver que son iguales en ambos
casos, pero en este caso se puede apreciar simplemente utilizando las reglas
lógicas de sobra conocidas o realizando tablas de verdad.

## Ejercicio 1.13
### Enunciado
Sean dos estados $s$ y $s'$ tales que $\forall x \in \mathrm{FV}\left( b
\right)$ se cumple que $s\ x = s'\ x$. Demostrar que $\mathcal{B}\llbracket b
\rrbracket s = \mathcal{B}\llbracket b \rrbracket s'$.
### Resolución
Lo realizaremos por inducción estructural. Veamos primero los casos base:
- Si $b \equiv \mathrm{true}$ ó $\mathrm{false}$, entonces se cumple
  trivialmente.
- Si $b \equiv a_1 = a_2$ ó $a_1 \le a_2$, entonces, si escribimos $op \in \left\{ =, \le \right\}$, tenemos que:
    $$
    \mathcal{B}\llbracket b \rrbracket s = \begin{cases}
        \mathbf{tt},\ \text{ si } \mathcal{A}\llbracket a_1 \rrbracket s\ op\
        \mathcal{A}\llbracket a_2 \rrbracket s\\
        \mathbf{ff},\ \text{ c.c.}
    \end{cases}
    $$
    pero como $\mathcal{A}\llbracket a \rrbracket s = \mathcal{A}\llbracket a
    \rrbracket s'$ (por el lema 1.12), 
    $$
    \begin{cases}
        \mathbf{tt},\ \text{ si } \mathcal{A}\llbracket a_1 \rrbracket s\ op\
        \mathcal{A}\llbracket a_2 \rrbracket s\\
        \mathbf{ff},\ \text{ c.c.}
    \end{cases} = \begin{cases}
        \mathbf{tt},\ \text{ si } \mathcal{A}\llbracket a_1 \rrbracket s'\ op\
        \mathcal{A}\llbracket a_2 \rrbracket s'\\
        \mathbf{ff},\ \text{ c.c.}
    \end{cases} = \mathcal{B}\llbracket b \rrbracket s'
    $$
    y, por tanto, se cumple el resultado.

Veamos ahora los casos composicionales. Supongamos que se cumple para los
constituyentes inmediatos $b_1$ y $b_2$:
- Si $b \equiv ¬b_1$. Como $\mathrm{FV}\left( b \right) = \mathrm{FV}\left( ¬b
    \right)$ podemos aplicar la hipótesis de inducción y $\mathcal{B}\llbracket
    b_1 \rrbracket s = \mathcal{B}\llbracket b_1 \rrbracket s'$. Ahora, con esto,
    tenemos que:
    $$
    \mathcal{B}\llbracket b \rrbracket s = \begin{cases}
    \mathbf{tt},\ \text{ si } \mathcal{B}\llbracket b_1 \rrbracket s = \mathbf{tt}\\
    \mathbf{ff},\ \text{ c.c }
    \end{cases} = \begin{cases}
    \mathbf{tt},\ \text{ si } \mathcal{B}\llbracket b_1 \rrbracket s' = \mathbf{tt}\\
    \mathbf{ff},\ \text{ c.c }
    \end{cases} = \mathcal{B}\llbracket b \rrbracket s'
    $$
    con lo que probamos el resultado.

- Si $b \equiv b_1 \land b_2$. Como $\mathrm{FV}\left( b \right) =
    \mathrm{FV}\left( b_1 \right) \cup \mathrm{FV}\left( b_2 \right)$ tenemos
    que $\mathrm{FV}\left( b_i \right) \subset \mathrm{FV}$ por lo que podemos
    aplicar la hipótesis de inducción, es decir, $\mathcal{B}\llbracket
    b_i \rrbracket s = \mathcal{B}\llbracket b_i \rrbracket s',\ i \in \left\{
    1, 2 \right\}$. Con esto,
    $$
    \mathcal{B}\llbracket b \rrbracket s = \begin{cases}
    \mathbf{tt},\ \text{ si } \mathcal{B}\llbracket b_1 \rrbracket s = \mathbf{tt} \text{ y } \mathcal{B}\llbracket b_2 \rrbracket s = \mathbf{tt} \\
    \mathbf{ff},\ \text{ c.c }
    \end{cases}\\ = \begin{cases}
    \mathbf{tt},\ \text{ si } \mathcal{B}\llbracket b_1 \rrbracket s' = \mathbf{tt} \text{ y } \mathcal{B}\llbracket b_2 \rrbracket s' = \mathbf{tt} \\
    \mathbf{ff},\ \text{ c.c }
    \end{cases} = \mathcal{B}\llbracket b \rrbracket s'
    $$
    con lo que probamos el resultado.

## Ejercicio 1.14
### Enunciado (Teorema fundamental de sustituciones)
Demostrar que, para todo estado $s$, se cumple:
$$
\mathcal{A}\llbracket a\left[ y \mapsto a_0 \right] \rrbracket s =
\mathcal{A}\llbracket a \rrbracket \left( s \left[ y \mapsto
\mathcal{A}\llbracket a_0 \rrbracket s \right] \right).
$$
### Resolución
Lo demostraremos por inducción estructural sobre $a$. Casos base:
- $a \equiv n \Rightarrow \llbracket n\left[ y \mapsto a_0 \right] \rrbracket s =
    n,\ \forall s$. Por otro lado, $\llbracket n \rrbracket \underbrace{\left( s \left[ y
    \mapsto \llbracket a_0 \rrbracket s \right] \right)}_{s'} = n$, por lo
    que tenemos el resultado.
- $a \equiv x \in \mathrm{Var} \Rightarrow \llbracket x \left[ y
    \mapsto a_0 \right] \rrbracket s = \begin{cases}
    \llbracket a_0 \rrbracket s,\ \text{ si } x = y\\
    s\ x,\ \text{ si } x \neq y
    \end{cases}$. Por otro lado, $\llbracket x \rrbracket \underbrace{\left( s \left[ y
    \mapsto \llbracket a_0 \rrbracket s \right] \right)}_{s'}$ donde:
    $$
    s'\ x = \begin{cases}
        s\ x,\ \text{ si } x \neq y\\
        \llbracket a_0 \rrbracket s,\ \text{ si } x = y
    \end{cases}.
    $$ 
    Por lo que tenemos el resultado.

Veamos que se da ahora para los casos composicionales. Supongamos que se da para
los constituyentes inmediatos $a_1$ y $a_2$ y que $op$ se refiere a $+, -$ ó
$*$.
- $a \equiv a_1\ op\ a_2 \Rightarrow \llbracket \left( a_1\ op\ a_2 \right)
    \left[ y \mapsto a_0 \right] \rrbracket s = \llbracket a_1 \left[ y \mapsto
    a_0 \right] \rrbracket s\ op\ \llbracket a_2 \left[ y \mapsto a_0 \right]
    \rrbracket s$. Por otro lado,
    $$
    \llbracket a_1\ op\ a_2 \rrbracket \underbrace{\left( s \left[ y \mapsto
    \llbracket a_0 \rrbracket s \right] \right)}_{s'} = \llbracket a_1
    \rrbracket s'\ op\ \llbracket a_2 \rrbracket s'.
    $$
    Pero por hipótesis de inducción tenemos que $\llbracket a_i \left[ y \mapsto
    a_0 \right] \rrbracket s = \llbracket a_i \rrbracket s',\ i \in \left\{ 1, 2
    \right\}$, por lo que el resultado buscado se cumple.
