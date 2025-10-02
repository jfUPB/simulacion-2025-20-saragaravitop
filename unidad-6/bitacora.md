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


### Actividad 04

**1. Explicación de las tres reglas**

> **Regla 1: separación** 
> * objetivo: evitar colisiones. 
> * lógica: Se calcula un vector que aleja al boid de sus vecinos cercanos. Mientras mas cerca, ma fuerte la fuerza de repulsión. 

> **Regla 2: alineación**
> * objetivo: coordinar la dirección. 
> * logica: se calcula el promedio de las velocidades de los vecinos y se ajusta a la dirección del boid para alinearse con ellos. 

> **Regla 3: cohesión**
> * objetivo: mantener al grupo unido. 
> * logica: se calcula el centro de masa de los vecinos y se genera un vector que atrae al boid hacia ese centro. 

**2. Parámetros clave**

> * Pesos de reglas: son multiplicadores que ajustan a influencia de cada regla.
> * maxfoce: es la fuerza máxima de dirección que se puede aplicar a un boid.
> * maxspeed: es la velocidad máxima que puede alcanzar un boid. Controla que tan rápido va.
> * PerceptionRadius: lo agrego porque es el radio dentro del cual el boid detecta a sus vecinos para poder aplicar las reglas de arriba.
> * seekForce: peso de la nueva fuerza de seguimiento del objetivo

**3. Modificaciones**

**Cohesión, y alineación = 0**
> puse el peso de cohesión y alineación a cero para que esta regla no tenga efecto.

**efecto observado**
> * El enjambre se dispersa con el tiempo.
> * Se ven grupitos sueltos que no terminan de reunirse.
> * El movimiento global es menos cohesivo, mostrando que la alineación puede crear hileras que se rompen rápido.

<img width="607" height="463" alt="image" src="https://github.com/user-attachments/assets/408875cb-9f61-44e8-836b-2c34c393f492" />

<img width="610" height="456" alt="image" src="https://github.com/user-attachments/assets/3419cbd6-0b1f-4417-8e79-56255fa77b05" />

<img width="612" height="465" alt="image" src="https://github.com/user-attachments/assets/362d8ffd-7b44-47d3-b527-92a76c955966" />


### Actividad 05 apply

Decisiones de diseño: 
* En principio, la obra estaba inspirada en la escena de las turbinas de tesla del aprendiz del brujo. Quería mostrar rayos, y que estos se movieran al ritmo de la música, y/o violines, lastimosamente no obtuve los resulados que deseaba. 

Propuesta 1: 

Esta propuesta es un flow field generado con ruido perlín, las particulas (son los agentes), se mueven siguiendo ese viento, posteriormente cuando se ingresa la música, estan son modificadas de la siguiente forma, según la música: 
* Bajos / graves (bass, cellos, kick): hacen girar y empujar el campo de flujo como unas turbinas invisibles, y generan fuerza de vórtices y golpes radiales.

* Violines / agudos: producen vibraciones rápidas (vibrato) en el movimiento de las partículas, y también disparan rayos eléctricos fríos (azules/violetas). No son directamente los rayos que yo queria, pero ahi se medio perciben. 

* Batería / percusión (onsets): generan pequeños flashes de luz blanca y ondas de choque que expanden partículas, también cambian de color global las partículas (monocromatico a multicolor).

* Voces / coros:modulan el brillo de las estelas, si hay mas voz, se supone que hay mas luminocidad.
  
* Pads / teclados:suavizan el fondo y hacen que el campo fluya más lentamente.

[Click acá](https://editor.p5js.org/saragaravitop/sketches/zo1sqFOBw)

código 

```js
let app;

// ---------- p5 lifecycle ----------
function setup(){
  createCanvas(window.innerWidth, window.innerHeight);
  pixelDensity(1);
  colorMode(HSB, 360, 100, 100, 100);
  app = new App();
  app.initUI();
  background(230,20,4);
}
function draw(){ app.update(); app.render(); }
function windowResized(){ resizeCanvas(window.innerWidth, window.innerHeight); app.resize(); }

// ---------- Utils ----------
const clamp = (x,a,b)=>Math.max(a,Math.min(b,x));
const mapExp = (x,inMin,inMax,outMin,outMax,exp=1)=>{
  const t=clamp((x-inMin)/(inMax-inMin),0,1); return outMin+(outMax-outMin)*Math.pow(t,exp);
};

// ---------- Audio ----------
class AudioEngine{
  constructor(){
    this.fft = new p5.FFT(0.9, 1024);
    this.amp = new p5.Amplitude();
    this.song = null;
    // promedios móviles para pads/voz
    this.padMA = 0; this.voiceMA = 0; this.energyPrev = 0; this.onset = 0;
  }
  load(file){
    if(this.song){ this.song.stop(); this.song.disconnect(); }
    this.song = loadSound(URL.createObjectURL(file), ()=>{ this.song.loop(); updateStatus(); }, e=>console.error(e));
  }
  toggle(){ if(this.song){ this.song.isPlaying()?this.song.pause():this.song.play(); updateStatus(); } }
  features(){
    const bass    = this.fft.getEnergy('bass');            // 20–140
    const lowMid  = this.fft.getEnergy('lowMid');          // 140–400 (cellos)
    const mid     = this.fft.getEnergy('mid');             // 400–2600
    const treb    = this.fft.getEnergy('treble');          // 2600–16000
    const violin  = this.fft.getEnergy(1000, 4000);        // 1–4k (violines)
    const pads    = this.fft.getEnergy(200, 800);          // pads/teclados
    const voice   = this.fft.getEnergy(500, 2000);         // voz/coros
    const level   = this.amp.getLevel();

    const bassN   = clamp(map(bass,   40,220, 0,1),0,1);
    const lowMidN = clamp(map(lowMid, 30,200, 0,1),0,1);
    const midN    = clamp(map(mid,    30,200, 0,1),0,1);
    const trebN   = clamp(map(treb,   30,180, 0,1),0,1);
    const violinN = clamp(map(violin, 25,200, 0,1),0,1);
    const padsN   = clamp(map(pads,   20,180, 0,1),0,1);
    const voiceN  = clamp(map(voice,  20,180, 0,1),0,1);
    const rmsN    = clamp(map(level,0.02,0.25,0,1),0,1);

    // Suavizados
    this.padMA   = lerp(this.padMA,   padsN,  0.05);
    this.voiceMA = lerp(this.voiceMA, voiceN, 0.12);

    // Onset por derivada de energía global (batería/percusiones)
    const energy = (bassN*1.2 + midN + trebN*1.1)/3.3;
    const dE = energy - this.energyPrev;
    this.onset = Math.max(0, this.onset*0.92 + (dE>0.12 ? dE*1.8 : 0));
    this.energyPrev = energy;

    return { bassN, lowMidN, midN, trebN, violinN, padsN:this.padMA, voiceN:this.voiceMA, rmsN, onset:this.onset };
  }
}

// ---------- Flow Field ----------
class FlowField{
  constructor(scl=24, inc=0.008){ this.scl=scl; this.inc=inc; this.zoff=0; this.cols=0; this.rows=0; this.field=[]; this.resize(); }
  resize(){ this.cols=floor(width/this.scl); this.rows=floor(height/this.scl); this.field=new Array(this.cols*this.rows); }
  update(spin=0, mag=1, zspeed=0.003){
    this.zoff += zspeed;
    let i=0;
    for(let y=0;y<this.rows;y++){
      for(let x=0;x<this.cols;x++){
        const angle = noise(x*this.inc, y*this.inc, this.zoff)*TWO_PI*4 + spin;
        const v = p5.Vector.fromAngle(angle); v.setMag(mag); this.field[i++]=v;
      }
    }
  }
  lookup(pos){
    const x=floor(clamp(pos.x/this.scl,0,this.cols-1));
    const y=floor(clamp(pos.y/this.scl,0,this.rows-1));
    return this.field[x+y*this.cols] || createVector(0,0);
  }
}

// ---------- Agents (partículas) ----------
class Agent{
  constructor(){
    this.pos=createVector(random(width), random(height));
    this.prev=this.pos.copy();
    this.vel=p5.Vector.random2D().mult(random(0.3,1.2));
    this.acc=createVector(0,0);
    this.w=random(0.6,1.4);
    this.h=210; // azul por defecto
  }
  applyForce(f){ this.acc.add(f); }
  update(friction){ this.vel.add(this.acc); this.vel.mult(friction); this.pos.add(this.vel); this.acc.mult(0); }
  follow(field){ this.applyForce(field.lookup(this.pos)); }
  edges(){
    if(this.pos.x>width){ this.pos.x=0; this.prev=this.pos.copy(); }
    if(this.pos.x<0){ this.pos.x=width; this.prev=this.pos.copy(); }
    if(this.pos.y>height){ this.pos.y=0; this.prev=this.pos.copy(); }
    if(this.pos.y<0){ this.pos.y=height; this.prev=this.pos.copy(); }
  }
  show(alphaBoost){
    stroke(this.h, 80, 60+alphaBoost, 46);
    strokeWeight(this.w);
    line(this.pos.x,this.pos.y,this.prev.x,this.prev.y);
    this.prev=this.pos.copy();
  }
}

// ---------- Invisible Energy Nodes (turbinas) ----------
class EnergyNode{
  constructor(x,y){ this.pos=createVector(x,y); this.str=0; this.spin=random([-1,1])*random(0.015,0.05); }
  // fuerza en vórtice (rotacional) + onda expansiva
  vortexForce(p, amount){
    const dir = p5.Vector.sub(p.pos, this.pos);
    const d = Math.max(12, dir.mag());
    // campo rotacional ~ 1/r
    const tang = createVector(-dir.y, dir.x).setMag(amount / d);
    return tang;
  }
  shockForce(p, strength){ // empuje radial (kick)
    const dir = p5.Vector.sub(p.pos, this.pos);
    const d = Math.max(10, dir.mag());
    return dir.setMag(strength / d);
  }
}

// ---------- Lightning ----------
class LightningSystem{
  constructor(){ this.bolts=[]; this.additive=false; this.hue=220; }
  burst(a,b,chaos=16,branches=0,life=16){ this.bolts.push(new Bolt(a,b,chaos,branches,life,this.hue)); }
  update(){ for(let i=this.bolts.length-1;i>=0;i--){ this.bolts[i].update(); if(this.bolts[i].dead()) this.bolts.splice(i,1); } }
  draw(){ if(this.additive) blendMode(ADD); else blendMode(BLEND); for(const b of this.bolts) b.draw(); blendMode(BLEND); }
}
class Bolt{
  constructor(from,to,chaos,branches,life,hue){ this.from=from.copy(); this.to=to.copy(); this.chaos=chaos; this.branches=branches; this.life=life; this.hue=hue; }
  update(){ this.life--; }
  dead(){ return this.life<=0; }
  draw(){
    const x1=this.from.x,y1=this.from.y,x2=this.to.x,y2=this.to.y;
    const dx=x2-x1, dy=y2-y1, d=Math.hypot(dx,dy); const segs=max(8,floor(d/18)); let px=x1,py=y1;
    for(let i=1;i<=segs;i++){
      const t=i/segs; const bx=lerp(x1,x2,t), by=lerp(y1,y2,t); const nx=-dy/d, ny=dx/d;
      const off=(noise(t*10, frameCount*0.02)-0.5)*this.chaos; const cx=bx+nx*off, cy=by+ny*off;
      stroke(this.hue,40,100,26); strokeWeight(5); line(px,py,cx,cy);   // halo
      stroke(this.hue,70,100,92); strokeWeight(2); line(px,py,cx,cy);   // núcleo
      if(this.branches>0 && random()<0.12){
        const bx2=cx+random(-40,40), by2=cy+random(-40,40);
        const b=new Bolt(createVector(cx,cy), createVector(bx2,by2), this.chaos*0.6, this.branches-1, max(6,this.life-4), this.hue);
        b.draw();
      }
      px=cx; py=cy;
    }
  }
}

// ---------- App ----------
class App{
  constructor(){
    this.audio=new AudioEngine();
    this.field=new FlowField(24,0.008);

    // agentes
    this.agents=[]; this.agentCount=3400; this.friction=0.965;
    for(let i=0;i<this.agentCount;i++) this.agents.push(new Agent());

    // nodos energéticos invisibles
    this.nodes=[];
    const n = 8; // cuántos
    for(let i=0;i<n;i++){
      this.nodes.push(new EnergyNode(random(width*0.1,width*0.9), random(height*0.1,height*0.9)));
    }

    this.lightning=new LightningSystem();
    this.monochromeHue = 210; // azul cuando baja
    this.multicolor=false;
    this.flash=0; // para flashes de batería
  }

  initUI(){
    const file=document.getElementById('file');
    document.getElementById('btnLoad').onclick=()=>file.click();
    file.onchange=e=>{ const f=e.target.files?.[0]; if(f) this.audio.load(f); };
    document.getElementById('btnPlay').onclick=()=>this.audio.toggle();
    updateStatus();
  }

  resize(){ this.field.resize(); background(230,20,4); }

  spawnLightningHue(h){
    const a=createVector(random(width*0.15,width*0.85), random(height*0.15,height*0.85));
    const b=createVector(random(width*0.15,width*0.85), random(height*0.15,height*0.85));
    this.lightning.hue=h;
    this.lightning.burst(a,b, 14+random(16), floor(random(0,2)), 14+floor(random(8)));
  }

  update(){
    const F=this.audio.features();

    // Fondo/pads: si hay pads altos → fade más suave y campo más lento
    const padSoft = F.padsN;                       // 0..1
    const fade = mapExp(1-F.rmsN,0,1, 2, 10, 1.15) * (0.7 + 0.6*padSoft);
    noStroke(); fill(230,20,4, fade); rect(0,0,width,height);

    // Color global: multicolor cuando está “a tope”
    this.multicolor = F.rmsN>0.55 || F.onset>0.4;

    // Flow field: giro con graves, magnitud con medios; zspeed más lento si pads altos
    const spin = (F.bassN+F.lowMidN)*0.45 + F.rmsN*0.3;
    const mag  = 0.55 + F.midN*0.7;
    const zspd = (0.003 + F.rmsN*0.02) * (0.6 + 0.4*(1-padSoft));
    this.field.update(spin,mag,zspd);

    // Nodos energéticos: vórtices alimentados por graves/cellos
    const vortexAmt = 0.9*(F.bassN*0.7 + F.lowMidN*0.6); // fuerza base
    const shockAmt  = F.onset>0.35 ? 3.0*F.onset : 0;    // golpes de batería → shock
    for(const node of this.nodes){
      node.str = lerp(node.str, vortexAmt, 0.1);
    }

    // Agents: violines “agitan” (vibrato); voces pulsan brillo/tamaño
    const vibrato = F.violinN * 0.6;
    const violinPush = 0.25 + F.violinN*1.2;
    const voicePulse = 0.5 + F.voiceN*0.6; // alpha/size

    for(const a of this.agents){
      // fuerza del campo
      const f = this.field.lookup(a.pos).copy();
      // vibrato direccional (violín)
      const ang = (noise(a.pos.x*0.01, a.pos.y*0.01, frameCount*0.03)-0.5) * vibrato;
      f.rotate(ang);
      f.mult(violinPush);

      // nodos → vórtices y shocks (invisibles)
      for(const node of this.nodes){
        const vf = node.vortexForce(a, node.str*1.8);
        f.add(vf);
        if(shockAmt>0){ f.add(node.shockForce(a, shockAmt)); }
      }

      a.applyForce(f);
      a.update(this.friction);
      a.edges();

      // color de partículas
      if(this.multicolor){
        a.h = lerp(180, 330, clamp((F.violinN*0.6 + F.trebN*0.4), 0, 1));
      }else{
        a.h = this.monochromeHue;
      }

      a.show( (voicePulse-0.5)*18 ); // voces → brillo leve
    }

    // Rayos:
    // Violines/Agudos → rayos fríos; Graves → rayos cálidos
    if((F.violinN>0.45 || F.trebN>0.55) && frameCount%5===0){
      this.spawnLightningHue(230); // azul/violeta
    }
    if((F.bassN>0.6 || F.lowMidN>0.6) && frameCount%6===0){
      this.spawnLightningHue(15);  // rojo/anaranjado
    }

    // Flashes por batería/onset
    this.flash = Math.max(this.flash*0.92, F.onset>0.35 ? F.onset : 0);
    if(this.flash>0.01){
      blendMode(ADD);
      noStroke(); fill(0,0,100, this.flash*12); rect(0,0,width,height);
      blendMode(BLEND);
    }

    // Bloom aditivo cuando hay picos
    this.lightning.additive = this.multicolor || F.trebN>0.65 || F.onset>0.45;

    this.lightning.update();
  }

  render(){
    // dibuja rayos sobre las partículas
    this.lightning.draw();
    // marco sutil
    noFill(); stroke(220,10,80,10); rect(8,8,width-16,height-16,18);
  }
}

// ---------- UI helper ----------
function updateStatus(){
  const el=document.getElementById('status');
  if(!app || !app.audio.song){ el.textContent='Sin audio'; return; }
  el.textContent = app.audio.song.isPlaying()? 'Reproduciendo' : 'Pausado';
}
```

**capturas de pantalla**

<img width="607" height="524" alt="image" src="https://github.com/user-attachments/assets/14743532-0acc-4d33-8c72-497db479402c" />

**Propuesta 2:**

Después, quise que en verdad se vieran los rayos con los violines, entonces hice algunos cambios, estos son: 

* las partículas, siguen un flujo invisible que vibra con la voz, cambian de color con la batería y brillan más cuando hay coros o voces.

* los rayos nacen cuando entran violines o cellos.

* La batería también decide el color global de las particulas, y creo que tambien de los rayos.

**Código**

```js
const CFG = {
  agentCount: 3600,
  fieldScl: 24,
  fieldInc: 0.008,
  friction: 0.965,

  // color
  baseHue: 210,          // azul cuando no hay mucha percusión
  drumHueMin: 200,       // inicio de recorrido con batería
  drumHueMax: 25,        // destino (más cálido)
  multicolorSat: 85,     // saturación en modo multicolor
  monoSat: 80,

  // rayos
  violinChaosBase: 18,
  celloChaosBase: 10,
  violinBranches: 1,
  celloBranches: 2,
  boltLife: 20,

  // umbrales
  thr: {
    violin: 0.46,  // violines/violín
    cello:  0.46,  // cellos/registro bajo de cuerdas
    violinFlux: 0.12,
    celloFlux:  0.10,
    drumsFlux:  0.12
  }
};

// ================== LIFECYCLE ==================
let app;
function setup(){
  createCanvas(window.innerWidth, window.innerHeight);
  pixelDensity(1);
  colorMode(HSB, 360, 100, 100, 100);
  app = new App();
  app.initUI();
  background(230,20,4);
}
function draw(){ app.update(); app.render(); }
function windowResized(){ resizeCanvas(window.innerWidth, window.innerHeight); app.resize(); }

// ================== UTILS ==================
const clamp = (x,a,b)=>Math.max(a,Math.min(b,x));
const mapExp = (x,inMin,inMax,outMin,outMax,exp=1)=>{
  const t = clamp((x-inMin)/(inMax-inMin),0,1);
  return outMin + (outMax-outMin)*Math.pow(t,exp);
};

// ================== AUDIO ==================
class AudioEngine{
  constructor(){
    this.fft = new p5.FFT(0.9, 1024);
    this.amp = new p5.Amplitude();
    this.song = null;

    // para spectral flux (cambios súbitos)
    this.prevSpec = new Array(1024).fill(0);
    this.smoothed = {
      voice: 0, pads: 0, violin: 0, cello: 0,
      kick: 0, snare: 0, hats: 0, drums: 0
    };
  }
  load(file){
    if(this.song){ this.song.stop(); this.song.disconnect(); }
    this.song = loadSound(URL.createObjectURL(file), ()=>{ this.song.loop(); updateStatus(); }, e=>console.error(e));
  }
  toggle(){ if(this.song){ this.song.isPlaying()?this.song.pause():this.song.play(); updateStatus(); } }

  // Helpers para bandas
  band(minHz, maxHz){ return clamp(this.fft.getEnergy(minHz, maxHz), 0, 255); }
  norm(v, a, b){ return clamp(map(v, a, b, 0, 1), 0, 1); }

  features(){
    const spectrum = this.fft.analyze();

    // bandas
    const voiceRaw  = this.band(300, 3000);     // voz/coros
    const violinRaw = this.band(1000, 4000);    // violines
    const celloRaw  = this.band(100, 300);      // cellos/graves de cuerdas
    const padsRaw   = this.band(200, 800);      // pads/teclados

    const kickRaw   = this.band(40, 100);
    const snareRaw  = this.band(150, 250);
    const hatsRaw   = this.band(6000, 12000);

    // normalizaciones empíricas
    const voiceN  = this.norm(voiceRaw, 20, 190);
    const violinN = this.norm(violinRaw, 25, 200);
    const celloN  = this.norm(celloRaw,  20, 180);
    const padsN   = this.norm(padsRaw,   20, 160);

    const kickN   = this.norm(kickRaw,   30, 200);
    const snareN  = this.norm(snareRaw,  25, 200);
    const hatsN   = this.norm(hatsRaw,   20, 180);

    // suavizados (MA simple)
    this.smoothed.voice  = lerp(this.smoothed.voice,  voiceN,  0.15);
    this.smoothed.violin = lerp(this.smoothed.violin, violinN, 0.12);
    this.smoothed.cello  = lerp(this.smoothed.cello,  celloN,  0.12);
    this.smoothed.pads   = lerp(this.smoothed.pads,   padsN,   0.08);
    this.smoothed.kick   = lerp(this.smoothed.kick,   kickN,   0.18);
    this.smoothed.snare  = lerp(this.smoothed.snare,  snareN,  0.18);
    this.smoothed.hats   = lerp(this.smoothed.hats,   hatsN,   0.18);

    // drums aggregate
    const drumsN = clamp((kickN*1.2 + snareN + hatsN*0.9)/3.1, 0, 1);
    this.smoothed.drums = lerp(this.smoothed.drums, drumsN, 0.18);

    // spectral flux por subbandas (strings y drums)
    const flux = (lo, hi) => {
      // aproximar sub-índices por frecuencia: fft bins ~ lineal en p5
      const binW = 22050 / spectrum.length;
      const i0 = Math.max(0, Math.floor(lo/binW));
      const i1 = Math.min(spectrum.length-1, Math.floor(hi/binW));
      let sum=0, sumPrev=0;
      for(let i=i0;i<=i1;i++){
        const cur = spectrum[i];
        const prev = this.prevSpec[i];
        const diff = Math.max(0, cur - prev);
        sum += diff; sumPrev += prev;
      }
      return sum / Math.max(1, (i1-i0+1)*255);
    };

    const violinFlux = flux(1000, 4000);
    const celloFlux  = flux(100,  300);
    const drumsFlux  = (flux(40,110)+flux(140,260)+flux(6000,12000))/3;

    // guarda para próxima
    this.prevSpec = spectrum.slice();

    // nivel global
    const level = this.amp.getLevel();
    const rmsN = this.norm(level, 0.02, 0.25);

    return {
      voiceN:  this.smoothed.voice,    // voz/coros
      padsN:   this.smoothed.pads,     // teclados/pads
      violinN: this.smoothed.violin,   // violines
      celloN:  this.smoothed.cello,    // cellos
      kickN:   this.smoothed.kick,
      snareN:  this.smoothed.snare,
      hatsN:   this.smoothed.hats,
      drumsN:  this.smoothed.drums,    // batería agregada
      violinFlux, celloFlux, drumsFlux,
      rmsN
    };
  }
}

// ================== FLOW FIELD ==================
class FlowField{
  constructor(scl=CFG.fieldScl, inc=CFG.fieldInc){
    this.scl=scl; this.inc=inc; this.zoff=0;
    this.cols=0; this.rows=0; this.field=[];
    this.resize();
  }
  resize(){ this.cols=floor(width/this.scl); this.rows=floor(height/this.scl); this.field=new Array(this.cols*this.rows); }
  update(spin=0, mag=1, zspeed=0.003){
    this.zoff += zspeed;
    let i=0;
    for(let y=0;y<this.rows;y++){
      for(let x=0;x<this.cols;x++){
        const angle = noise(x*this.inc, y*this.inc, this.zoff) * TWO_PI * 4 + spin;
        const v = p5.Vector.fromAngle(angle);
        v.setMag(mag);
        this.field[i++] = v;
      }
    }
  }
  lookup(pos){
    const x=floor(clamp(pos.x/this.scl,0,this.cols-1));
    const y=floor(clamp(pos.y/this.scl,0,this.rows-1));
    return this.field[x+y*this.cols] || createVector(0,0);
  }
}

// ================== AGENTS ==================
class Agent{
  constructor(id){
    this.id=id;
    this.pos=createVector(random(width), random(height));
    this.prev=this.pos.copy();
    this.vel=p5.Vector.random2D().mult(random(0.2,1));
    this.acc=createVector(0,0);
    this.w=random(0.6,1.5);
    this.h=CFG.baseHue; // se reasigna por drums
    this.phi=random(TWO_PI); // fase para vibrato
  }
  applyForce(f){ this.acc.add(f); }
  update(friction){ this.vel.add(this.acc); this.vel.mult(friction); this.pos.add(this.vel); this.acc.mult(0); }
  edges(){
    if(this.pos.x>width){ this.pos.x=0; this.prev=this.pos.copy(); }
    if(this.pos.x<0){ this.pos.x=width; this.prev=this.pos.copy(); }
    if(this.pos.y>height){ this.pos.y=0; this.prev=this.pos.copy(); }
    if(this.pos.y<0){ this.pos.y=height; this.prev=this.pos.copy(); }
  }
  show(alphaBoost){
    stroke(this.h, this.sat||CFG.monoSat, 60+alphaBoost, 50);
    strokeWeight(this.w);
    line(this.pos.x,this.pos.y,this.prev.x,this.prev.y);
    this.prev=this.pos.copy();
  }
}

// ================== RAYOS ==================
class LightningSystem{
  constructor(){ this.bolts=[]; this.additive=false; }
  burst(a,b,opts){
    const { chaos=16, branches=0, life=18, hue=220, halo=26, core=92 } = opts||{};
    this.bolts.push(new Bolt(a,b,chaos,branches,life,hue,halo,core));
  }
  update(){ for(let i=this.bolts.length-1;i>=0;i--){ this.bolts[i].update(); if(this.bolts[i].dead()) this.bolts.splice(i,1); } }
  draw(){ if(this.additive) blendMode(ADD); else blendMode(BLEND); for(const b of this.bolts) b.draw(); blendMode(BLEND); }
}
class Bolt{
  constructor(from,to,chaos,branches,life,hue,haloAlpha,coreAlpha){
    this.from=from.copy(); this.to=to.copy();
    this.chaos=chaos; this.branches=branches; this.life=life;
    this.hue=hue; this.haloAlpha=haloAlpha; this.coreAlpha=coreAlpha;
  }
  update(){ this.life--; }
  dead(){ return this.life<=0; }
  draw(){
    const x1=this.from.x, y1=this.from.y, x2=this.to.x, y2=this.to.y;
    const dx=x2-x1, dy=y2-y1, d=max(1,Math.hypot(dx,dy));
    const segs=max(8, floor(d/16));
    let px=x1, py=y1;
    for(let i=1;i<=segs;i++){
      const t=i/segs;
      const bx=lerp(x1,x2,t), by=lerp(y1,y2,t);
      const nx=-dy/d, ny=dx/d;
      const off=(noise(t*10, frameCount*0.025)-0.5)*this.chaos;
      const cx=bx+nx*off, cy=by+ny*off;
      // halo
      stroke(this.hue, 40, 100, this.haloAlpha); strokeWeight(5); line(px,py,cx,cy);
      // núcleo
      stroke(this.hue, 70, 100, this.coreAlpha); strokeWeight(2.2); line(px,py,cx,cy);
      if(this.branches>0 && random()<0.12){
        const bx2=cx+random(-40,40), by2=cy+random(-40,40);
        const b=new Bolt(createVector(cx,cy), createVector(bx2,by2), this.chaos*0.6, this.branches-1, max(6,this.life-4), this.hue, this.haloAlpha*0.8, this.coreAlpha);
        b.draw();
      }
      px=cx; py=cy;
    }
  }
}

// ================== APP ==================
class App{
  constructor(){
    this.audio=new AudioEngine();
    this.field=new FlowField();

    // agentes
    this.agents=[];
    for(let i=0;i<CFG.agentCount;i++) this.agents.push(new Agent(i));

    // puntos guía invisibles para rayos (constelación base)
    this.anchorPts = this.makeAnchorPoints();

    this.lightning=new LightningSystem();

    // estado de color/ambiente
    this.drumHue = CFG.baseHue;  // evoluciona con batería
    this.multicolor = false;
  }

  initUI(){
    const file=document.getElementById('file');
    document.getElementById('btnLoad').onclick=()=>file.click();
    file.onchange=e=>{ const f=e.target.files?.[0]; if(f) this.audio.load(f); };
    document.getElementById('btnPlay').onclick=()=>this.audio.toggle();
    updateStatus();
  }

  resize(){ this.field.resize(); this.anchorPts=this.makeAnchorPoints(); background(230,20,4); }

  makeAnchorPoints(){
    // puntos invisibles (espiral + anillos) para que los rayos tengan estructura
    const pts=[];
    const cx=width/2, cy=height/2;
    // espiral
    let r=18, dr=min(width,height)*0.002;
    for(let i=0;i<360;i++){
      const a=i*0.28;
      pts.push(createVector(cx+cos(a)*r, cy+sin(a)*r));
      r+=dr;
    }
    // anillos
    for(let k=1;k<=4;k++){
      const R=min(width,height)*(0.12+0.08*k);
      const N=80;
      for(let i=0;i<N;i++){
        const a=i/N*TWO_PI + k*0.17;
        pts.push(createVector(cx+cos(a)*R, cy+sin(a)*R));
      }
    }
    return pts;
  }

  spawnBoltFromAnchors(hue, chaos, branches, lifeMul=1){
    const a = random(this.anchorPts);
    const b = random(this.anchorPts);
    this.lightning.burst(a,b,{ chaos, branches, life: floor(CFG.boltLife*lifeMul), hue });
  }

  update(){
    const F = this.audio.features();

    // --------- FONDO (pads → más sedoso, menos barrido) ----------
    const fade = mapExp(1-F.rmsN,0,1, 2, 10, 1.15) * (0.8 + 0.4*F.padsN);
    noStroke(); fill(230,20,4, fade); rect(0,0,width,height);

    // --------- FLOW FIELD ----------
    const spin = (F.voiceN*0.2) + (F.celloN*0.5);       // algo de voz + cello dan giro
    const mag  = 0.55 + F.voiceN*0.9;                   // voz empuja el flujo
    const zspd = (0.003 + F.rmsN*0.02) * (0.6 + 0.4*(1-F.padsN));
    this.field.update(spin, mag, zspd);

    // --------- COLOR por DRUMS ----------
    // drumsFlux detecta "batería completa"; drumsN el nivel base
    const drumsActive = F.drumsN*0.7 + F.drumsFlux*0.8;
    const hueTarget = lerp(CFG.drumHueMin, CFG.drumHueMax, clamp(drumsActive,0,1));
    this.drumHue = lerp(this.drumHue, hueTarget, 0.12);
    this.multicolor = (F.drumsFlux > CFG.thr.drumsFlux || F.drumsN > 0.55);

    // --------- AGENTS (VOZ) ----------
    // campo vocal: atractor elíptico que se mueve con la voz
    const cx=width/2, cy=height/2;
    const A=min(width,height)*0.28*(0.7+0.6*F.voiceN);
    const B=A*0.6;
    const t=frameCount*0.01*(0.6+0.7*F.voiceN);
    const vocalCenter = createVector(cx + A*Math.sin(2*t), cy + B*Math.sin(3*t+PI/3));

    const vibratoBase = F.voiceN * 0.6;        // oscilación angular
    const pushBase    = 0.25 + F.voiceN*1.3;   // ganancia

    for(const p of this.agents){
      // fuerza del flow field
      const f = this.field.lookup(p.pos).copy();

      // vibrato direccional con fase propia
      p.phi += 0.06 + 0.12*F.voiceN;
      const jitter = (noise(p.id*0.1, p.phi)-0.5) * vibratoBase;
      f.rotate(jitter);

      // atracción suave hacia el "centro vocal"
      const toVC = p5.Vector.sub(vocalCenter, p.pos);
      const dist = Math.max(24, toVC.mag());
      toVC.setMag( (0.4*F.voiceN) / dist );
      f.add(toVC);

      // potencia final
      f.mult(pushBase);

      p.applyForce(f);
      p.update(CFG.friction);
      p.edges();

      // COLOR de partículas
      if(this.multicolor){
        // Recorrido de paleta con drums; saturación alta
        p.h = this.drumHue;
        p.sat = CFG.multicolorSat;
      }else{
        p.h = CFG.baseHue;
        p.sat = CFG.monoSat;
      }

      // brillo con voz
      const alphaBoost = (0.4 + 0.6*F.voiceN) * 20;
      p.show(alphaBoost);
    }

    // --------- RAYOS por CUERDAS ----------
    // Violín → azul/violeta, caos alto, ramas pocas; cello → cálido, menos caos, más ramas.
    const violinHit = (F.violinN > CFG.thr.violin) || (F.violinFlux > CFG.thr.violinFlux);
    const celloHit  = (F.celloN  > CFG.thr.cello)  || (F.celloFlux  > CFG.thr.celloFlux);

    // ritmo de disparo controlado por frameCount para no saturar
    if(violinHit && frameCount%4===0){
      this.spawnBoltFromAnchors(
        230,                                            // hue frío
        CFG.violinChaosBase + F.violinN*22,             // más caos con energía
        CFG.violinBranches,
        1 + F.violinN*0.6
      );
    }
    if(celloHit && frameCount%6===0){
      this.spawnBoltFromAnchors(
        20,                                             // hue cálido
        CFG.celloChaosBase + F.celloN*10,
        CFG.celloBranches,
        1.1 + F.celloN*0.7
      );
    }

    // modo aditivo cuando hay mucha cuerda o picos globales
    this.lightning.additive = violinHit || celloHit || F.rmsN>0.6;

    this.lightning.update();
  }

  render(){
    // Rayos encima para que destaquen
    this.lightning.draw();
    // Marco sutil
    noFill(); stroke(220,10,80,10); rect(8,8,width-16,height-16,18);
  }
}

// ================== UI helper ==================
function updateStatus(){
  const el=document.getElementById('status');
  if(!app || !app.audio.song){ el.textContent='Sin audio'; return; }
  el.textContent = app.audio.song.isPlaying()? 'Reproduciendo' : 'Pausado';
}
```
[Click acá](https://editor.p5js.org/saragaravitop/sketches/EvKR4FLdc)

**Captura**

<img width="607" height="522" alt="image" src="https://github.com/user-attachments/assets/def92a58-672d-4159-a04c-39527b67c1bc" />

**Propuesta "final":**

Estos fueron los cambios: 
* Partículas: siguen un campo de flujo dinámico, la voz hace que se muevan segun el vibrato y brillen más, la batería les da más empuje, como si todo el campo se acelerara en conjunto, y el color pasa de frío (azul) a cálido (naranja) cuando la percusión sube en intensidad.

* Circulos: los quise agregar para dar mas variedad visual, siguen a la voz principal, el círculo se expande y contrae con la voz, y el color cambia según la presencia de pads (teclados), pasando a tonos más suaves o más saturados.

* Noise: Está vinculada a los pads/teclados, cuanto más suaves, más densa se vuelve la atmósfera, como una bruma envolvente.
  
* Líneas: se disparan con cambios súbitos de energía (detonados por la batería o transiciones fuertes), y funcionan como flashes de luz que aparecen en coros y clímax para dar dramatismo.

* Batería: sigue dirigiendo el color global.
  
[Click acá](https://editor.p5js.org/saragaravitop/sketches/D4DhGsHsR)

**Código**

```js
const CFG = {
  agentCount: 3000,
  fieldScl: 24,
  fieldInc: 0.008,
  friction: 0.965,

  baseHue: 210,        // azul frío base
  hueWarm: 30,         // cálido para coros/clímax

  // Paleta por escena (modulada)
  sceneSats: { A:60, B:70, C:75, D:90, E:55, F:95, G:45 },
  sceneBris: { A:60, B:68, C:72, D:90, E:60, F:95, G:50 },

  // Director
  minSceneTime: 5.0,   // seg mínimos por escena
  rmsHigh: 0.55,
  drumsHigh: 0.55,
  voiceHigh: 0.45,
  padsHigh: 0.45
};

// ================== GLOBALS ==================
let app;

// ================== LIFECYCLE ==================
function setup(){
  createCanvas(window.innerWidth, window.innerHeight);
  pixelDensity(1);
  colorMode(HSB, 360, 100, 100, 100);
  app = new App();
  app.initUI();
  background(230,20,4);
}
function draw(){ app.update(); app.render(); }
function windowResized(){ resizeCanvas(window.innerWidth, window.innerHeight); app.resize(); }

// ================== UTILS ==================
const clamp = (x,a,b)=>Math.max(a,Math.min(b,x));
const lerp01 = (a,b,t)=>a+(b-a)*t;

// ================== AUDIO ==================
class AudioEngine{
  constructor(){
    this.fft = new p5.FFT(0.9, 1024);
    this.amp = new p5.Amplitude();
    this.song = null;

    this.prevSpec = new Array(1024).fill(0);
    this.smoothed = { voice:0, pads:0, drums:0 };
    this.t = 0; // tiempo estimado (frames/60)
  }
  load(file){
    if(this.song){ this.song.stop(); this.song.disconnect(); }
    this.song = loadSound(URL.createObjectURL(file), ()=>{ this.song.loop(); updateStatus(); });
  }
  toggle(){ if(this.song){ this.song.isPlaying()?this.song.pause():this.song.play(); updateStatus(); } }

  band(minHz, maxHz){ return clamp(this.fft.getEnergy(minHz, maxHz), 0, 255); }
  norm(v,a,b){ return clamp(map(v,a,b,0,1),0,1); }

  features(){
    const spectrum = this.fft.analyze();

    // bandas
    const voiceRaw = this.band(300, 3000);
    const padsRaw  = this.band(200, 800);
    const kickRaw  = this.band(40, 100);
    const snareRaw = this.band(150, 250);
    const hatsRaw  = this.band(6000, 12000);

    const voiceN = this.norm(voiceRaw, 20, 190);
    const padsN  = this.norm(padsRaw,  20, 160);

    const drumsN = this.norm( (kickRaw*1.2 + snareRaw + hatsRaw*0.9)/3.1, 25, 200 );

    this.smoothed.voice = lerp(this.smoothed.voice, voiceN, 0.12);
    this.smoothed.pads  = lerp(this.smoothed.pads,  padsN,  0.08);
    this.smoothed.drums = lerp(this.smoothed.drums, drumsN, 0.18);

    // spectral flux general para flashes
    let fluxSum = 0, n = spectrum.length;
    for (let i=0;i<n;i++) fluxSum += Math.max(0, spectrum[i] - this.prevSpec[i]);
    const flux = fluxSum / (n * 255);
    this.prevSpec = spectrum.slice();

    const rmsN = this.norm(this.amp.getLevel(), 0.02, 0.25);

    // tiempo aproximado (si se está reproduciendo)
    if (this.song && this.song.isPlaying()) this.t += 1/60;

    return {
      voice: this.smoothed.voice,
      pads:  this.smoothed.pads,
      drums: this.smoothed.drums,
      rms:   rmsN,
      flux
    };
  }
}

// ================== FLOW FIELD ==================
class FlowField{
  constructor(scl=CFG.fieldScl, inc=CFG.fieldInc){
    this.scl=scl; this.inc=inc; this.zoff=0; this.resize();
  }
  resize(){
    this.cols=floor(width/this.scl);
    this.rows=floor(height/this.scl);
    this.field=new Array(this.cols*this.rows);
  }
  update(spin=0, mag=1, zspeed=0.003){
    this.zoff += zspeed;
    let i=0;
    for(let y=0;y<this.rows;y++){
      for(let x=0;x<this.cols;x++){
        const angle = noise(x*this.inc, y*this.inc, this.zoff)*TWO_PI*4 + spin;
        const v = p5.Vector.fromAngle(angle); v.setMag(mag);
        this.field[i++] = v;
      }
    }
  }
  lookup(pos){
    const x=floor(clamp(pos.x/this.scl,0,this.cols-1));
    const y=floor(clamp(pos.y/this.scl,0,this.rows-1));
    return this.field[x+y*this.cols] || createVector(0,0);
  }
}

// ================== LAYERS ==================
class ParticlesLayer{
  constructor(count){
    this.ps=[];
    for(let i=0;i<count;i++){
      this.ps.push({
        pos:createVector(random(width),random(height)),
        prev:null,
        vel:p5.Vector.random2D().mult(random(0.2,1)),
        acc:createVector(0,0),
        w:random(0.6,1.5),
        h:CFG.baseHue
      });
    }
  }
  update(field, voice, drums, friction){
    for(const p of this.ps){
      const f = field.lookup(p.pos).copy();
      // voz = empuje + vibrato
      const vibr = (noise(p.pos.x*0.01, p.pos.y*0.01, frameCount*0.03)-0.5) * voice*0.5;
      f.rotate(vibr);
      f.mult(0.5 + voice*1.3);

      // empuje sutil por batería
      f.mult(0.9 + drums*0.3);

      p.acc.add(f);
      p.vel.add(p.acc);
      p.vel.mult(friction);
      p.pos.add(p.vel);
      p.acc.mult(0);

      // wrap
      if(p.pos.x>width) p.pos.x=0;
      if(p.pos.x<0) p.pos.x=width;
      if(p.pos.y>height) p.pos.y=0;
      if(p.pos.y<0) p.pos.y=height;
    }
  }
  draw(hue, sat, bri, voice){
    strokeWeight(1);
    for(const p of this.ps){
      const prev = p.prev || p.pos.copy();
      stroke(hue, sat, bri + voice*20, 55);
      strokeWeight(p.w);
      line(p.pos.x, p.pos.y, prev.x, prev.y);
      p.prev = p.pos.copy();
    }
  }
}

class PulseRings{
  constructor(){ this.r=0; this.alpha=0; this.h=210; }
  update(voice, pads){
    this.r = lerp(this.r, map(voice,0,1,20, min(width,height)*0.4), 0.08);
    this.alpha = lerp(this.alpha, map(voice,0,1,10,60), 0.1);
    this.h = lerp(this.h, map(pads,0,1,200,260), 0.05);
  }
  draw(){
    noFill();
    stroke(this.h, 50, 90, this.alpha);
    strokeWeight(3);
    ellipse(width/2, height/2, this.r*2);
    stroke(this.h, 40, 80, this.alpha*0.6);
    strokeWeight(7);
    ellipse(width/2, height/2, this.r*2.35);
  }
}

class NoiseFog{
  constructor(){ this.z=0; }
  draw(intensity){
    // intensidad 0..1
    if(intensity<=0.02) return;
    this.z += 0.005 + intensity*0.015;
    noStroke();
    // cuadricula grande para textura ligera
    const step = 32;
    for(let y=0;y<height;y+=step){
      for(let x=0;x<width;x+=step){
        const n = noise(x*0.002, y*0.002, this.z);
        const a = (n*intensity)*10; // alpha muy bajo
        fill(220, 10, 80, a);
        rect(x,y,step,step);
      }
    }
  }
}

class ShockFlash{
  constructor(){ this.alpha=0; }
  trigger(level){ this.alpha = min(100, this.alpha + level*50); }
  update(){ this.alpha *= 0.88; }
  draw(){
    if(this.alpha<1) return;
    blendMode(ADD);
    noStroke(); fill(0,0,100,this.alpha);
    rect(0,0,width,height);
    blendMode(BLEND);
  }
}

class CameraFx{
  constructor(){ this.zoom=1; this.rot=0; }
  update(scene, voice, drums){
    // según escena, diferentes rangos
    const zBase = (scene==='D'||scene==='F') ? 1.06 : (scene==='C'?1.03:1.0);
    const rBase = (scene==='F') ? 0.02 : (scene==='D'?0.01:0.0);
    this.zoom = lerp(this.zoom, zBase + drums*0.05 + voice*0.03, 0.1);
    this.rot  = lerp(this.rot, rBase  + drums*0.02, 0.1);
  }
  begin(){
    push();
    translate(width/2, height/2);
    rotate(this.rot);
    scale(this.zoom);
    translate(-width/2, -height/2);
  }
  end(){ pop(); }
}

// ================== SCENE DIRECTOR ==================
class SceneDirector{
  constructor(){
    this.scene='A';
    this.lastChange=0;
  }
  update(F){
    const now = F.t ?? 0; // (no usamos acá, pero lo dejamos por si)
    const enoughTime = app.audio.t - this.lastChange > CFG.minSceneTime;

    // reglas simples con histéresis por energía
    switch(this.scene){
      case 'A': // Intro
        if(enoughTime && F.voice>0.25) this.set('B');
        break;
      case 'B': // Verso
        if(enoughTime && (F.pads>CFG.padsHigh || F.rms>0.45)) this.set('C');
        break;
      case 'C': // Pre-chorus (build)
        if(enoughTime && (F.drums>CFG.drumsHigh || F.rms>CFG.rmsHigh)) this.set('D');
        break;
      case 'D': // Coro
        if(enoughTime && (F.rms<0.4 && F.drms<0.5)) this.set('E');
        // si vuelve a subir mucho más tarde, pasará a F
        if(enoughTime && (F.rms>0.65 && F.drums>0.65)) this.set('F');
        break;
      case 'E': // Puente
        if(enoughTime && (F.rms>CFG.rmsHigh || F.drums>CFG.drumsHigh)) this.set('F');
        break;
      case 'F': // Clímax
        if(enoughTime && (F.rms<0.4 && F.drums<0.45)) this.set('G');
        break;
      case 'G': // Outro
        // final: se queda o termina
        break;
    }
  }
  set(s){ this.scene=s; this.lastChange = app.audio.t; }
}

// ================== APP ==================
class App{
  constructor(){
    this.audio = new AudioEngine();
    this.field = new FlowField();

    this.particles = new ParticlesLayer(CFG.agentCount);
    this.rings = new PulseRings();
    this.fog = new NoiseFog();
    this.flash = new ShockFlash();
    this.camera = new CameraFx();

    this.director = new SceneDirector();

    // estado de color
    this.hue = CFG.baseHue;
  }

  initUI(){
    const file=document.getElementById('file');
    document.getElementById('btnLoad').onclick=()=>file.click();
    file.onchange=e=>{ const f=e.target.files?.[0]; if(f) this.audio.load(f); };
    document.getElementById('btnPlay').onclick=()=>this.audio.toggle();
    updateStatus();
  }

  resize(){ this.field.resize(); background(230,20,4); }

  update(){
    const F0 = this.audio.features();
    // empaquetamos para el director
    const F = { voice:F0.voice, pads:F0.pads, drums:F0.drums, rms:F0.rms, flux:F0.flux, t:this.audio.t };

    // actualizar escena
    this.director.update(F);
    const scene = this.director.scene;

    // fondo según escena (respira con RMS)
    const baseFade = map(1-F.rms, 0,1, 2, 10);
    noStroke(); fill(230,20,4, baseFade); rect(0,0,width,height);

    // parámetros por escena
    let spin=0, mag=0.7, zspd=0.004;
    let targetHue = CFG.baseHue;
    let sat = CFG.sceneSats[scene] || 70;
    let bri = CFG.sceneBris[scene] || 70;
    let fogInt = 0.0;

    if(scene==='A'){ // Intro
      spin=0.0; mag=0.6+F.pads*0.2; zspd=0.003+F.pads*0.005; fogInt=0.05*F.pads;
    }
    if(scene==='B'){ // Verso
      spin=F.voice*0.2; mag=0.6+F.voice*0.5; zspd=0.004+F.pads*0.008; fogInt=0.1+0.15*F.pads;
    }
    if(scene==='C'){ // Build
      spin=F.voice*0.3+F.pads*0.2; mag=0.7+F.voice*0.7; zspd=0.005+F.pads*0.01; fogInt=0.2+0.25*F.pads;
    }
    if(scene==='D'){ // Coro
      spin=F.drums*0.4; mag=0.9+F.voice*0.8; zspd=0.006+F.rms*0.015; fogInt=0.25+0.25*F.pads;
      // color recorre hacia cálido con batería
      targetHue = lerp(CFG.baseHue, CFG.hueWarm, clamp(F.drums,0,1));
      // flashes por flux
      if(F.flux>0.08) this.flash.trigger(F.flux);
    }
    if(scene==='E'){ // Puente
      spin=F.pads*0.2; mag=0.6+F.voice*0.3; zspd=0.004; fogInt=0.3*F.pads;
      targetHue = CFG.baseHue;
    }
    if(scene==='F'){ // Clímax
      spin=F.drums*0.5; mag=1.0+F.voice*0.9; zspd=0.007+F.rms*0.02; fogInt=0.3+0.35*F.pads;
      targetHue = lerp(CFG.baseHue, CFG.hueWarm, clamp(F.drums*1.1,0,1));
      if(F.flux>0.06) this.flash.trigger(F.flux*1.2);
    }
    if(scene==='G'){ // Outro
      spin=0.05; mag=0.55+F.voice*0.2; zspd=0.003; fogInt=0.1*F.pads;
      targetHue = lerp(CFG.baseHue, CFG.hueWarm, 0.15);
      sat *= 0.8; bri *= 0.9;
    }

    // cámara
    this.camera.update(scene, F.voice, F.drums);
    this.camera.begin();

    // campo
    this.field.update(spin, mag, zspd);

    // partículas
    this.hue = lerp(this.hue, targetHue, 0.1);
    this.particles.update(this.field, F.voice, F.drums, CFG.friction);
    this.particles.draw(this.hue, sat, bri, F.voice);

    // anillos
    this.rings.update(F.voice, F.pads);
    this.rings.draw();

    // fog
    this.fog.draw(fogInt);

    this.camera.end();

    // flashes sobre todo
    this.flash.update();
    this.flash.draw();

    // borde sutil
    noFill(); stroke(220,10,80,10); rect(8,8,width-16,height-16,18);
  }

  render(){ /* todo el dibujo ya ocurre en update() */ }
}

// ================== UI helper ==================
function updateStatus(){
  const el=document.getElementById('status');
  if(!app || !app.audio.song){ el.textContent='Sin audio'; return; }
  el.textContent = app.audio.song.isPlaying()? 'Reproduciendo' : 'Pausado';
}
```

**Captura de pantalla**

<img width="672" height="525" alt="image" src="https://github.com/user-attachments/assets/ed4117a9-9bce-4138-8db3-a05db8af9ddb" />

**Evaluación del proceso**

Según la rubrica, mi nota sería 5 porque hice las 5 actividades completas y estoy haciendo la autoevaluación. 
Defendiendo mi nota, considero que hice un buen trabajo porque de verdad hice todo lo que mi mente podía para conseguir actividades que fueran increíbles, no solo con el apply, sino en general. Busqué empaparme del tema y diseñar algo, que según yo fuera a ser "wow", creo que tenia expectativas muy altas para lo que yo podia conseguir, pero al menos, creo que tengo una base sólida para poder desarrollar esta idea mas a profundidad. Aprendí mucho sobre los flow fields y me gustó mucho crear una obra de arte generativo inspirado en la música que mas me gusta. 
