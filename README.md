# Signals And Slots


- [Introduction](#INTRO)
- [Traffic Light](#TrLight)
- [Digital Clock](#DigiClock)
- [Calculator](#Calcul)

<div id = "back"></div>


    
## **Introduction** 



<a name="INTRO"></a>
    
"" **All programming is maintenance programming, because you are rarely writing original code.**""

A **widget** is an element of a graphical user interface (**GUI**) which displays information or provides a specific way for a user to interact with the operating system or an application. 

**Widgets** include icons, pull-down menus, buttons, selection boxes, progress indicators, on-off checkmarks, scroll bars, windows, window edges, toggle buttons, form, and many other devices for displaying information and for inviting, accepting, and responding to user actions.

In **programming**, widget also means the small program that is written in order to describe what a particular widget looks like, how it behaves and how it responds to user actions. Most operating systems include a set of ready-to-tailor widgets that a programmer can incorporate in an application, specifying how it is to behave. New widgets can be created.

The **Layout Widget** is a responsive container that allows you to separate a responsive container into sections. Layouts can be placed inside the responsive containers of other layouts to create more even sections.

The Header and Footer have constant heights with responsive widths, while the sidebars have a constant width and responsive heights. The Columns and rows have both responsive widths and heights, but you can change the percent of available space they will occupy relative to the space not being occupied by headers, footers, and sidebars.


![Image](/intro.png)

Example of a Form 
    

 
 >**In This Zip you will have the project** [WidgetLayout.zip]() 

### Traffic Light
<a name="TraLight"></a>

**Traffic Light** is a set of automatically operated coloured lights, typically red, yellow, and green, for controlling traffic at road junctions, pedestrian crossings, and roundabouts.

In this Exercise, we used the QTimer to simulate the traffc light.

We created a Class called **traffic light** with 3 options: 
**The First One** is that the colors are displayed randomly.

Here is the code for the class:*
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

**The Second One** is the keypress, when we press R the red is displaying, Y for the yellow and G for the green.

**The Last One** is to give 4 seconds for the Red, 1 second for the Yellow and 2 for the Green.

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
  int times[3]={4,1,2}; //temps de chacune
  int index;//indice du light active
  int currentTime;//temps d'activation du feu

};

```
And here is the implementation of the methods:
for the first **option**:

for the second **option**:

for the last **one**::
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

  //surcherger
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
    



 In this project we analyzed and coded many forms using the widget layout.
    
   
    
 [(**Back to top**)](#back)
    
Made By:
* Khadija Fahem
* Wafa Harir

    
  
