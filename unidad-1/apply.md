# Unidad 1

## 🛠 Fase: Apply

### Actividad 08

**Conceptos que quiero usar**      
* caminatas aleatorias
* ruido perlin
* distribución de probabilidad.

**Concepto de mi obra generativa**       
Los conceptos principales de mi obra generativa giraron al rededor de como al combinar caminatas aleatorias y ruido perlin puedo generar pequeñas animaciones que me permitan representar movimientos fluidos y vivos, tales como dos cosas que me representan, los girasoles y el baile (elementos que amo y que son especiales para mi). Por ende, lo que pasa en mi obra es como la representacion de estos conceptos se dibujan y cambian conforme el usuario interactua con la obra. 

**Explicación de los conceptos de manera mas detallada:**      
Las caminatas aleatorias definen el movimiento individual de las partículas, dándoles trayectorias impredecibles que permiten construir la imagen de manera fragmentada y única en cada ejecución. El ruido Perlin se utiliza para guiar el flujo general del movimiento de las partículas, logrando un desplazamiento más suave y orgánico. Por último, aplico una distribución de probabilidad para determinar cuál de las dos imágenes se muestra al iniciar la obra, lo que introduce un factor de sorpresa y variación controlada.

**¿Cómo funciona la obra? (interacción)**        
*para que la funcione la opción del teclado primero se debe de clickear el lienzo, de lo contrario no se percibe ningún cambio*      

* con el mouse (click) se puede cambiar de una imagen a otra.
* con la tecla 1 se cambia la imagen a escala de grises.
* con la tecla 2, se invierten los colores.
* con la tecla 3, se cambia la imagen a sepia.
* con la tecla 0, se vuelve al color original de la obra.

**Código**     
```js
let girasol, bailarina;
let currentImage;
let particles = [];
let numParticles = 800;
let tintColor;
let colorModeType = 'tint'; 
let c; // canvas

function preload() {
  girasol = loadImage('girasol.png');
  bailarina = loadImage('bailarina.png');
}

function setup() {
  c = createCanvas(650, 450);
  c.elt.focus(); // Asegura que el canvas reciba eventos de teclado
  imageMode(CENTER);
  pickRandomImage();
  initParticles();
  background(000);
  tintColor = color(255, 255, 255);
}

function draw() {
  for (let p of particles) {
    p.update();
    p.show();
  }
}

function pickRandomImage() {
  currentImage = random(1) < 2.0 ? girasol : bailarina;
}

function mousePressed() {
  currentImage = currentImage === girasol ? bailarina : girasol;
  initParticles();
  background(000);
}

function keyPressed() {
  console.log("Tecla presionada:", key); // Para depuración

  if (key === ' ') {
    tintColor = color(random(150, 255), random(150, 255), random(150, 255));
    colorModeType = 'tint';
  } else if (key === '1') {
    colorModeType = 'bw';
  } else if (key === '2') {
    colorModeType = 'invert';
  } else if (key === '3') {
    colorModeType = 'sepia';
  } else if (key === '0') {
    colorModeType = 'tint';
  }
}

function initParticles() {
  particles = [];
  for (let i = 0; i < numParticles; i++) {
    particles.push(new Particle());
  }
}

class Particle {
  constructor() {
    this.x = random(width);
    this.y = random(height);
    this.stepSize = random(5, 1);
  }

  update() {
    this.x += random(-this.stepSize, this.stepSize);
    this.y += random(-this.stepSize, this.stepSize);
    this.x = constrain(this.x, 0, width - 1);
    this.y = constrain(this.y, 0, height - 1);
  }

  show() {
    let imgX = int(map(this.x, 0, width, 0, currentImage.width));
    let imgY = int(map(this.y, 0, height, 0, currentImage.height));
    let c = currentImage.get(imgX, imgY);

    let r, g, b;

    if (colorModeType === 'bw') {
      let avg = (red(c) + green(c) + blue(c)) / 3;
      r = g = b = avg;
    } else if (colorModeType === 'invert') {
      r = 255 - red(c);
      g = 255 - green(c);
      b = 255 - blue(c);
    } else if (colorModeType === 'sepia') {
      let or = red(c);
      let og = green(c);
      let ob = blue(c);
      r = constrain((or * 0.393) + (og * 0.769) + (ob * 0.189), 0, 255);
      g = constrain((or * 0.349) + (og * 0.686) + (ob * 0.168), 0, 255);
      b = constrain((or * 0.272) + (og * 0.534) + (ob * 0.131), 0, 255);
    } else {
      r = red(c) * red(tintColor) / 255;
      g = green(c) * green(tintColor) / 255;
      b = blue(c) * blue(tintColor) / 255;
    }

    noStroke();
    fill(r, g, b, 100);
    circle(this.x, this.y, 2);
  }
}
```
**Enlace**       
[Actividad 08](https://editor.p5js.org/saragaravitop/sketches/UJLYsjOgA)      

**Captura de pantalla**      
<img width="656" height="455" alt="image" src="https://github.com/user-attachments/assets/b62bf5f4-dc86-4b12-9e6d-c466f3e49ef8" />


