# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 01
creo que la aleatoriedad le da al arte generativo una sensación de realidad y/o naturalidad puesto que se puede generar mediante la libertad de ser algo random (dentro de ciertos limites). Me da la sensación de, a pesar de hacerse con máquinas que teoricamente no sienten, pueden darle vida al arte mediante esta opción, como lo haria una persona que si siente. 

### Actividad 02

* **Cuál es el papel de la aleatoriedad en su obra.**  
  me parece que en la obra de Sofi la aleatoriedad aporta de manera especial al hacer que las raíces parezcan vivas, tanto por los movimientos como por la intensidad de los colores y la forma que estas toman, ya que no son movimientos rígidos o estructurados (como se espera que sean las máquinas) sino que son movimientos fluidos, como si las raíces estuvieran vivas y bailaran fluidamente al rededor de la música. Creo que es un trabajo muy lindo que aplica la aleatoriedad de una manera muy poética (si nos sentamos a pensar más en el concepto que transmite y no en el tecnicismo matemático).
  
* **Según tu perfil profesional, cómo se aplica el concepto de aleatoriedad en el tipo de proyectos que desarrollas. Ilustra tu respuesta con ejemplos concretos.**     
  Mi perfil profesional lo estoy encaminando hacia la creación de mundos (por así decirlo), me estoy desarrollando no solo como animadora (que es la rama que elegí) sino como artista, es decir, hay muchas cosas que todavia quiero desarrollar hacia un nivel mas profesional, sin dejar de lado el hecho de que en algunos casos debo de enfocarme mas en ciertos perfiles; pero por el momento ese es el plan, así que veo la aleatoriedad como:       

  > la capacidad o la opción de creación libre bajo unos limites especificos para crear orden y no caos.   

  Por ejemplo, si quiero crear un personaje no solo necesito características físicas, sino que también debo de pensar en que esas características físicas tengan un sentido dentro del mundo, la historia y el pasado de mi personaje (una creación libre, pero con límites, como la coherencia), así mismo como para animar, modelar o hacer rigging a un personaje o escenario, debo pensar en como funciona el mundo natural para no hacer cosas rígidas o sin sentido, poder darle vida a lo inanimado. Finalmente y mas importante, la creación de muchos de los proyectos en la carrera se basan en generar una experiencia para el usuario y al hacer eso tambien hay que marcar límites por las tecnologias usadas, el contexto y gustos de mi usuario. 

### Actividad 03

modificaciones 
1. quiero cambiar el color, por lo que voy a agregar la opción de que se muestre dicho color en el walker. Al ejecutar el código, sí se cambiaba de color pero como no modifique cada cuanto se moviera (se mueve 1 pixel cada vez) entonces no se identifican los colores claramente.
 ```js
show() {
    stroke(random(255), random(255), random(255));
    strokeWeight(2);
    point(this.x, this.y);
  }
 ```

2. Voy a modificar cada cuanto se mueve el walker para que se identifiquen mas los colores, entonces hare que se mueva cada 4 pixeles. Al ejecutar el codigo si se identifican mas los colores pero investigué que entonces el walker se mueve en los multiplos de 4 entonces genera una cuadricula 4x4 dejando un punto cada que pasa.
 ```js
const stepSize = 4;
 ```

### Actividad 04

* **En tus propias palabras cuál es la diferencia entre una distribución uniforme y una no uniforme de números aleatorios**
  entiendo que es como los valores se reparten en cierto rango, porque en la uniforme se muestran los valores mas uniformemente (no se favorece ningún número en especifico), y en no uniforme esto no existe, se muestran los valores que estén más cercanos a la media.

* **Modifica el código de la caminata aleatoria para que utilice una distribución no uniforme, favoreciendo el movimiento hacia la derecha**

 ```js
function setup() {
  createCanvas(100, 100);
  background(200);

  describe('Tres líneas horizontales con distribuciones diferentes. La superior es uniforme, la del medio favorece la derecha, la inferior usa distribución gaussiana amplia.');
}

function draw() {
  noStroke();
  fill(0, 10);

  let x = random(100);
  let y = 25;
  circle(x, y, 5);

  
  x = pow(random(1), 0.5) * 100;   y = 50;
  circle(x, y, 5);

  x = randomGaussian(50, 10);
  y = 75;
  circle(x, y, 5);
}
 ```


### Actividades pasadas 

### Actividad 05

**Explicación del sketch**     
El ejemplo que use para representar una distribución normal fue mediante unas lineas de colores donde cada línea vertical se dibuja con una x basada en esta distribución, las líneas se van acumulando, y se forma una zona más densa en el centro (donde la probabilidad es mayor). adicional, la opacidad es baja, así que, también, se ve la acumulación por densidad.   

**Código**
```js
function setup() {
  createCanvas(600, 400); 
  background(255);        
  colorMode(HSB);        
}

function draw() {
  let x = randomGaussian(width / 2, 80);
  let hue = map(x, 0, width, 0, 360);
  stroke(hue, 100, 80, 0.2); // Color HSB con opacidad baja (0.2)
  line(x, 0, x, height);
}
```

**Enlace**      
[Actividad 05](https://editor.p5js.org/saragaravitop/sketches/yfY3M9duw)     

**Captura del sketch**

<img width="670" height="407" alt="image" src="https://github.com/user-attachments/assets/1bf23965-1b0e-4a60-9046-323800d7c27e" />

### Actividad 06

Para esta actividad use el ejemplo de la distribución normal que hice en la actividad 05 y lo modifiqué para incluir un Lévy flight. 
Use esta técnica porque me parecio muy curioso el concepto de Lévy flight, que es un tipo de movimiento aleatorio en el que la mayoría de los pasos son cortos, pero de vez en cuando hay saltos muy largos. Lo que se esperaba obtener es ese resultado dentro del ejemplo, que es generar pasos cortos y saltos largos entre las lineas de colores. 

**Explicación del ejemplo**
> En lugar de solo dibujar líneas verticales en posiciones basadas en una distribución normal, ahora un punto "caminará" por el lienzo usando un patrón de Lévy flight, y cada vez que se mueva, dibujará una línea en su nueva posición, creando patrones impredecibles, que combina zonas densas de pasos cortos con explosiones visuales causadas por saltos largos.

**Código** 
```js
let pos;

function setup() {
  createCanvas(600, 400);
  background(255);
  colorMode(HSB);
  pos = createVector(width / 2, height / 2);
}

function draw() {
  let angle = random(TWO_PI);
  let stepSize = levy();
  let step = p5.Vector.fromAngle(angle);
  step.mult(stepSize);
  pos.add(step);

  if (pos.x < 0 || pos.x > width || pos.y < 0 || pos.y > height) {
    pos.set(width / 2, height / 2);
  }
  let hue = map(pos.x, 0, width, 0, 360);
  stroke(hue, 100, 100, 0.2);
  line(pos.x, 0, pos.x, height); 
}

function levy() {
  let r = random(1);
  return 1 / pow(r, 1.5); 
}
```

**Enlace**   
[Actividad 06](https://editor.p5js.org/saragaravitop/sketches/yfY3M9duw)    

**Captura de pantalla**  
<img width="667" height="430" alt="image" src="https://github.com/user-attachments/assets/0b3e7ec7-61e1-43b5-ade6-f7b3cf81bea1" />

### Actividad 07

Para entender como funcionaba el ruido perlin, pense en el ejemplo que usaba Two-Dimensional Noise, porque quería simular comportamientos naturales y fluidos, por eso "cree" un escenario donde vemos el movimiento del agua, el desplazamiento de una criatura flotante (una ameba) y los patrones visuales de luz al "reflejarse" en el mar. 

Lo que esperaba obtener una animación visualmente rica que se sintiera natural, sin saltos ni cambios bruscos. El fondo debía parecer agua real, y la criatura debía moverse como si estuviera suspendida y arrastrada suavemente por corrientes invisibles.

**Explicación del ejemplo**
> Usé ruido Perlin para que todo el sketch se sintiera más natural y fluido. En lugar de usar random(), que da saltos bruscos, el noise() genera valores más suaves, ideales para simular agua y movimiento orgánico. Lo usé en el fondo para crear una textura animada, en los reflejos de luz para que brillen suavemente, y en el movimiento de la ameba para que parezca que nada o flota como si estuviera viva.

**Código**
```js
let bubbles = [];
let creature;
let lightOffset = 0;

function setup() {
  createCanvas(600, 400);
  noStroke();
  colorMode(HSB, 360, 100, 100, 100);

  for (let i = 0; i < 30; i++) {
    bubbles.push(new Bubble());
  }

  creature = new Amoeba();
}

function draw() {
  drawWaterBackground();
  drawLightReflections();
  
  for (let b of bubbles) {
    b.update();
    b.display();
  }
  creature.update();
  creature.display();
}

function drawWaterBackground() {
  loadPixels();
  let yoff = 0;

  for (let y = 0; y < height; y++) {
    let xoff = 0;
    for (let x = 0; x < width; x++) {
      let n = noise(xoff, yoff, frameCount * 0.01);
      let hue = 200 + n * 30;
      let sat = 80 + n * 20;
      let bright = 70 + n * 30;

      let c = color(hue, sat, bright);

      let index = (x + y * width) * 4;
      pixels[index] = red(c);
      pixels[index + 1] = green(c);
      pixels[index + 2] = blue(c);
      pixels[index + 3] = 255;

      xoff += 0.01;
    }
    yoff += 0.01;
  }

  updatePixels();
}

function drawLightReflections() {
  blendMode(ADD);
  fill(255, 30);
  for (let i = 0; i < width; i += 60) {
    let offset = sin((frameCount + i) * 0.02) * 20;
    ellipse(i + offset, 50 + sin(i * 0.1 + frameCount * 0.01) * 10, 100, 10);
  }
  blendMode(BLEND);
}

class Bubble {
  constructor() {
    this.x = random(width);
    this.y = random(height);
    this.r = random(5, 15);
    this.speed = random(0.5, 1.5);
    this.alpha = random(50, 100);
  }

  update() {
    this.y -= this.speed;
    if (this.y < -this.r) {
      this.y = height + this.r;
      this.x = random(width);
    }
  }

  display() {
    fill(0, 0, 100, this.alpha);
    ellipse(this.x, this.y, this.r);
  }
}

class Amoeba {
  constructor() {
    this.xoff = random(1000);
    this.yoff = random(1000);
  }

  update() {
    this.x = noise(this.xoff) * width;
    this.y = noise(this.yoff) * height;
    this.xoff += 0.005;
    this.yoff += 0.005;
  }

  display() {
    fill(300, 80, 100, 60); 
    ellipse(this.x, this.y, 30 + sin(frameCount * 0.1) * 5, 30 + cos(frameCount * 0.1) * 5);
    fill(0, 0, 0, 20);
    ellipse(this.x + 5, this.y - 5, 5, 5); 
  }
}
```
**Enlace**    
[Actividad 07](https://editor.p5js.org/saragaravitop/sketches/X0S-o2Izw)    

**Captura de pantalla**      
<img width="606" height="406" alt="image" src="https://github.com/user-attachments/assets/3fc8e6c0-5200-4419-90e7-5e84f3355a3e" />


