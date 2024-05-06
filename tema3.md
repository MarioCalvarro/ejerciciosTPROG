# Semántica operacional básica. Ejercicios
## Ejercicio 3.2
### Enunciado
Extender la semántica operacional de paso corto de **WHILE** con la instrucción:

    assert b before S

Demostrar la equivalencia semántica entre `assert true before S` y `S`, pero que
`assert false before S` no es equivalente a `skip` ni a `while true do skip`.

### Resolución
En primer lugar, la definición de su semántica será:
$$
\left[ \mathrm{assert}_{\mathrm{sos}} \right] := \langle \mathtt{assert}\ b\
\mathtt{before}\ S, s \rangle \Rightarrow \langle S, s \rangle, \text{ si }
\mathcal{B}\llbracket b \rrbracket = \mathbf{tt}
$$
Es decir, solo está definido para el caso en que la condición se cumpla. En
cualquier otro, simplemente tenemos *undefined*.

Veamos ahora la equivalencia. Claramente, $\langle \mathtt{assert\ true}\
\mathtt{before}\ S, s \rangle \Rightarrow \langle S, s \rangle$ con lo que es
equivalente a $S$.

Veamos ahora las *no* equivalencias. Por la propia definición, para $\langle
\mathtt{assert\ false\ before}\ S, s \rangle$ no existe ninguna derivación que se le
pueda aplicar. Sin embargo, $\langle \mathtt{skip}, s \rangle \Rightarrow s$ y
$\langle \mathtt{while\ true\ do}\ S, s \rangle \Rightarrow \langle \mathtt{if}\ \mathtt{true}\ \mathtt{then}\ \left( S\ \mathtt{;}\ \mathtt{while}\ \mathtt{true}\ \mathtt{do}\ S\ \right)\ \mathtt{else}\ \mathtt{skip}, s \rangle \Rightarrow \langle S\ \mathtt{;}\ \mathtt{while}\ \mathtt{true}\ \mathtt{do}\ S\, s \rangle$ con lo que es un bucle infinito, pero está definido en todo paso.
