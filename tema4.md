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
donde $c_1'$ puede ser $\varepsilon$.

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
\langle c_1, e, s \rangle \triangleright \langle c_1', e_1, s_1 \rangle \triangleright^{k_1'} \langle \varepsilon, e', s' \rangle
$$
