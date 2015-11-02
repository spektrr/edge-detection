# edge-detection

import processing.video.*;
import gab.opencv.*;

OpenCV opencv;
Capture cam;

HScrollbar hs1, hs2;

PImage src, canny;
int minThres;
int maxThres;

int rminThres;
int rmaxThres;

color white=color(255);

boolean b = false;

//int mainColour = color(255);
//boolean isIt = true;

//int i = 0, j = 0;

void setup(){
  size(640,480);
  frameRate(24);
  
  cam = new Capture(this,640,480);
  cam.start();
  opencv = new OpenCV(this, cam);
  
//  checkColor = color(255);
  
  
  hs1 = new HScrollbar(0, 8, width, 16, 16);
  hs2 = new HScrollbar(0, 32, width, 16, 16);
  

}

void captureEvent(Capture c){
  c.read();
}

void draw(){
  background(0); 
  
  
  
  float rminThres = hs1.getPos() - width/2;
  float rmaxThres = hs2.getPos() - width/2;
  
  minThres = round(rminThres);
  maxThres = round(rmaxThres);
  
  opencv.loadImage(cam);
  opencv.findCannyEdges(minThres,maxThres);
  canny = opencv.getSnapshot();
  loadPixels();
  
//  image(canny, width/2 - cam.width/2 + rminThres*1.5, cam.height-480);
  image(canny, cam.width - 640, cam.height-480);
  
  for(int x = 0; x < cam.width; x++){
    for(int y = 0; y < cam.height; y++){
      int loc = x + y*cam.width;
      if(pixels[loc] > color(100)){
        b = true;
      }
    }

  }
  if (b == true){
    println("yoooho");
  }
  
  hs1.update();
  hs2.update();
  hs1.display();
  hs2.display();
  
//  for(int i = 0; i<pPixels.length; i++){
//      myPixels = cam.get(i,j);
//      i++;
//        if(i == width){
//          i = 0;
//          j++;
//      }
//        if(j == height){
//          j = 0;
//      }
//      if (myPixels == mainColour){
//    isIt = true;
//    println("yaay");
//  }
//  }

  
  
}
