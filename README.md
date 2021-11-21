# Signals And Slots


- [Introduction](#INTRO)
- [Traffic Light](#TrLight)
- [Digital Clock](#DigiClock)
- [Calculator](#Calcul)

<div id = "back"></div>


    
## **Introduction** 



<a name="INTRO"></a>
    
"" **Programming isn't about what you know; it's about what you can figure out.**""

A **widget** is an element of a graphical user interface (**GUI**) which displays information or provides a specific way for a user to interact with the operating system or an application. 

In **programming**, widget also means the small program that is written in order to describe what a particular widget looks like, how it behaves and how it responds to user actions. Most operating systems include a set of ready-to-tailor widgets that a programmer can incorporate in an application, specifying how it is to behave. New widgets can be created.

The **Layout Widget** is a responsive container that allows you to separate a responsive container into sections. Layouts can be placed inside the responsive containers of other layouts to create more even sections.

**Signals** and **Slots** is a language construct introduced in Qt for communication between objects which makes it easy to implement the observer pattern while avoiding boilerplate code. The concept is that GUI widgets can send signals containing event information which can be received by other widgets / controls using special functions known as slots. This is similar to C/C++ function pointers, but signal/slot system ensures the type-correctness of callback arguments.
They are used for communication between objects. The signals and slots mechanism is a central feature of Qt and probably the part that differs most from the features provided by other frameworks.


![Image](/intro.png)

Example of a Form 
    

 
 >**In This Zip you will have the project** [WidgetLayout.zip](https://github.com/HarirFahem/homework22/blob/main/part2_hw2_fahem_harir.zip) 

### Traffic Light
<a name="TraLight"></a>

**Traffic Light** is a set of automatically operated coloured lights, typically red, yellow, and green, for controlling traffic at road junctions, pedestrian crossings, and roundabouts.

In this Exercise, we used the QTimer to simulate the traffc light.

We created a Class called **traffic light** with 3 options: 
**The First One** is that the colors are displayed randomly.

Here is the code for the class:
```javascript

class TrafficLight: public QWidget{
  Q_OBJECT

public:

  TrafficLight(QWidget * parent = nullptr);
  void timerEvent(QTimerEvent *e) override;
protected:
     void createWidgets();
     void placeWidgets();
     void makeConnexions();
private:

  QRadioButton * redlight;
  QRadioButton * yellowlight;
  QRadioButton * greenlight;
  QVector<QRadioButton*> lights;
  int index;//indice du light active
  int currentTime;//temps d'activation du feu


};
```

And here is the implementation:
```javascript
TrafficLight::TrafficLight(QWidget * parent): QWidget(parent){

    //Creatign the widgets
    createWidgets();

    //place Widgets
    placeWidgets();
    currentTime=0;
}

void TrafficLight::createWidgets()
{
    redlight = new QRadioButton;
    redlight->setEnabled(false);
    redlight->toggle();//activer le radio Button
    redlight->setStyleSheet("QRadioButton::indicator:checked { background-color: red;}");

    yellowlight = new QRadioButton;
    yellowlight->setEnabled(false);
    yellowlight->toggle();
    yellowlight->setStyleSheet("QRadioButton::indicator:checked { background-color: yellow;}");

    greenlight = new QRadioButton;
    greenlight->setEnabled(false);
    greenlight->toggle();
    greenlight->setStyleSheet("QRadioButton::indicator:checked { background-color: green;}");

  //ajouter les lights dans un tableu
  lights.append(redlight);
  lights.append(yellowlight);
  lights.append(greenlight);
  index=0;
  startTimer(1000);


}


void TrafficLight::placeWidgets()
{

  // Placing the widgets
  auto layout = new QVBoxLayout;
  layout->addWidget(redlight);
  layout->addWidget(yellowlight);
  layout->addWidget(greenlight);
  setLayout(layout);
}


void TrafficLight::timerEvent(QTimerEvent *e){
    //index light active
    index=(index+1)%3;
    //lights(vecteur des lights)
     lights[index]->toggle();

}
```
![Image](/randomly_option2.png)

colors display randomly

**The Second One** is the keypress, when we press R the red is displaying, Y for the yellow and G for the green.

Here is the code for the class:
```javascript
class TrafficLight: public QWidget{
  Q_OBJECT

public:

  TrafficLight(QWidget * parent = nullptr);
  void keyPressEvent(QKeyEvent *e) override;

protected:
     void createWidgets();
     void placeWidgets();
     void makeConnexions();
private:
  QRadioButton * redlight;
  QRadioButton * yellowlight;
  QRadioButton * greenlight;
  QVector<QRadioButton*> lights;
  int index;
  int currentTime;
};
```

And here is the implementation:
```javascript

TrafficLight::TrafficLight(QWidget * parent): QWidget(parent){

    //Creatign the widgets
    createWidgets();

    //place Widgets
    placeWidgets();
   index=0;
}

void TrafficLight::createWidgets()
{

  redlight = new QRadioButton;
  redlight->setEnabled(false);
  redlight->toggle();//activer le radio Button
  redlight->setStyleSheet("QRadioButton::indicator:checked { background-color: red;}");

  yellowlight = new QRadioButton;
  yellowlight->setEnabled(false);
  yellowlight->toggle();
  yellowlight->setStyleSheet("QRadioButton::indicator:checked { background-color: yellow;}");

  greenlight = new QRadioButton;
  greenlight->setEnabled(false);
  greenlight->toggle();
  greenlight->setStyleSheet("QRadioButton::indicator:checked { background-color: green;}");
  startTimer(1000);
  currentTime=0;
  //ajouter les lights dans un tableu
  lights.append(redlight);
  lights.append(yellowlight);
  lights.append(greenlight);


}


void TrafficLight::placeWidgets()
{

  // Placing the widgets
  auto layout = new QVBoxLayout;
  layout->addWidget(redlight);
  layout->addWidget(yellowlight);
  layout->addWidget(greenlight);
  setLayout(layout);
}
void TrafficLight::keyPressEvent(QKeyEvent *e){


//traiter le cas du rouge

    if(e->key() == Qt::Key_R){
        index=0;
        lights[index]->toggle();
    }
    if(e->key() == Qt::Key_Y){
        index=1;
        lights[index]->toggle();
    }
    if(e->key() == Qt::Key_G){
        index=2;
        lights[index]->toggle();
    }


    if(e->key() == Qt::Key_Escape)
        qApp->exit();
}
```
![Image](/option3.png)

colors provided the key pressed 

**The Last One** is to give 4 seconds for the Red, 1 second for the Yellow and 2 for the Green in addition of the keypress.

Here is the code for the class:

```javascript
class TrafficLight: public QWidget{
  Q_OBJECT

public:

  TrafficLight(QWidget * parent = nullptr);
  void timerEvent(QTimerEvent *e) override;
  void keyPressEvent(QKeyEvent *e) override;

protected:
     void createWidgets();
     void placeWidgets();
     void makeConnexions();

private:

  QRadioButton * redlight;
  QRadioButton * yellowlight;
  QRadioButton * greenlight;
  QVector<QRadioButton*> lights;
  int times[3]={4,1,2}; 
  int index;
  int currentTime;

};

```
And here is the implementation of the methods:

```javascript

TrafficLight::TrafficLight(QWidget * parent): QWidget(parent){

    //Creatign the widgets
    createWidgets();

    //place Widgets
    placeWidgets();
   index=0;
}

void TrafficLight::createWidgets()
{

  redlight = new QRadioButton;
  redlight->setEnabled(false);
  redlight->toggle();//activer le radio Button
  redlight->setStyleSheet("QRadioButton::indicator:checked { background-color: red;}");

  yellowlight = new QRadioButton;
  yellowlight->setEnabled(false);
  yellowlight->toggle();
  yellowlight->setStyleSheet("QRadioButton::indicator:checked { background-color: yellow;}");

  greenlight = new QRadioButton;
  greenlight->setEnabled(false);
  greenlight->toggle();
  greenlight->setStyleSheet("QRadioButton::indicator:checked { background-color: green;}");
  startTimer(1000);
  currentTime=0;
  lights.append(redlight);
  lights.append(yellowlight);
  lights.append(greenlight);


}


void TrafficLight::placeWidgets()
{

  // Placing the widgets
  auto layout = new QVBoxLayout;
  layout->addWidget(redlight);
  layout->addWidget(yellowlight);
  layout->addWidget(greenlight);
  setLayout(layout);
}
void TrafficLight::keyPressEvent(QKeyEvent *e){


//traiter le cas du rouge

   if(e->key() == Qt::Key_R){
        index=0;
        lights[index]->toggle();
    }
    if(e->key() == Qt::Key_Y){
        index=1;
        lights[index]->toggle();
    }
    if(e->key() == Qt::Key_G){
        index=2;
        lights[index]->toggle();
    }
    if(e->key() == Qt::Key_Escape)
        qApp->exit();
}

void TrafficLight::timerEvent(QTimerEvent *e){
    currentTime++;
     if(redlight->isChecked()&&currentTime==4){
         yellowlight->toggle();
         currentTime=0;
     }
     if(yellowlight->isChecked()&&currentTime==1){
         greenlight->toggle();
         currentTime=0;
     }
     if(greenlight->isChecked()&&currentTime==2){
        redlight->toggle();
         currentTime=0;
     }

}

}

```
And for showing the window we wrote in the main class the following code:
```javascript

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    //Creating the traffic light
    auto light = new TrafficLight;
    //showing the trafic light
    light->show();
    return a.exec();
}
```
And here is the Form that we Obtain :

![Image](/traffic_light.png)

A traffic light example
    

 [(**Back to top**)](#back)

## Digital Clock
<a name="DigiClock"></a>
A **Digital Clock** is a clock that shows the current time in format(hours : minutes : seconds), it is a type of clock that displays the time digitally 
here is the code of the class:
```javascript
#ifndef DIGITALCLOCK_H
#define DIGITALCLOCK_H

#include <QWidget>
#include<QLCDNumber>
#include<QTimerEvent>

class DigitalClock : public QWidget
{

public:
    explicit DigitalClock(QWidget *parent = nullptr);

protected:
  void   createWidgets();
  void  placeWidgets();
  void updateTime();

protected:
  QLCDNumber *hour;
  QLCDNumber *second;
  QLCDNumber *minute;

  //surcharger
  void timerEvent(QTimerEvent *e) override;

};

#endif // DIGITALCLOCK_H

```

Here is the implementation of the methods:
```javascript
#include "digitalclock.h"
#include<QTime>
#include<QHBoxLayout>

DigitalClock::DigitalClock(QWidget *parent) : QWidget(parent)
{
createWidgets();

placeWidgets();
setWindowTitle("Digital Clock");
startTimer(1000);
}
void DigitalClock::updateTime(){
    //mettre le temp
    auto T =QTime::currentTime();
    hour->display(T.hour());
    minute->display(T.minute());
    second->display(T.second());



}

void DigitalClock::createWidgets(){
    hour = new QLCDNumber;
    minute = new QLCDNumber;
    second = new QLCDNumber;

   // mettre le temp
    auto T =QTime::currentTime();
    hour->display(T.hour());
    minute->display(T.minute());
    second->display(T.second());
    //have a color in the background and the numbres
   hour->setStyleSheet("background: black; color: #72A8CB");
   minute->setStyleSheet("background: black; color: #3291CD");
   second->setStyleSheet("background: black; color: #0B45C6");

}

void DigitalClock::placeWidgets(){

   hour->setMinimumHeight(100);
   minute->setMinimumHeight(100);
   second->setMinimumHeight(100);
 QLayout * layout = new QHBoxLayout;
 setLayout(layout);
 layout->addWidget(hour);
 layout->addWidget(minute);
 layout->addWidget(second);


}


void DigitalClock::timerEvent(QTimerEvent *e){
    auto T =QTime::currentTime();
    hour->display(T.hour());
    minute->display(T.minute());
    second->display(T.second());

}

```
And for the Mainly part:
 ```javascript
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
   //showing digital clock
   auto D = new DigitalClock;
   D->show();
    return a.exec();
}
 ```
 
 And here is the result which is showing in the current time:
 
 ![Image](/digital_clock.png)

[(**Back to top**)](#back)

##  Calculator

<a name="Calcul"></a>
    
We added the interactive functionality to the calculator widgets written in the first part of the homework. We used **signals and slots** to simulate a calculator that supports operations: "*","+","-","/","sqrt","1/x", "x^2", "cos", "sin", "ln"
 
 The result of the first part:
 
 ![Image](/numerickeypad.png)
 
 
 To start with, we represented all the mathematical operations by : 
 ```markdown
 left (op) right
 ```
 
Then we respond to each digit click, we used the **Sender** approach which allpw a slot to get the identity of the sender object, from that we got the button which was clicked.

Then , we passed to the digits interaction, in which we connected all the buttons to the slot, the function will use the sender method to get the identify of which button was clicked and act accordingly.
After, we focused on the left and the right operand, and we made part of the operation_interaction(using the same sender method)

Finally, we implement the slot for the enter button to compute the result of combining the left and the right value according to the entered operation

for the code of the class:

```javascript
class Calculator : public QWidget
{
    Q_OBJECT

public:
    Calculator(QWidget *parent = nullptr);
    ~Calculator();


protected:
    void createWidgets();
    void placeWidgets();
    void makeConnexions();

public slots:
  void newDigit();
  void changeOperation();
  void Clear();
  void ShowResult();
  void ShowResult1();
  void Ln();

  private:

      QGridLayout *buttonsL;
      QVBoxLayout *Layout;
      QVector<QPushButton*> digits;
      QPushButton *clear;
      QPushButton *Close;
      QVector<QPushButton*> operations;
      QVector<QPushButton*> operations1;
      QLCDNumber *disp;
      QPushButton *point;
      QPushButton *result;
      double * left=nullptr;
      double * right=nullptr;
      QString *operation=nullptr;


};


```

For the implementation of the methods:
```javascript

Calculator::Calculator(QWidget *parent) : QWidget(parent)
{
createWidgets();
placeWidgets();
makeConnexions();

}

Calculator::~Calculator()
{

    delete buttonsL;
    delete disp;
    delete Layout;
    delete clear;
    delete Close;

}

void Calculator::createWidgets(){
    Layout = new QVBoxLayout;


    //LCD
    disp = new QLCDNumber(this);
    disp->setDigitCount(6);
    disp->setStyleSheet("background: black; color: #FFFF00");
    disp->setSegmentStyle(QLCDNumber::Flat);

   buttonsL= new QGridLayout;


   for(int i=0; i < 10; i++)
   {
       digits.push_back(new QPushButton(QString::number(i)));
       digits.back()->setSizePolicy(QSizePolicy::Expanding, QSizePolicy::Fixed);
       digits.back()->resize(sizeHint().width(), sizeHint().height());
   }

   clear = new QPushButton("CLEAR",this);
   clear->setSizePolicy(QSizePolicy::Expanding, QSizePolicy::Fixed);
   clear->resize(sizeHint().width(), sizeHint().height());


   Close= new QPushButton("CLOSE",this);
   Close->setSizePolicy(QSizePolicy::Expanding, QSizePolicy::Fixed);
   Close->resize(sizeHint().width(), sizeHint().height());

   point = new QPushButton("ln",this);
   result = new QPushButton("=",this);

      // Operations

      operations1.push_back(new QPushButton("/"));
      operations1.push_back(new QPushButton("*"));
      operations1.push_back(new QPushButton("-"));
      operations1.push_back(new QPushButton("+"));

      operations.push_back(new QPushButton("X²"));
      operations.push_back(new QPushButton("Sqrt"));
      operations.push_back(new QPushButton("1/x"));
      operations.push_back(new QPushButton("Cos"));
      operations.push_back(new QPushButton("Sin"));


}
void Calculator::placeWidgets(){

       Layout->addWidget(disp);
       Layout->addLayout(buttonsL);

       for(int i=1; i <10; i++)
           buttonsL->addWidget(digits[i], (i-1)/3, (i-1)%3);

       //Adding the operations
       for(int i=0; i < 4; i++)
           buttonsL->addWidget(operations1[ i], i, 3);

       for(int i=0; i < 5; i++)
           buttonsL->addWidget(operations[ i], i,4 );


       //Adding the  button
       buttonsL->addWidget(digits[0], 3, 0);
       buttonsL->addWidget(point,3, 1);
       buttonsL->addWidget(result, 3, 2);
       buttonsL->addWidget(clear,4,0,1,2);
       buttonsL->addWidget(Close,4,2,1,2);

       setLayout(Layout);

}

void Calculator::newDigit( )
{
    auto button = dynamic_cast<QPushButton*>(sender());

               //getting the value
               double value = button->text().toDouble();
               //Check if we have an operation defined
               if(operation)
                   {
                       //check if we have a value or not
                           *right = 10 * (*right) + value;
                       disp->display(*right);

                   }
                   else
                   {
                       if(!left)
                           left = new double {value};
                       else
                           *left = 10 * (*left) + value;

                       disp->display(*left);
                   }
          }





void Calculator::changeOperation(){
    auto button = dynamic_cast<QPushButton*>(sender());

        //Storing the operation
        operation = new QString{button->text()};

        //Initiating the right button
        right = new double{0};

        //reseting the display
        disp->display(0);

}

void Calculator::makeConnexions(){

    for(int i=0; i <10; i++)
             connect(digits[i], &QPushButton::clicked,
                     this, &Calculator::newDigit);


        connect(Close, &QPushButton::clicked, this, &Calculator::close);

        for(int i=0; i <4; i++){
                            connect(operations1[i], &QPushButton::clicked,
                                    this, &Calculator::changeOperation);
    }
        for(int i=0; i <5; i++){
                            connect(operations[i], &QPushButton::clicked,
                                    this, &Calculator::changeOperation);
      }
      connect(result, &QPushButton::clicked,this, &Calculator::ShowResult);
      connect(result, &QPushButton::clicked,this, &Calculator::ShowResult1);



        connect(clear,&QPushButton::clicked,this, &Calculator::Clear);
        connect(point,&QPushButton::clicked,this, &Calculator::Ln);

}
    void Calculator::ShowResult(){
      if(*operation=="+"){
         *left=*left + *right;
         disp->display(*left);
         right = new double{0};
       }
        else if(*operation=="-"){
         *left=*left-(*right);
         disp->display(*left);
         right = new double{0};
       }
       else if(*operation=="*"){
       *left=(*left)*(*right);
        disp->display(*left);
         right = new double{1};
       }
       else if(*operation=="/"){
       if(*right==0)
        disp->display("error");
       else {
        *left=*left/((*right));
         disp->display(*left);
        right = new double{1};
       }
      }
    }

    void Calculator::ShowResult1(){
        if(*operation=="Sqrt"){
            if(*left==0)
                disp->display("error");
            else{
                    *left = std::sqrt(*left);
                    disp->display(*left);
                    left = new double{0};
                }
        }else if(*operation=="X²"){

              *left = qPow(*left, 2.0);
             disp->display(*left);


        }else if(*operation=="1/X"){
            *left = 1.0 / *left;
            disp->display(*left);

        }else if (*operation=="Cos"){
            *left= qCos(*left);
            disp->display(*left);


        }else if (*operation=="Sin"){
            *left= qSin(*left);
            disp->display(*left);

        }

    }
void Calculator::Clear()
{

    left = new double{0};
    right = new double{0};
    operation=nullptr;
  disp->display("0");

}
void Calculator::Ln()
{
    *left= qLn(*left);
    disp->display(*left);
    left = new double{0};

}
```
we use this code to move the main window which is named calc in our project to the center(getting the size of the screen then creating a rectangle around the screen and then the form is going to the center of this rectangle)
```markdown
calc.move(QApplication::desktop()->screen()->rect().center()-calc.rect().center());
```
And for the mainly part:
```javascript
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
     Calculator calc;
     calc.setWindowTitle("Calculator");
     calc.showMaximized();
     calc.setFixedSize(400,500);
     calc.move(QApplication::desktop()->screen()->rect().center()-calc.rect().center());

    return a.exec();
}
```
![Image](/calculatorr.png)

Calculator Form

 In this project we analyzed and coded many forms using the widget layout, and we made communication between objects using the signals and slots.
    
   
    
 [(**Back to top**)](#back)
    
Made By:
* Khadija Fahem
* Wafa Harir

    
  
