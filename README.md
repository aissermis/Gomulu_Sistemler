# Gomulu_Sistemler
Gömülü Sistemler ve Uygulamaları dersi için yapılan Hc 06 Bluetooth modülü ve Arduino Uno kullanılarak yapılan araba <br>
projesi yer almaktadır.<br>

Proje videosuna <a href="https://youtu.be/-97DwgG1Tms">buradan  </a> ulaşabilirsiniz<br>

Proje Kodlarını Bluetooth_Araba.ino dosyasında yer almaktadır.<br>

<b>Proje Kodları;</b> <br>
//L298N Bağlantısı  <br> 
  const int motorA1  = 5;  // L298N'in IN3 Girişi<br> 
  const int motorA2  = 6;  // L298N'in IN1 Girişi<br> 
  const int motorB1  = 10; // L298N'in IN2 Girişi<br> 
  const int motorB2  = 9;  // L298N'in IN4 Girişi<br> 
<br> 

  int i=0; //Döngüler için atanan rastgele bir değişken<br> 
  int j=0; //Döngüler için atanan rastgele bir değişken<br> 
  int state; //Bluetooth cihazından gelecek sinyalin değişkeni<br> 
  int vSpeed=255;     // Standart Hız, 0-255 arası bir değer alabilir<br> 
<br> 
void setup() { <br> 
    // Pinlerimizi belirleyelim <br> 
    pinMode(motorA1, OUTPUT);<br> 
    pinMode(motorA2, OUTPUT);<br> 
    pinMode(motorB1, OUTPUT);<br> 
    pinMode(motorB2, OUTPUT);    <br> 
    // 9600 baud hızında bir seri port açalım <br> 
    Serial.begin(9600);<br> 
}<br> 
 <br> 
void loop() {<br> 
  //Bluetooth bağlantısı koptuğunda veya kesildiğinde arabayı durdur.<br> 
 //(Aktif etmek için alt satırın "//" larını kaldırın.)<br> 
//     if(digitalRead(BTState)==LOW) { state='S'; }<br> 
<br> 
  //Gelen veriyi 'state' değişkenine kaydet <br> 
    if(Serial.available() > 0){     <br> 
      state = Serial.read();   <br> 
    }<br> 
  <br> 
  // Uygulamadan ayarlanabilen 4 hız seviyesi.(Değerler 0-255 arasında olmalı)<br> 
    if (state == '0'){<br> 
      vSpeed=0;}<br> 
    else if (state == '1'){<br> 
      vSpeed=100;}<br> 
    else if (state == '2'){<br> 
      vSpeed=180;}<br> 
    else if (state == '3'){<br> 
      vSpeed=200;}<br> 
    else if (state == '4'){<br> 
      vSpeed=255;}<br> 
     
  /***********************İleri****************************/<br> 
  //Gelen veri 'F' ise araba ileri gider.<br> 
    if (state == 'F') {<br> 
      analogWrite(motorA1, vSpeed); <br> analogWrite(motorA2, 0);<br> 
        analogWrite(motorB1, vSpeed);  <br>     analogWrite(motorB2, 0); <br> 
    }
  /**********************İleri Sol************************/<br> 
  //Gelen veri 'G' ise araba ileri sol(çapraz) gider.<br> 
    else if (state == 'G') {<br> 
      analogWrite(motorA1,vSpeed ); <br> analogWrite(motorA2, 0);  <br> 
        analogWrite(motorB1, 100);  <br>   analogWrite(motorB2, 0); <br> 
    }
  /**********************İleri Sağ************************/<br> 
  //Gelen veri 'I' ise araba ileri sağ(çapraz) gider.<br> 
    else if (state == 'I') {<br> 
        analogWrite(motorA1, 100);<br>  analogWrite(motorA2, 0); <br> 
        analogWrite(motorB1, vSpeed);    <br>   analogWrite(motorB2, 0); <br> 
    }
  /***********************Geri****************************/<br> 
  //Gelen veri 'B' ise araba geri gider.<br> 
    else if (state == 'B') {<br> 
      analogWrite(motorA1, 0); <br>   analogWrite(motorA2, vSpeed); <br> 
        analogWrite(motorB1, 0); <br>   analogWrite(motorB2, vSpeed); <br> 
    }
  /**********************Geri Sol************************/<br> 
  //Gelen veri 'H' ise araba geri sol(çapraz) gider<br> 
    else if (state == 'H') {<br> 
      analogWrite(motorA1, 0);  <br>  analogWrite(motorA2, 100); <br> 
        analogWrite(motorB1, 0); <br> analogWrite(motorB2, vSpeed); <br> 
    }
  /**********************Geri Sağ************************/<br> 
  //Gelen veri 'J' ise araba geri sağ(çapraz) gider<br> 
    else if (state == 'J') {<br> 
      analogWrite(motorA1, 0); <br>   analogWrite(motorA2, vSpeed); <br> 
        analogWrite(motorB1, 0);<br>    analogWrite(motorB2, 100); <br> 
    }
  /***************************Sol*****************************/<br> 
  //Gelen veri 'L' ise araba sola gider.<br> 
    else if (state == 'L') {<br> 
      analogWrite(motorA1, vSpeed); <br>   analogWrite(motorA2, 150); <br> 
        analogWrite(motorB1, 0);<br>  analogWrite(motorB2, 0);<br>  
    }
  /***************************Sağ*****************************/<br> 
  //Gelen veri 'R' ise araba sağa gider<br> 
    else if (state == 'R') {<br> 
      analogWrite(motorA1, 0); <br>   analogWrite(motorA2, 0); <br> 
        analogWrite(motorB1, vSpeed);  <br>  analogWrite(motorB2, 150);    <br>  
    }
  
  /************************Stop*****************************/<br> 
  //Gelen veri 'S' ise arabayı durdur.<br> 
    else if (state == 'S'){<br> 
        analogWrite(motorA1, 0);  analogWrite(motorA2, 0); <br> 
        analogWrite(motorB1, 0);  analogWrite(motorB2, 0);<br> 
    }  
}


