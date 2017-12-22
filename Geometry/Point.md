# Point

### Declaración de constante π (pi)
```cpp
const double PI = 2*asin(1);
```
## Funciones auxiliares

### equals
```cpp

bool eq(double a, double b) { return fabs(a-b) < EPS; }

```

**Entrada:** Dos valores flotantes _a_ y _b_.

**Retorno:** Regresa si el primer parámetro _a_ es igual al segundo parámetro _b_.

Obtiene el valor absoluto de la resta de dos flotantes y asegura que sea menor a un margen de error _EPS_.

### less than
```cpp

bool les(double a, double b) { return !eq(a, b) && a < b; }

```

**Entrada:** Dos valores flotantes _a_ y _b_.

**Retorno:** Regresa si el primer parámetro _a_ es menor al segundo parámetro _b_.

Usa la función auxiliar _eq_ para asegurar que los valores de los parámetros no sean iguales y compara _a_ menor a _b_. Es necesario llamar la función _eq_ para asegurar que el punto flotante no sea igual dado el margen de _EPS_.

## Struct Point

```cpp

struct Point {
	double x, y, z;
	Point() : x(0), y(0), z(0) {}
	Point(double x, double y) : x(x), y(y), z(0) {}
	Point(double x, double y, double z) : x(x), y(y), z(z) {}
	bool operator <(const Point &p) const {
        return les(x, p.x) || (eq(x, p.x) && les(y, p.y)) || (eq(x, p.x) && eq(y, p.y) && les(z, p.z));
	}
    bool operator==(const Point &p) {
        return eq(x, p.x) && eq(y, p.y) && eq(z, p.z);
    }
};

```

### DEG_to_RAD

Conversión de grados a radianes.

```cpp

double DEG_to_RAD(double deg) {
    return deg/180*2*asin(1);
}

```

**Entrada:** Un valor flotante _deg_ que representa un valor en grados.

**Retorno:** Un valor flotante que representa el valor del parámetro en radianes.

Usa la fórmula tradicional para convertir de grados a radianes:
![alt text](http://www.radianstodegrees.net/img/degrees-to-radians2.png)


### dist

Obtiene la distancia entre dos puntos

```cpp

double dist(Point p1, Point p2) {
	return sqrt(pow(p1.x-p2.x, 2) + pow(p1.y-p2.y, 2) + pow(p1.z-p2.z, 2)); }
	
```

**Entrada:** Dos puntos _p1_ y _p2_.

**Retorno:** Valor flotante que representa la distancia entre los puntos dados como parámetro.

Usa la fórmula tradicional para obtener la distancia entre dos puntos:

![alt text](http://www.onlinemath4all.com/images/dbtpformula.png)

### rotate

```cpp

Point rotate(Point p, double theta) {
	double rad = DEG_to_RAD(theta);
	return Point(p.x*cos(rad) - p.y*sin(rad),
				 p.x*sin(rad) + p.y*cos(rad));
}

```

**Entrada:** Un Point _p_ relativo al origen del plano y un valor flotante _theta_ que representa el ángulo de rotación en grados.

**Retorno:** Un punto con la rotación del ángulo _theta_ dado.

Construye un objeto Point nuevo, pasando en el constructor los puntos ajustados al ángulo dado, usando la ecuación tradicional de rotación de puntos:

![alt text](https://wikimedia.org/api/rest_v1/media/math/render/svg/4e8193cc301ba228063af7ecdf292c2b8c7e76d3)

double ANG(double rad) { return rad*180/PI; }
double angulo(Point p) {
	double d = atan(double(p.y)/p.x);
	if(p.x < 0)
		d += PI;
	else if(p.y < 0)
		d += 2*PI;
	return ANG(d);
}
