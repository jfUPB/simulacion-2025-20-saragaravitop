# Evidencias de la unidad 6

### Actividad 01

* Estas son las imagenes que me gustaron: 

<img width="1072" height="395" alt="image" src="https://github.com/user-attachments/assets/5b979f96-e669-453a-a644-55e7a2a96432" /> 


> Estas me gustaron mucho por como se ven, me gustan mucho los colores que se usan y el efecto que genera. Me hizo pensar en la obra de Van Gogh "la noche estrellada" porque se ve como si fueran diferentes pinceladas, es una obra que personalmente me parece increíble por la tecnica tan única. También, mientras observaba la obra, me imaginaba como se vería en movimiento y me gustó también por eso, porque se vería muy genial también por eso. 


<img width="1071" height="524" alt="image" src="https://github.com/user-attachments/assets/efab144b-5cfd-487d-9911-ff7bccc45519" />

> Estas imagenes me gustaron por el volumen que genera, mientras las pasaba parecia que tuvieran movimiento y cuando me sente a analizarlo me gusto mucho la sensación que genera, es como el parallax en animación, de como utilizar elementos 2D en 3D solamente con la perspectiva.


* Creo que lo que mas me inspira del trabajo de Tyler Hobbs es su creatividad, la capacidad de crear que tiene, de simular diferentes tecnicas y estilos. Me inspira también que esa creatividad no es solo de manera visual en el resultado de la obra sino en la generación del código. De las cosas que mas me gustan de esta materia es la conexión que tiene el arte (desde lo visual), con la generación de código, para mi eran dos cosas que no iban juntas, y que si iban juntas no llegaban a niveles tan extraordinarios, es decir, de una forma u otra siempre supe que iban de la mano, porque yo animo en programas que fueron creados mediante la programación, pero digamos que veía eso muy lejano, crear con programación, como dije antes, lo veia lejano, algo que jamás conectaría conmigo, y eso es lo que me inspira de Tyler, que él si combina elementos artisticos que me gustan mucho con la programación. 

### Actividad 02

1. ¿qué es una fuerza de dirección (steering force)?

> es un vector que representa el ajuste en la trayectoria actual para que se aproxime a la trayectoria deseada. Es la diferencia entre la velocidad deseada y la actual. 

2. ¿Qué diferencia tiene este tipo de fuerza con las que ya hemos estudiado en el contexto de la simulación de agentes?

> La diferencia es que en el contexto de la simulación de agentes la fuerza no es una fuerza física real, sino que es una que diseñamos para modelar el comportamiento del agente, por eso depende de lo que se quiera hacer con el mismo. Otra diferencia, es que no es constante porque se va recalculando en cada frame y tiene un límite máximo para simular el comportamiento natural de diferentes mecánicas biológicas. 

3. ¿Qué relación tiene la steering force con Craig Reynolds y su trabajo en simulación de comportamiento animal?

desarrollo el famoso modelo de los "Boids", que simulaba el movimiento de peces o bandadas de pajaros. 

contribuyó introduciendo reglas 
* la separación: evitar chocar con vecinos.
* alineación: moverse en la misma dirección de los vecinos.
* cohesión: moverse hacia el centro del grupo.

estas reglas se implmentan con steering forces, y la relación es que estos son la base matemática de todo el modelo. 


### Actividad 03

**1. estructura de datos**

> La estructura del campo se guarda en un array 2D dentro de la clase Flowfield.
> * cada elemento de la matriz es un vector. 
> * el vector indica la dirección del flujo en la celda de la rejilla. 
> * cuando un vehiculo cae en esa celda, usa a esa vector como su velocidad deseada. 

**¿cómo se calcula cada vector?**  

> se recorre cada celda de la rejilla (por columna y fila), para cada celda se genera un perlin noise que cambia el espacio, luego ese valor se convierte en un angulo y con ese angulo se crea un vector unitario que se guarda enla matriz. 

**2. Cómo un agente utiliza el campo para calcular su fuerza de dirección**

> Primero se mapea sus coordenadas de posición a los índices de la cuadricula del campo de flujo. una vez se obtiene esa posición (la del agente), se divide por la resolución de la cuadricula, ese resultado el agente lo busca en la estructura de datos del campo de flujo, y para calcular la fuerza, primero mira la dirección en que va la corriente del flujo, para poder ir en esa dirección, se calcula la diferencia entre la velocidad deseada y la velocidad que el vector tiene en ese momento, el resultado es la fuerza de corrección que se necesita para cambiar la velocidad actual a la deseada.

**3. Lista los parámetros clave identificados**

> * resolución: define el tamaño de cada celda en la cuadricula del campo de flujo. tambien controla las corrientes. 
> * maxspeed: es la que establece el limite de velocidad de un agente, también controla que tan rapido puede moverse por el lienzo. 
> * maxforce: es la que limita la fuerza de corrección que un agente puede aplicar para cambiar la dirección del vector, tambien controla la capacidad de giro, como la habilidad para hacerlo. 

4. Modificiones

* **Resolución muy gruesa (Baja Precisión). Cambié la resolución de 20 a 5.**

> **Efecto que tuvo:** A medida que un agente se mueve, cruza los bordes de estas pequeñas baldosas constantemente, por lo que en cada cruce recibe una nueva instrucción de dirección, que solo es un poco diferente a la anterior. Al hacer esto de esta forma, le permite a los agentes seguir las curvas suaves y sutiles generadas por el perlin noise con mayor precisión, haciendo que el movimiento sea mas natural. 

<img width="583" height="224" alt="image" src="https://github.com/user-attachments/assets/957daa8d-b688-48dc-8948-909a494c74e1" />

* **Resolución muy fina (alta presición). Cambié la resolución de 20 a 80.**

> **Efecto que tuvo:** dentro de una de estas enormes baldosas, todos los agentes  reciben la misma instrucción de dirección, logrando que se muevan en grandes grupos sincronizados, aún así, el movimiento se ve tosco porque las transiciones de dirección son infrecuentes y abruptas.

  <img width="583" height="221" alt="image" src="https://github.com/user-attachments/assets/18ab9f13-e26c-4091-8f10-46d5cb4f6715" />






