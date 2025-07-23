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
