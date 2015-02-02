# brickbreaker
how to create a brickbreaker game


Paddle miPaddle;
Pelota miPelota;
ArrayList ListaBricks;

void setup() {
  size(1024, 768);
  noCursor();
  miPaddle = new Paddle(height, 120, 30, 255); //pos x, ancho, alto, color
  //miPelota = new Pelota(400, 350, random(1, 4), random(1, 4), 50, 250);
  miPelota = new Pelota(500, 400, 4, 4, 25, 250);   //posx, posy, velocidadx, velocidady, diametro, color

  ListaBricks = new ArrayList();

  for (int x=1; x<=9; x++) { // column
    for (int y=1; y<=7; y++) {
      //aqui se define la nueva clase bricks
      ListaBricks.add(new Bricks (x*100+15, y*50+20, 90, 30)); //posX, posy, ancho,alto
    }
  }
}//salir setup


void draw() {
  background(0);

  miPelota.displayPelota(); //para que se vea la pelota
  miPelota.animarPelota(); //para que sirva la pelota
  
  
 miPaddle.paddleDisplay(); //para que se vea el paddle


    for (int i=ListaBricks.size ()-1; i>=0; i--) {
    //conseguir un solo brick (i)
    Bricks unBrick = (Bricks) ListaBricks.get(i);

    if (unBrick.colision(miPelota.posXpelota, miPelota.posYpelota, miPelota.radioPelota)) {
     
      miPelota.speedY = miPelota.speedY*-1;
      ListaBricks.remove(i);
    }//salir if
  } //salir for


  //para que vuelva a aparecer el array con ya el unBrick restado
  for (int i= ListaBricks.size ()-1; i>=0; i--) {
    Bricks unBrick = (Bricks) ListaBricks.get(i);
    unBrick.displayBrick();
  }//salir for


 
}//salir draw



//esto va en una nueva pestaña
//======================================================================================




class Bricks {

  int posX;
  int posY;
  int anchoBrick; //width
  int altoBrick; //hegith

  Bricks(int posX_, int posY_, int anchoBrick_, int altoBrick_) {
    posX= posX_;
    posY= posY_;
    anchoBrick = anchoBrick_;
    altoBrick = altoBrick_;
  }//salir constructor

  void displayBrick() {
    fill(255);
    rectMode(CENTER);
    rect(posX, posY, anchoBrick, altoBrick);
  }//salis dislaybrick


    //se le toma a la parte del iif (unBrick.colision(miPelota.posXpelota, miPelota.posYpelota, miPelota.radio)
  //para 

  boolean colision(float posXPelotaDos, float posYPelotaDos, int radioPelotaDos) {

    if ((posXPelotaDos + radioPelotaDos >= (posX-anchoBrick)) && 
      (posYPelotaDos+radioPelotaDos >= (posY-altoBrick)) &&
      (posXPelotaDos-radioPelotaDos <= posX+anchoBrick) &&
      (posYPelotaDos-radioPelotaDos <= posY+altoBrick))
    {
      return(true);
    }//salir if

    else {
      return(false);
    }//salir else
  }//salir boolean
}//salir clase




//esto va en una nueva pestaña
//======================================================================================





class Paddle {

 int anchoPaddle;
  int altoPaddle;
  int colorPaddle;

  //constructor

  Paddle(int tempPosX, int PaddleWidth, int PaddleHeight, color PaddleColor) {


    anchoPaddle = PaddleWidth;
    altoPaddle = PaddleHeight;
    colorPaddle = PaddleColor;
  }//salir constructor


    void paddleDisplay() {
    fill(colorPaddle);
    rectMode(CENTER);
    rect(mouseX, height*0.9, anchoPaddle, altoPaddle);
  }//salir display
}







//esto va en una nueva pestaña
//======================================================================================





class Pelota {


  // float posXpelota, posYpelota, speedX, speedY;
  int posXpelota, posYpelota, speedX, speedY;
  int diametro;
  color colorPelota;

  int radioPelota = diametro/2;

  //constructor
  //Pelota(float posXpelota_, float posYpelota_, float speedX_, float speedY_, int diametro_, color colorPelota_) {

  Pelota(int posXpelota_, int posYpelota_, int speedX_, int speedY_, int diametro_, color colorPelota_) {


    posXpelota = posXpelota_;
    posYpelota = posYpelota_;
    speedX = speedX_;
    speedY = speedY_;
    diametro = diametro_;
    colorPelota = colorPelota_;
  }//salir constructor


  void displayPelota() {
    fill(colorPelota);
    ellipseMode(CENTER);
    ellipse(posXpelota, posYpelota, diametro, diametro);
  }//salir display

  //las funciones movPelota no van declaradas en el setup xq son void temporales
  void movPelotaX() { 
    speedX = speedX * -1;
  }

  void movPelotaY() {
    speedY= speedY*-1;
  }

  void animarPelota() {
    posXpelota = posXpelota + speedX; //tienenq ue irse actualizando la posicion actual con la velocidad actual
    posYpelota = posYpelota + speedY;


    if ((posXpelota >width-diametro/2) || (posXpelota <0+diametro/2)) {
      movPelotaX();
    }//salir if x



    // if ((posYpelota > height-diametro/2) || (posYpelota <0+diametro/2)) {
    //   movPelotaY();
    //}//salir if y

    if (posYpelota <0+diametro/2) {
      movPelotaY();
    }//salir if y

    if (posYpelota > height) {
      fill(255,0,0);
      text("GAME OVER", width/2, 500);

      if (keyPressed) {
        if (key == ' ' || key == ' ') {
         posYpelota=400;
        }
      }
    }


    //esto es para que funcione el paddle con la pelota y rebote en el paddle
    if ((posYpelota > 665) && (posXpelota < mouseX+60) && (posXpelota > mouseX-60)) {
      movPelotaY();
    }//salir animarpelota
  }//salir mov
}//salir class

