# Unidad 1

## 🔎 Fase: Set + Seek

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

