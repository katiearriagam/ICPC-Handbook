Un _segment tree_ es una estructura de datos que permite dos operaciones:
1) Procesar una consulta dentro de un rango
2) Actualizar un valor del arreglo que lo compone.

Los _segment trees_ pueden soportar consultas de suma, consultas de mínimos y máximos y otras operaciones de tipo asociativas. Generalmente estas operaciones corren en tiempo O(log(n)).

Para construir un _segment tree_, asumimos que el tamaño del arreglo es una potencia de dos y se usa la indexación basada en cero, ya que es más conveniente construir un árbol de segmentos para dicho arreglo. Si el tamaño del arreglo no es una potencia de dos, se le pueden añadir valores “dummy” para que el tamaño sea una potencia de 2.

**Ejemplo**

Para el siguiente arreglo, se muestra su _segment tree_ de sumas correspondiente.

![alt text](https://i.imgur.com/jJgV7Cd.png)

En el árbol, cada nodo corresponde a la suma de los valores de sus hijos, izquierdo y derecho.
Las ventajas de acomodar la información en esta estructura es que las consultas sobre un rango toman menos tiempo. Si hiciéramos una consulta de la suma de los valores del índice 2 al índice 7, esto implicaría revisar 6 celdas si se hiciera sobre el arreglo.

![alt text](https://i.imgur.com/Jvra9sw.png)

En cambio, si utilizamos un segment tree, solamente revisamos los nodos que cubren esta suma.

![alt text](https://i.imgur.com/7OZb7XW.png)

Esto reduce el tiempo de la consulta de O(n) a O(log(n)).

La siguiente es una implementación de un _segment tree_ utilizando un vector de enteros como su arreglo.

```cpp

struct SegmentTree {
  vi t; int N;
  SegmentTree(vi &values) {
    N = values.size();
    t.assign(N<<1, 0);
    for(int i = 0; i < N; i++) t[i+N] = values[i];
    for(int i = N-1; i; --i) t[i] = combine(t[i<<1], t[i<<1|1]);
  }
  int combine(int a, int b) { return a+b; }
  void set(int index, int value) {
    t[index+N] = value;
    for(int i = (index+N)>>1; i; i >>= 1) t[i] = combine(t[i<<1], t[i<<1|1]);
  }
  int query(int from, int to) {
    int ansL = 0, ansR = 0;
    for(int l = N+from, r = N+to; l<r; l >>= 1, r >>= 1) {
      if (l&1) ansL = combine(ansL, t[l++]);
      if (r&1) ansR = combine(ansR, t[--r]);
    }
    return combine(ansL, ansR);
  }
};
```

A continuación se explica cada una de las funciones.

### SegmentTree

**Entrada:** Lista de valores enteros.

**Salida:** Ninguna.

Constructor del _segment tree_. Inicializa el valor de _N_ con el tamaño de la lista, luego separa el doble de este valor de casillas para el vector de enteros _t_, y le asigna el valor 0 a cada una.

### combine

**Entrada:** Dos valores enteros.

**Salida:** La suma de los dos valores recibidos como entrada.

La idea es que esta función reciba el valor de dos nodos del árbol cuya suma represente la suma de un rango de la lista de números que forman el árbol.

### set

**Entrada:** Dos valores enteros, uno representando un índice y otro un valor para este índice.

**Salida:** Ninguna.



### query
