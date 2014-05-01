Processing_RadBot
=================

Let's try this again, shall we?
import processing.serial.*;
Serial myPort;
String str1;
String str2;
int arrCount = 0;
float[] arrXGps = new float[100000];
float[] arrYGps = new float[100000];
float[] intLight = new float[100000];
float fltY;

PrintWriter output;

void setup() {
  println(Serial.list());
   output = createWriter("positions.txt"); 
  // Open the port you are using at the rate you want:
  myPort = new Serial(this, Serial.list()[1], 9600);
  background(0);
  size(1000,1000);
}

void draw() {
  while (myPort.available() > 0)  {
   str1 = myPort.readString();
    if (str1 != null){
      if(str1.indexOf("|")>-1){
        if(arrCount != 0){
          str2 = str2 + str1;
          //println(str2);
          arrXGps[arrCount] = float(str2.substring(1,9));
          fltY= float(str2.substring(12,21));
          arrYGps[arrCount] = fltY;
          intLight[arrCount] = float(str2.substring(24, (str2.length()-2)));
          println((arrXGps[arrCount]));
          output.print("^");
          output.print((arrXGps[arrCount]));
          output.print("^");
          println(arrYGps[arrCount]);
          output.print((arrYGps[arrCount]));
          output.print("^");
         // println(intLight[arrCount]);
          output.print(intLight[arrCount]);
          output.print("^");
          output.print("|");
          
           
            stroke(intLight[arrCount]/4);
            fill(255);
         
          
          ellipse( (1000000*(arrXGps[arrCount]-42.73)),(1000000*((arrYGps[arrCount])+73.68)+500), 5,5);
          str2 = "";
        }
        arrCount = arrCount + 1;
        }else if (str1.indexOf("|")== -1){
        str2 = str2 + str1;
      }
    }
      }
      
   

  }
     void keyPressed() {
  output.flush();  // Writes the remaining data to the file
  output.close();  // Finishes the file
  exit();  // Stops the program
}
