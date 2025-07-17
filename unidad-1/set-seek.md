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

### Actividad 02

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





