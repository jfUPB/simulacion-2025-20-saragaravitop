# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 01

* **¿Cómo funciona la suma dos vectores en p5.js?      
  R//** se usa el método `.add()`, que suma componente a componente. 

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
