# Unidad 2


## 🛠 Fase: Apply

**1. Describe el concepto de tu obra generativa.**        
La obra se llama "Atracción Orgánica", y representa un sistema de partículas que orbitan alrededor del puntero del mouse, como si fueran cuerpos atraídos por una fuerza invisible.
El comportamiento simula patrones naturales como enjambres, campos magnéticos o incluso sistemas galácticos, pero con un toque artístico y caótico.
La interacción con el mouse permite controlar el "centro de atracción", y cada partícula responde de manera fluida gracias al uso de ruido Perlin, mientras que eventos raros introducidos por probabilidades le dan un toque impredecible.

**2. ¿Cómo piensas aplicar el marco MOTION 101 y por qué?**

Motion 101 se basa en que la posición cambia con la velocidad, y la velocidad cambia con la aceleración, teniendo esto en cuenta, yo lo aplique dee esta forma, cada partícula tiene posición, velocidad y aceleración (vectores), en cada draw(), la aceleración se calcula hacia el mouse (a veces rotada para dar órbitas), esa aceleración se suma a la velocidad, y la velocidad a la posición, esto permite un movimiento realista y controlado por física básica.

> Elegí usar Motion 101 porque me da control total sobre cómo se mueve cada partícula y, al mismo tiempo, me permite experimentar con algoritmos complejos sin perder la naturalidad del movimiento.

**3. ¿Qué algoritmo de aceleración vas a utilizar? ¿Por qué?**
Usé una combinación de: 
* aceleración hacia el mouse, para que las partículas respondan a la posición del cursor.
* rotación del vector de dirección, para que el movimiento no sea directo, sino orbital.
* modulación de la fuerza con ruido Perlin, para que la aceleración cambie suavemente en el tiempo.
* eventos aleatorios raros (con random()) que hacen que algunas partículas “escapen” o cambien de comportamiento.

para crear un sistema con comportamientos naturales, pero que también sorprende al espectador, o al menos a mi me sorprendio.

**4. El contenido generado debe ser interactivo. Puedes utilizar mouse, teclado, cámara, micrófono, etc, para variar los parámetros del algoritmo en tiempo real.**

La interacción de mi obra funciona así: 
* La posición del mouse, que define el punto de atracción y afecta la dirección de todas las partículas.
* La obra se transforma visual y dinámicamente cuando se mueve el mouse.

**6. El código de la aplicación.**
```js
let particles = [];

function setup() {
  createCanvas(800, 600);
  for (let i = 0; i < 200; i++) {
    particles.push(new Particle(i * 0.1));
  }
  background(0);
}

function draw() {
  fill(0, 25);
  rect(0, 0, width, height);

  for (let p of particles) {
    p.update();
    p.show();
  }
}

class Particle {
  constructor(noiseOffset) {
    this.position = createVector(random(width), random(height));
    this.velocity = p5.Vector.random2D().mult(random(0.5, 2));
    this.acceleration = createVector(0, 0);
    this.noiseOffset = noiseOffset; 
    this.time = random(1000);
    this.r = map(noise(this.noiseOffset), 0, 1, 2, 6);
    this.col = color(
      map(noise(this.noiseOffset + 10), 0, 1, 100, 255),
      map(noise(this.noiseOffset + 20), 0, 1, 100, 255),
      map(noise(this.noiseOffset + 30), 0, 1, 100, 255),
      220
    );
  }

  update() {
    let target = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(target, this.position);

    let noiseFactor = map(noise(this.time), 0, 1, 0.1, 0.5);
    dir.normalize();
    dir.rotate(HALF_PI);
    dir.mult(noiseFactor); 

    this.acceleration = dir;
    this.velocity.add(this.acceleration);
    this.velocity.limit(4);
    this.position.add(this.velocity);

    if (random(1) < 0.002) {
      this.velocity.add(p5.Vector.random2D().mult(10));
    }

    this.time += 0.01;
  }

  show() {
    noStroke();
    fill(this.col);
    ellipse(this.position.x, this.position.y, this.r);
  }
}
```

**7. Un enlace al proyecto en el editor de p5.js.**      
[actividad 08](https://editor.p5js.org/saragaravitop/sketches/f80OWqPkn)    

**8. Una captura de pantalla representativa de tu pieza de arte generativo.**   
<img width="588" height="501" alt="image" src="https://github.com/user-attachments/assets/f06e68b9-ee86-493a-a399-94a813396b88" />
