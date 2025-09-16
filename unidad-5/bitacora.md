# Evidencias de la unidad 5

### Actividad 02

#### 1. ejemplo 4.2: an array of particles

¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?
> La creación de las particulas se da en draw(), se crea una nueva instancia de la clase Particle. En el origin es donde nacen las particulas (como un punto fijo). Cada particula tiene su posición, velocidad, aceleración y una vida util (para que si aparezcan y desaparezcan). Y el array es el contenedor donde se guarda toda esta información.
> Las particulas desaparecen con el lifespan, cuando ese valor llega a 0, o menos se mueren las particulas. En el arreglo, si isDead es true, la particula ya no tiene vida útil (para asi mantener el bucle sin que se rompa), y splice, la elimina de la memoria.

experimento
1. conceptos usados:
    * unidad 1: distribución de probabilidad
    * unidad 2: motion 101
    * unidad 5: sistema de partículas básico 


#### 2. ejemplo 4.4 a system of systems

¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?   
> Ls creación de las particulas se da por medio de un sistema, que se llama emisor, el cual tiene su propio origen, este método crea una nueva instancia y la agrega al array interno del emisor. Como funciona cada que se hace click en la pantalla, si hay varios emisores activos, al hacer clic en diferentes partes del lienzo se generan particulas de forma independiente.
> en este caso, la desaparición de las particulas tambien se da por medio de un lifespan, el cual, cuando llega a 0, se muere la particula, y el emisor recorre el array para eliminar las que ya no estan activas.

#### 3. ejemplo 4.5: a Particle System with Inheritance and Polymorphism.

¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?      
> la creación de partículas en este ejemplo funciona en que en cada frame se añade una nueva particula al sistema, que puede ser de cualquier subclase. Cada particula tiene el metodo isDead que es el que indica cuando ya esten muertas y se usa splice para eliminarlas. Esto se logra de manera mas rapida y eficiente por el uso de herencia y polimorfismo.

#### 4. ejemplo 4.6 a particle system with forces.

¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?      
> en este ejemplo tambien usan emisores para la creación de particulas, el emisor les da una posición fija. a las particulas se les puede aplicar una fuerza antes de actualizar su movimiento. Nuevamente se usa el lifespan, pero en esta ocasión va disminuyendo con el tiempo y cuando finalmente llega a 0, se eliminan las particulas usando el splice.

#### 5. ejemplo 4.7: a particle system with a repeller.

¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?      
> para la creación de partículas tambien se usa un emisor, con posición, velocidad aceleración y con el lifespan, que se agregan al array interno del sistema. En este caso, se agregan fuerzas y un repeller, que modifican la aceleración de cada particula, lo que afecta la trayectoria.
> El lifespan va disminuyendo en cada frame, hasta que llega a 0 y la particula se considera muerta, ya el sistema recorre el array y elimina las que ya no están activas. 

