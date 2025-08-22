# Unidad 3


## 🤔 Fase: Reflect

### Actividad 11

**Parte 1: recuperación de conocimiento (Retrieval Practice)**

**1. Escribe la ecuación vectorial de la segunda ley de Newton y explica cada uno de sus componentes.**         
diavlo que dificil me la pusiste.        
broma, la ecuación es *`F=m.a`*, donde: 
* F, es la fuerza aplicada
* m, es la masa del objeto al que le es aplicada la fuerza.
* a, es la aceleración (o la posible) del objeto
   
**2. ¿Por qué es necesario multiplicar la aceleración por cero en cada frame del método update()?**
porque de no hacerlo se acumularían las fuerzas en el mismo frame y eso haría que la aceleración fuera irreal. Al multiplicarla por cero es como si se limpiara el lienzo en cada frame. 
   
**3. Explica la diferencia entre paso por valor y paso por referencia cuando aplicamos fuerzas a un objeto.**
* paso por valor: se pasa una copia del dato, por lo que cualquier cambio no afecta al original (ejemplo: un número).
* paso por referencia: se pasa la dirección del objeto, de modo que cualquier cambio sí modifica el objeto original. 
   
**4. ¿Cuál es la diferencia conceptual entre modelar fuerzas (como fricción, gravedad) y simplemente definir algoritmos de aceleración?**
Modelar fuerzas implica usar conceptos de física de manera mas efectiva, y creo que los algoritmos de aceleración son los que no tienen en cuenta los conceptos de fisica y por eso la aceleración es menos real. 
   
**Parte 2: reflexión sobre tu proceso (Metacognición)**

**1. ¿Qué fue lo más desafiante en la Actividad 10 (problema de los n-cuerpos)? ¿El concepto creativo, la implementación de las fuerzas o la integración de la interactividad?**
Como claramente no supe en que parte aplique el codigo de los n-cuerpos pues diría que lo mas dificil fue la implementación de este porque entender el concepto fue dificil tambien, me demore mucho pensando como entenderlo de manera "creativa", para luego implementar lo de Calder. 
   
**2. ¿Las fuerzas que modelaste produjeron el comportamiento que esperabas? Describe un momento “sorpresa” (esperado o inesperado) durante el desarrollo.**
No tuve ningún momento sorpresa lol, por el contrario, tuve momentos donde la obra que yo planeaba en mi cabeza salia incorrectamente en el codigo. 
   
**3. ¿Cómo ha cambiado tu forma de pensar sobre la “física” en el arte generativo después de esta unidad?**
Yo ya sabia que la física era un aspecto muy importante en el diseño porque al estar en cada uno de los aspectos del comportamiento del mundo y del ser humano, y como animadora busco replicar estos comportamientos pues valoraba mucho ciertos conceptos, pero como muchos de los conceptos que vi en esta unidad eran procesados por un programa yo no tenia idea de como eran aplicados desde la conexión codigo - concepto. 
   
**4. Si tuvieras una semana más, ¿Qué otras fuerzas te gustaría modelar o cómo mejorarías tu simulación del problema de los n-cuerpos?**
En este momento solo se me ocurre, que si tuviera una semana más aprendería mejor donde esta el codigo de los n-cuerpos en el código que pensé conceptualmente.

### Actividad 12

Compartí mi obra con Valeria Zuñiga. 

**1. En tu propia bitácora, escribe una retroalimentación constructiva para tu compañero, evaluando:**      

 * Claridad del Concepto: ¿La obra visual refleja la inspiración en las esculturas cinéticas de Calder y el problema de los n-cuerpos?
    > me parece que conceptualmente lo logra, como en el balanceo en un espacio dinámico y hace buen uso del problema de los n-cuerpos, solo diría que visualmente le falta un poco, en cuanto a los colores y formas.
*  Implementación de Fuerzas: ¿Se aplican correctamente las leyes de Newton? ¿Las fuerzas se acumulan apropiadamente?
     > Sí, se nota el uso correcto de las leyes de Newton porque la fuerza se calcula a partir de la distancia, se limita para evitar explosiones, y luego se convierte en aceleración que se acumula hasta actualizar la velocidad y posición.
* Creatividad en el Modelado: ¿El modelado de fuerzas es interesante y genera comportamientos únicos?
   > Sí, me parece interesante porque también integro Perlin noise para simular viento y vi que usó Lévy flight para los pasos largos aleatorios, haciendo que no se vea mecánico, por ende siempre es un movimiento diferente. 
* Calidad de la Interactividad: ¿La interacción permite explorar diferentes aspectos del sistema de fuerzas?
     > Pues la interacción es normal, con cada nuevo clic se agrega partículas, pero lo interesante es que también cambia el movimiento del sistema completo.
     
2. Conversa con tu compañero sobre las obras que cada uno creó. En tu bitácora, comenta:
  * ¿Qué fue lo que más te llamó la atención del trabajo de tu compañero?
      > Le dije a Val que lo que más me gustó de su trabajo fue la implementación de sus algoritmos de la unidad 1, porque compartimos lo del perlín noise y que ella penso una manera interesante de usar el Levy Flight porque a mi no se me ocurrió. 
  * ¿Qué aprendiste de su enfoque para modelar fuerzas?
      > Le dije a Val que la combinación de ruido y aleatoriedad en su obra fue creativo e inesperado. 
  * ¿Qué técnica o idea de su implementación te gustaría aplicar en tus futuros proyectos?
      > la idea del Lévy flight, como lo he dicho, porque da un balance entre movimientos pequeños y saltos inesperados, haciendo que los sistemas se sientan más vivos.
      

### Actividad 13

**1. Continuar: ¿Qué actividad o concepto de esta unidad te resultó más “revelador” para entender las fuerzas en el arte generativo?**    
Todos los conceptos de esta unidad me gustaron mucho, la física me parece un tema muy interesamte y mas en este tema de la creación de arte generativo porque a mi me gusta mucho crear pero no me gusta casi programar, aún asi me he divertido haciendo ambas cosas. 
   
**2. Dejar de hacer: ¿Hubo alguna actividad que te pareció redundante o menos efectiva para comprender el modelado de fuerzas?**      
yo creo que todas para mi fueron muy divertidas de realizar porque me permitieron entender tanto el concepto como la aplicación. 

**3. Progresión conceptual: ¿El paso de manipular aceleración directamente (unidad 2) a modelar fuerzas (unidad 3) te pareció una progresión natural y efectiva? ¿Por qué?**     
Sí me parecio natural y efectiva porque primero es necesario entender la cinemática antes de la dinámica de los cuerpos. 
   
**4.Conexión arte-física: ¿Cómo te ha ayudado esta unidad a ver la conexión entre conceptos físicos y expresión artística? ¿Te sientes más cómodo “jugando” con las leyes de la física en tus creaciones?**      
Se podría decir, creo que nunca hubiera pensando que podría jugar con las leyes de la física. 

**5. Comentario adicional: ¿Algo más que quieras compartir sobre tu experiencia explorando fuerzas en el arte generativo?**    
Fue una buena forma de ver los conceptos de física, como ya lo había mencionado antes. 
