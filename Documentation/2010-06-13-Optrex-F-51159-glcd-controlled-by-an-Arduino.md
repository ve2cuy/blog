# Optrex F-51159 glcd controlled by an Arduino

*Document converti depuis [ve2cuy.com/blog](https://ve2cuy.com/blog/index.php/2010/06/13/320/) — publié le 2010-06-13 — dernière modification : 2010-06-13*

---

Just got some Optrex F-51159 GLCD’s from eBay.

If you ever need the datasheet or some code example to make them work with an Arduino, read further:

glcd_demo.pde  
============================================================================  
/\*  
   Demo of a Optrex F-51159 controlled by an Arduino.  

   Note:  

   There is no CSEL1, CSEL2 on the F-51159.  Connect CS to  pin 14 (analog_0), Nothing on pin 15 (analog_1)  
   See: <http://www.arduino.cc/playground/Code/GLCDks0108>  

   LCD Bias voltage (-12 to -5 volts) to glcd pin 3 is mandatory.

   Display is 120 x 64 dots, so make these changes to ‘ks0108_Panel.h’ :  
   ———————————————  
       // for F-51159NYJ-SEW-AA  
       #define DISPLAY_WIDTH 120  
       #define DISPLAY_HEIGHT 64

      // panel controller chips  
      #define CHIP_WIDTH     60  // pixels per chip  
   ———————————————  
 \*/

#include <ks0108.h>  
#include « Arial14.h »         
#include « clock.h »           // Clock bitmap

unsigned long startMillis;

void setup(){  
  GLCD.Init(NON_INVERTED);    
  GLCD.ClearScreen();   
  GLCD.DrawBitmap(clock, 0,0, BLACK);  
  GLCD.SelectFont(Arial_14);  
  GLCD.GotoXY(60, 2);  
  GLCD.Puts(« Pixel Show »);  
  GLCD.GotoXY(61, 47);  
  GLCD.Puts(« by ve2cuy »);  
  countdown(9);  
}

void countdown(int count){  
    while(count–){  
     GLCD.GotoXY(30, 40);  
     GLCD.PutChar(count + ‘0’);  
     delay(1000);   
     GLCD.GotoXY(30, 40);  
     GLCD.Puts( »  « );  
  }   
}

void  loop(){  
  GLCD.ClearScreen();

  startMillis = millis();  
  while( millis() – startMillis < 15000){  
    GLCD.SetDot( random(120), random(64), BLACK);  
  }

  startMillis = millis();  
  while( millis() – startMillis < 15000){  
    GLCD.SetDot( random(120), random(64), WHITE);  
  }

  GLCD.Init(INVERTED);   
  GLCD.ClearScreen();  
  GLCD.GotoXY(36, 24);  
  GLCD.Puts(« The  end… »);  
  while (true){}  
}  
============================================================================

Datasheet:

[F-51159](fichiers/f-51159.pdf)

`▶️ Vidéo YouTube : [Voir la vidéo](https://www.youtube.com/watch?v=yUbRnAU_2Is)`
