El siguiente _template_ se puede utilizar como una plantilla al iniciar un nuevo programa. Esta plantilla facilita y agiliza el desarrollo del programa al definir macros que son más cortos de escribir durante el proceso de codificación.

```cpp

#include <bits/stdc++.h>
#define _ ios_base::sync_with_stdio(0), cin.tie(0), cout.tie(0);
#define INF 1000000000
#define FOR(i, a, b) for(int i=int(a); i<int(b); i++)
#define FORC(cont, it) for(decltype((cont).begin()) it=(cont).begin(); it!=(cont).end(); it++)
#define pb push_back
#define mp make_pair
#define eb emplace_back
#define fi first
#define se second
#define all(x) (x).begin(), (x).end()
using namespace std;
typedef long long ll;
typedef pair <int, int> ii;
typedef vector<int> vi;
typedef vector<ii> vii;
typedef vector<vi> vvi; 
```

Enseguida se describe el propósito de cada uno de estos estatutos.

```cpp

#include <bits/stdc++.h>
```

Encabezado que incluye todas las librerías estándares de c++.

```cpp

#define _ ios_base::sync_with_stdio(0), cin.tie(0), cout.tie(0);
```

**ios_base::sync_with_stdio(0):** Deshabilita la sincronización entre los streams estándares de C y C++. Esto permite a los streams de C++ tener buffers independientes de entrada y de salida, lo que hace más rápidas estas operaciones (Input/Output).

**Cin.tie(0), Cout.tie(0):** Desliga _cin_ de _cout_. Sin esta opción, nuestro programa haría _flushing_ automáticamente antes de cada operación de I/O, lo que consume mucho tiempo de nuestro programa.

```cpp

#define INF 1000000000
```

Definimos un número muy grande como infinito. Se utiliza como un _shortcut_ que, además de ser más corto, hace mucho más legible el código cuando queremos revisar si hemos rebasado un valor límite superior.

```cpp

#define FOR(i, a, b) for(int i=int(a); i<int(b); i++)
```

Forma simplificada de realizar un ciclo _for_. Recibe como parámetros el nombre de la variable a usar como contador, su valor inicial, y el límite superior al que llegará. Este _for_ incrementa la variable recibida como parámetro en 1 cada iteración, por lo que si el ciclo for requiere un comportamiento distinto (i.e. incrementos diferentes a 1, ir decrementando, etc.), se tendría que escribir de la forma normal.

```cpp

#define FORC(cont, it) for(decltype((cont).begin()) it=(cont).begin(); it!=(cont).end(); it++)
```
Forma simplificada de realizar un ciclo _for_ que utiliza iteradores de estructuras que no se acceden por medio de su índice, tales como el _map_ o el _set_. Recibe como parámetros el nombre de la variable a iterar y el nombre a darle al iterador.

```cpp

#define pb push_back
#define mp make_pair
#define eb emplace_back
#define fi first
#define se second
```
Forma simplificada de escribir las instrucciones a la derecha de cada una.

```cpp

#define all(x) (x).begin(), (x).end()
```

Forma corta de hacer referencia al rango completo de una estructura, de inicio a fin. La estructura debe de ser iterable, es decir, se le debe de poder hacer una referencia .begin() y .end(), tal como en un _vector_ o en un _map_.

```cpp

using namespace std;
```

Evitamos tener que escribir el prefijo _std::_ antes de usar elementos de la librería estándar de C++.

```cpp

typedef long long ll;
typedef pair <int, int> ii;
typedef vector<int> vi;
typedef vector<ii> vii;
typedef vector<vi> vvi;
```
Crea un alias que se puede usar en cualquier parte del programa en lugar del nombre de un tipo de dato.
