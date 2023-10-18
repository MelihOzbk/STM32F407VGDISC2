# STM32F407VGDISC2 ve 7 Segment Display İle Sayaç Yapımı.
Cube IDE üzerinde HAL ile 0-9 veya 0-100 kadar sayan sayaç yapımı

## Ne öğreneceğiz,
* CUBE IDE Proje Açma
* Bir Adet 7 Segment Display ile yapımı
* İki Adet 7 Segment Display ile yapımı

## Cube IDE üzerinde Proje açma
Cube ide penceresi üzerinde STM32 Project Sekmesi açık değilse
File> New > STM32 Project
![alt text](https://github.com/MelihOzbk/STM32F407VGDISC2/blob/main/Proje%201/Pasted%20image%2020231018181824.png?raw=true)

Board Selector Penceresine girdikten sonra Commerical Part Number kısmına "STM32F407G-DISC1" yazıyoruz.
![alt text](https://github.com/MelihOzbk/STM32F407VGDISC2/blob/main/Proje%201/Pasted%20image%2020231018182121.png?raw=true)
Proje ismini girdikten sonra finish yapıyoruz 
![alt text](https://github.com/MelihOzbk/STM32F407VGDISC2/blob/main/Proje%201/Pasted%20image%2020231018182227.png?raw=true)
Kart üzerindeki fonksiyonların standart şekilde çalışabilmesi için evet diyoruz
# Bir adet 7SD ile yapımı
Sadece 0 dan 9 a kadar sayma yapabilir

Açılan IOC sekmesi üzerinde 7 Segment Display için kullanacağımız pinleri GPIO_OUTPUT olarak değiştiriyoruz

![alt text](https://github.com/MelihOzbk/STM32F407VGDISC2/blob/main/Proje%201/Pasted%20image%2020231018182712.png?raw=true)
Kullandığımız pinleri harflendirmek ve Maksimum çıkış hızını ayarlamak için System Core altından GPIO bölümüne giriyoruz

Kullandığımız pinleri bağlı oldukları 7 Segment Display'in pinleri için Harflendiriyoruz
Örneğin PE15 7SD üzerinde A pinine bağlı olduğu için A ismini verdim


![alt text](https://github.com/MelihOzbk/STM32F407VGDISC2/blob/main/Proje%201/Pasted%20image%2020231018182921.png?raw=true)

![alt text](https://github.com/MelihOzbk/STM32F407VGDISC2/blob/main/Proje%201/150px-7_Segment_Display_with_Labeled_Segments.svg.png)
IOC Dosyamızı kaydettikten sonra gerekli çekirdek dosyalarını kendisi oluşturduktan sonra main.c dosyasını bize açacaktır açmazsa Core>Src altında bulabilirsiniz

Kodların başlatıldığı veya sonsuz tekrara uğradığı bölüm öncesinde 
7SD üzerinde sayıları belirlememizde yardımcı olacak 2 eksenli sırayı eklememiz gerekiyor

``` 
int digit[10][7] = {
    {1,1,1,1,1,1,0}, //0
    {0,1,1,0,0,0,0}, //1
    {1,1,0,1,1,0,1}, //2
    {1,1,1,1,0,0,1}, //3
    {0,1,1,0,0,1,1}, //4
    {1,0,1,1,0,1,1}, //5
    {1,0,1,1,1,1,1}, //6
    {1,1,1,0,0,1,0}, //7
    {1,1,1,1,1,1,1}, //8
    {1,1,1,1,0,1,1}  //9
};
```
* 0 Yapması için Örneğin A,B,C,D,E,F pinlerine güç verirken G pinine güç vermeyi keser
 ### Bu bilgiyi ise sayma döngümüzde kullanacak olursak sayacın göstereceği her sayı için gösterim sıramızla güncelleme yapmamız gerekir
``` 
for (int var = 0; var < 10; ++var) {
  			  	  	  	HAL_GPIO_WritePin(A_GPIO_Port, A_Pin, digit[var][0]);
  			  	  		HAL_GPIO_WritePin(B_GPIO_Port, B_Pin, digit[var][1]);
  			  	  		HAL_GPIO_WritePin(C_GPIO_Port, C_Pin, digit[var][2]);
  			  	  		HAL_GPIO_WritePin(D_GPIO_Port, D_Pin, digit[var][3]);
  			  	  		HAL_GPIO_WritePin(E_GPIO_Port, E_Pin, digit[var][4]);
  			  	  		HAL_GPIO_WritePin(F_GPIO_Port, F_Pin, digit[var][5]);
  			  	  		HAL_GPIO_WritePin(G_GPIO_Port, G_Pin, digit[var][6]);
  			  	  		HAL_Delay(1000);
  			  	  		};
``` 
 * var değerinin sayma sayıları içerisinde aldığı değerler için hangi pinleri açmasını gerektiğini gösteren algoritmayı bu şekilde ifade edebiliriz
 * Örneğin var değerinin 0 olduğu durumda 2 boyutlu digit dizisi içerisinden 0 yazması için pinin açık veya kapalılık durumunu belirleyen değerlerini çeker A B C D E F için 1 G için 0 değerini alır
 * ![alt text](https://github.com/MelihOzbk/STM32F407VGDISC2/blob/main/Proje%201/Pasted%20image%2020231018195046.png?raw=true)

# İki adet 7SD ile yapımı

İki adet kullanmamızla beraber bir basamaklı ve 2 basamaklı tüm sayıları yazdırabilmiş olacağız. 

Bir adetle benzer şekilde proje oluşturduktan ve pinleri ABCDEFG olarak harflendirdikten sonra bu sefer 2. 7SD için AA BB CC DD EE FF GG olarak harflendiriyoruz




```
 int delay = 700;
  	  for (int var = 0; var < 32; ++var) {
  		  if (var<10) {
  			  	  	  	HAL_GPIO_WritePin(A_GPIO_Port, A_Pin, digit[0][0]);
  			  	  		HAL_GPIO_WritePin(B_GPIO_Port, B_Pin, digit[0][1]);
  			  	  		HAL_GPIO_WritePin(C_GPIO_Port, C_Pin, digit[0][2]);
  			  	  		HAL_GPIO_WritePin(D_GPIO_Port, D_Pin, digit[0][3]);
  			  	  		HAL_GPIO_WritePin(E_GPIO_Port, E_Pin, digit[0][4]);
  			  	  		HAL_GPIO_WritePin(F_GPIO_Port, F_Pin, digit[0][5]);
  			  	  		HAL_GPIO_WritePin(G_GPIO_Port, G_Pin, digit[0][6]);

  			  	  		HAL_GPIO_WritePin(AA_GPIO_Port, AA_Pin, digit[var][0]);
  			  	  		HAL_GPIO_WritePin(BB_GPIO_Port, BB_Pin, digit[var][1]);
  			  	  		HAL_GPIO_WritePin(CC_GPIO_Port, CC_Pin, digit[var][2]);
  			  	  		HAL_GPIO_WritePin(DD_GPIO_Port, DD_Pin, digit[var][3]);
  			  	  		HAL_GPIO_WritePin(EE_GPIO_Port, EE_Pin, digit[var][4]);
  			  	  		HAL_GPIO_WritePin(FF_GPIO_Port, FF_Pin, digit[var][5]);
  			  	  		HAL_GPIO_WritePin(GG_GPIO_Port, GG_Pin, digit[var][6]);
  			  	  		HAL_Delay(1000);
  		} else {
  					int tendigit = (var / 10) % 10;
					int digi = var - tendigit*10;
					HAL_GPIO_WritePin(A_GPIO_Port, A_Pin, digit[tendigit][0]);
					HAL_GPIO_WritePin(B_GPIO_Port, B_Pin, digit[tendigit][1]);
					HAL_GPIO_WritePin(C_GPIO_Port, C_Pin, digit[tendigit][2]);
					HAL_GPIO_WritePin(D_GPIO_Port, D_Pin, digit[tendigit][3]);
					HAL_GPIO_WritePin(E_GPIO_Port, E_Pin, digit[tendigit][4]);
					HAL_GPIO_WritePin(F_GPIO_Port, F_Pin, digit[tendigit][5]);
					HAL_GPIO_WritePin(G_GPIO_Port, G_Pin, digit[tendigit][6]);

					HAL_GPIO_WritePin(AA_GPIO_Port, AA_Pin, digit[digi][0]);
					HAL_GPIO_WritePin(BB_GPIO_Port, BB_Pin, digit[digi][1]);
					HAL_GPIO_WritePin(CC_GPIO_Port, CC_Pin, digit[digi][2]);
					HAL_GPIO_WritePin(DD_GPIO_Port, DD_Pin, digit[digi][3]);
					HAL_GPIO_WritePin(EE_GPIO_Port, EE_Pin, digit[digi][4]);
					HAL_GPIO_WritePin(FF_GPIO_Port, FF_Pin, digit[digi][5]);
					HAL_GPIO_WritePin(GG_GPIO_Port, GG_Pin, digit[digi][6]);
					HAL_Delay(1000);
  		}
}
``` 

* Tendigit değeri var sayısının onlar basamağını alır ve birinci 7SD için kullanılacak değeri yansıtır
* digi ise birler basamağını alır ve ikinci 7SD kullanılacak değeri yansıtır.
# Örnek IOC görüntüsü
![alt text](https://github.com/MelihOzbk/STM32F407VGDISC2/blob/main/Proje%201/Pasted%20image%2020231018201422.png?raw=true)
# Örnek kodları ise SRC/main.c içinde bulabilirsiniz.
