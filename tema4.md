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
