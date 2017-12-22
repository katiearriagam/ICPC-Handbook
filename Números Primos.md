Un número _a_ es un factor (o divisor) de _b_ si _a_ divide a _b_ sin residuo restante. Por ejemplo, los factores de 32 son 1, 2, 4, 8, 16 y 32. Un número _n_ (mayor a uno) es primo si sus únicos divisores son 1 y _n_. Por ejemplo, el número 7 es primo porque sus únicos divisores son 1 y 7.

Las siguientes funciones son muy útiles de conocer cuando se trabaja con problemas que involucran números primos y factores.

### Variables Globales

```cpp

#define SIZE 1000000
bitset<SIZE> sieve;
vi primesList;
```

### buildSieve

```cpp

void buildSieve() {
	sieve.set();
	sieve[0] = sieve[1] = 0;
	int root = sqrt(SIZE);
	FOR(i, 2, root+1)
		if (sieve[i])
			for(int j = i*i; j < SIZE; j+=i)
				sieve[j] = 0;
}
```
**Entrada:** Ninguna, pero utiliza el bitset sieve declarado de manera global por conveniencia de uso.

**Retorno:** No retorna ningún valor.

Construye un sieve que guarda en cada índice un 1 si el índice es primo y un 0 si no lo es. Primero establece el bitset con todos sus bits en 1, excepto por el índice 0 y 1. Luego itera de 2 a la raíz + 1. Si el índice actual está en 1, establece todos los índices múltiplos de i en 0, iniciando desde el cuadrado de i. Se repite esto hasta llegar a la raíz. Cuando llega a la raíz, todos los índices primos serán los únicos que no se accedieron y permanecerán en 1.


### buildPrimesList

```cpp

void buildPrimesList() {
	if(!sieve[2])
		buildSieve();
	primesList.reserve(SIZE/log(SIZE));
	FOR(i, 2, SIZE+1)
		if(sieve[i])
			primesList.pb(i);
}
```
**Entrada:** Ninguna, pero los números primos que genera los inserta en un vector de enteros declarado de manera global por conveniencia de uso.

**Retorno:** No retorna ningún valor.

Utilizando el sieve construido en la función buildSieve(), obtiene todos los números primos de 2 a SIZE+1. Si el sieve no está construido aún, esta función manda a llamar la función que lo construye. Luego reserva la cantidad de elementos que contendrá el vector. Finalmente itera de 2 a SIZE+1, si el iterador i está en el sieve, lo mete en la lista primesList.


### primeFactorization

```cpp

vii primeFactorization(int N) {
	vii factors;
	int idx = 0, pf = primesList[0];
	while(pf*pf <= N) {
		while(N%pf==0) {
			N /= pf;
			if(factors.size() && factors.back().first == pf)
				factors.back().second++;
			else
				factors.pb(ii(pf, 1));
		}
		pf = primesList[++idx];
 	}
	if(N!=1) factors.pb(ii(N, 1));
	return factors;
}
```
**Entrada:** Un entero del cual se quiere obtener su factorización prima.

**Retorno:** Un vector de pares de enteros. Cada par tiene la siguiente representación: el primer índice del par representa al número primo dentro de la factorización prima, y el segundo índice del par representa la cantidad de veces que el número del primer índice se repite en la factorización prima. Por ejemplo: La factorización prima de 540 es 2^2 * 3^3 * 5^1. Entonces esta función retornaría un vector de pares como el siguiente: < (2,2), (3,3), (5,1) >.

Antes de llamar esta función se debe de tener una lista de números primos, la cual se puede generar utilizando las otras funciones. Ya que se tiene esta lista, se empieza con el primer número primo. Se iterará hasta la raíz de N y, dentro de este ciclo, se inicia un nuevo ciclo que dividirá N por el factor primo actual. Mientras se pueda dividir, se incrementará la cantidad de veces que aparece este número primo en la factorización prima. Si es la primera vez que este número primo divide a N, se agrega a la lista con valor de 1. Si no es la primera vez, se le suma 1 al segundo índice del par actual. Cuando el número primo actual ya no puede dividir a N, se obtiene el siguiente número primo de la lista de números primos y se repite el proceso, hasta que el número primo actual sea mayor a la raíz de N. Cuando termina este proceso, se revisa si N es igual a 1. Si no lo es, quiere decir que N, después de haberlo dividido, es el último factor primo, y se agrega a la lista.


### getDivisors

```cpp

void getDivisors(vii pf, int d, int index, vi &div) {
	if (index == pf.size()) {
		div.pb(d);
		return;
	}
	for (int i = 0; i <= pf[index].second; i++) {
		getDivisors(pf, d, index+1, div);
		d *= pf[index].first;
	}
	return;
}
```
**Entrada:** Vector de pares de enteros que representan una factorización prima, un entero que representa al divisor actual, un entero que representa el índice en el que inicia a iterar sobre los factores primos y un vector de enteros pasado como referencia (inicialmente vacío).

**Retorno:** No retorna valor, pero inserta los divisores en el vector de enteros que recibió como referencia.

Esta función es recursiva. Si el índice que recibe es igual al tamaño del vector de factor primos (en donde el tamaño del vector de factores primos representa la cantidad de números primos distintos presentes en el vector), insertas el valor actual de d en el vector de divisores. Si no, iteramos con un for loop la cantidad de veces que el factor primo actual se repite en la factorización prima. Para cada repetición, llamamos recursivamente a getDivisors() con los mismos parámetros pero incrementando el índice en 1 y luego multiplicamos al divisor actual por el factor primo actual. Cuando este ciclo se repita, llamará recursivamente la función de nuevo, pero ahora el divisor a insertar será con una multiplicación más de la factorización prima. Esto se repetirá recursivamente hasta que se hayan multiplicado todos los factores primos y agregado al vector de divisores cada combinación de la multiplicación, generando todos los divisores posibles.


### divisors

```cpp

vi divisors(ll N) {
	vii pf = primeFactorization(N);
	vi div;
	getDivisors(pf, 1ll, 0, div);
	sort(div.begin(), div.end());
	return div;
}
```
**Entrada:** Un entero del que se quieren obtener sus divisores.

**Retorno:** Vector de números enteros, divisores de N.

Dado un número N, regresa todos los divisores de N. Primero, para hacer esto, se obtiene la factorización prima de N. La factorización prima de N es una secuencia de números primos que, al multiplicarlos, resultan en N. Luego se manda a llamar la función getDivisors() que, dada la factorización prima de N, dos números enteros y un vector de enteros pasado como referencia, regresa el vector de enteros que se pasó con todos los divisores de N. Finalmente se ordenan ascendentemente (este paso es opcional) y termina la función.


### IsPrime

```cpp

bool isPrime(int n) {
	if(n < 2) return false;
	if(n == 2 || n == 3) return true;
	if(!(n&1 && n%3)) return false;
	long long sqrtN = sqrt(n)+1;
	for(long long i = 6LL; i <= sqrtN; i += 6)
		if(!(n%(i-1)) || !(n%(i+1))) return false;
	return true;
}

```

**Entrada:** Un entero del que se desea saber si es primo o no.

**Retorno:** Valor booleano indicando si el número de entrada es primo o no.

* Si un número es menor a 2, no es primo.
* Si un número es 2 o 3, es primo.
* Si un número es par o divisible entre 3 y el número es mayor a 2 o 3, no es primo.

De ahí en adelante iteramos hasta la raíz del número. Si un número n no es primo, éste se puede representar como un producto a · b, donde a ≤ √n o b ≤ √n, entonces es seguro que uno de los factores está dentro del rango de 2 a √n. Sabiendo que uno de los factores es ≤ √n, podemos iterar únicamente hasta la raíz de n, checando si n % i (n modulo i) regresa 0 como residuo. Si llegamos a la raíz y ningún número lo dividió sin dejar residuo, entonces sabemos que el número es primo. Empezando en 5 e incrementando por 6, podemos saltarnos todos los múltiplos de 2 y 3. Esto nos permite checar únicamente dos tercios de todos los números dentro del rango de 2 a √n. Dentro del if, tenemos que revisar si n es divisible entre i-1 e i+1, ya que son los únicos valores dentro de nuestro incremento que no hemos revisado. Esta función corre en tiempo O(√n).
