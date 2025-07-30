# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 01

* **¿Cómo funciona la suma dos vectores en p5.js?      
  R//** se usa el método `.add()`, que suma componente a componente (internamente al usar ese método). 

* **¿Por qué esta línea position = position + velocity; no funciona?      
  R//** no funciona porque position y velocity no son numeros sino que son objetos, entonces no los suma correctamente. 

### Actividad 02

* **¿Qué tuviste que hacer para hacer la conversión propuesta?       
R//** lo primero que tuve que hacer fue que en lugar de variables x e y separadas, tuve que reemplazar `this.x` y `this.y` por un solo vector llamado `this.position`, para poder manejasr la posición del walker con una sola variable.
Luego, tuve que crear un vector de paso (step) en lugar de modificar x o y directamente, para poder representar la dirección del movimiento como un vector. Finalmente, utilicé el método .add() de p5.Vector para sumar el vector de movimiento a la posición.

**Código**
```js
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.position = createVector(width / 2, height / 2);
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y);
  }

  step() {
    let step = createVector(0, 0);
    const choice = floor(random(4));

    if (choice == 0) {
      step.x = 1;
    } else if (choice == 1) {
      step.x = -1;
    } else if (choice == 2) {
      step.y = 1;
    } else {
      step.y = -1;
    }
    this.position.add(step);
  }
}
```

### Actividad 03

**¿Qué resultado esperas obtener en el programa anterior?   
R//** de acuerdo a lo que explico el profe, esperaba que el vector de position tuviera los valores (6, 9) inicialmente, y que después de llamar a la función, esos valores cambiaran a (20, 30). También esperaba ver ambos estados impresos en la consola.

**¿Qué resultado obtuviste?   
R//**  El resultado fue parecido pero, la consola mostró los valores esperados (6, 9) primero y luego (20, 30) después de modificar el vector, solo que ocurrió en una variable distinta llamada posicion, no en position (era un error de nombre). 

**Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.  
R//**  

**ejemplos pasos por valor**       

Cuando pasamos tipos a una función, se pasa una copia del valor, y el original no se modifica.
```js
function cambiarValor(x) {
  x = 10;
}
let numero = 5;
cambiarValor(numero);
console.log(numero); 
```
**ejemplo pasos por referencia**     

Cuando pasamos un objeto, se pasa una referencia al original, y cualquier cambio afecta el original.
```js
function modificarVector(v) {
  v.x = 100;
}
let v = createVector(10, 10);
modificarVector(v);
console.log(v.x); 
```

**¿Qué tipo de paso se está realizando en el código?     
R//** Se está realizando un paso por referencia, ya que se está pasando un objeto p5.Vector a una función. Al modificar las propiedades x y y dentro de la función, se está modificando el mismo objeto original.

**¿Qué aprendiste?   
R//** que los objetos en javaScript se pasan por referencia, porque modificarlos dentro de una función afecta el objeto original. Es importante usar nombres de variables consistentes para evitar errores, los valores, usar el paso por referencia es útil pero también puede causar errores si no se maneja con cuidado y finalmente, si se quiero evitar modificar el original, debo de hacer una copia del objeto. 

### Actividad 04

* **¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?    
  R//** `mag()` devuelve la magnitud del vector. La diferencia con `magSq()`, es que este devuelve la magnitud al cuadrado. `magSq()` es mas eficiente porque evita calcular la raíz cuadrada (que es mas dificil computacionalmente). 
  
* **¿Para qué sirve el método normalize()?     
  R//** este método sirve para convertir el vector en unitario, es decir, le cambia la magnitud a 1, pero mantiene su dirección.
  
* **Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?    
  R//** le diría: el método `dot()` mide si dos vectores apuntan en la misma dirección, es decir, si son paralelos o perpendiculares. 
  
* **El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?     
  R//** que ambas versiones devuelven el mismo valor, pero la forma en que llamas al método es diferente.
  
* **Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.     
  R//** la interpretación geometrica es que al hacer el producto cruz de dos vectores, sale un tercer vector en una dirección perpendicular a los otros dos (dependiendo del resultado). para saber hacia donde apunta este vector, en física me enseñaron que se usa la regla de la mano, y para saber la magnitud es ver que tan larga es el area del paralelogramo (que es el vector resultante del producto cruz).
  
* **¿Para que te puede servir el método dist()?     
  R//** sirve para saber la distancia entre dos puntos (vectores).

* **¿Para qué sirven los métodos normalize() y limit()?     
  R//** `normalize()` convierte el vector a dirección unitaria (1) y `limit()` restringe la magnitud del vector a un valor máximo sin cambiar la dirección.
