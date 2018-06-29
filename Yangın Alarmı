#include <reg51.h>
#include <stdio.h>
#include <string.h>
#include <intrins.h>


#define satir1	0x80				    
#define satir2	0xC0					

//Fonksiyonlar
void delay(int); //GECIKME FONKSIYONU
void lcdinit(void); //LCD BASLANGIÇ FONKSIYONU

void writecmd(int);
void writedata(char);
void lcd_cmdwr(unsigned char dbyte);

void startAlarmF(void); 				//ALARM BASLATMA FONKSIYONU
void stopAlarmF(void);	 			//ALARMI KAPATMA FONKSIYONU
				

// PINLER
sbit RS = P1^5; 									//(LCD)RS-(AT89C51)P1^5
sbit E  = P1^6; 									//(LCD)E-(AT89C51)P1^6
sbit LED0 = P3^0;								//1.LED
sbit LED1 = P3^1;								//2.LED
sbit SPEAKER= P0^0;							//SPEAKER

sbit stopAlarmP =P1^3;					//ALARMI KAPATAN PIN
sbit startAlarmP = P3^2;				//ALARMI BASLATAN INTERRUPT PINI


void lcd_yaz(char str[])
{
	unsigned char j = 0;
	while (str[j]!= 0)
	{
		writedata(str[j]) ;
	 	j ++;				
	}
	while (j < 16) 				
	{
		writedata(' ') ;
	 	j ++;				
	}	
}
//ALARMIN BASLADIGI YER
void startAlarmF(void) interrupt 0
{
	
//LCD'YE MESAJ GÖNDERILIR
	lcd_cmdwr(satir1);
	lcd_yaz("YANGIN ALARMI!");
	lcd_cmdwr(satir2);
	lcd_yaz("BINAYI BOSALTIN!");
		
	P0= 0X00;
	P1 = 0x00; 
	
	stopAlarmP=1;
	
//LEDLER VE SPEAKER AKTIF EDILIR
		while(stopAlarmP==1){
		LED0=0;
		LED1=0;
		SPEAKER=0;
		delay(100);
			
		LED0=1;
		LED1=1;
		SPEAKER=1;
		delay(100);
			
			if(stopAlarmP==0){
				stopAlarmF();
		}
		
	}
	
}


//ALARMIN KAPATILDIGI YER
void stopAlarmF(void) 
{
	
//EGER ALARM DURDURULDUYSA
	
    
		lcd_cmdwr(satir1);
		lcd_yaz("ALARM");

		lcd_cmdwr(satir2);
		lcd_yaz("DURDURULDU!");
		P0= 0X00;
		P1 = 0x00; 
		
//LEDLER VE SPEAKER SIFIRLANIR VE DEAKTIF EDILIR.
		startAlarmP=1;
		while(startAlarmP==1){
		LED0=0;
		LED1=0;
		SPEAKER=0;	
	}
}


//GECIKME FONKSIYONU 
void delay(int a)
{
	int i;
   
  for(i=0;i<a;i++); 
	 
}

void writedata(char t)
{
   RS = 1;             // => RS = 1   
   P2 = t;             //Data transfer
   E  = 1;             // => E = 1
   delay(150);
   E  = 0;             // => E = 0
   delay(150);
}


void writecmd(int z)
{
   RS = 0;             // => RS = 0
   P2 = z;             //Data transfer
   E  = 1;             // => E = 1
   delay(150);
   E  = 0;             // => E = 0
   delay(150);
}

void lcd_cmdwr(unsigned char dbyte)
{	
	RS=0; 
  E=1;
  P2=dbyte;
  E = 0;
  delay(150);	
}

void lcdinit(void)
{
  
     delay(15000);
   writecmd(0x30);
     delay(4500);
   writecmd(0x30);
     delay(300);
   writecmd(0x30);
     delay(650);
  /////////////////////////////////////////////////////
   writecmd(0x38);    
   writecmd(0x0c);    
   writecmd(0x01);    
   writecmd(0x06);    
}
void main(void)
{
	
	P0 = 0x00;    
	P1 = 0xFF;    
	P2 = 0x00;    
	P3= 0X04;
	
	IP =00001001;
	
	IE = 1; 					//INTERRUPT ENABLE 
	EX0=1;						//INT0 YETKILENDIRILMESI
	EX1=1;						//INT1 YETKILENDIRILMESI
	IT0 = 1; 					//INT0 DUSEN KENAR TETIKLEMELI AYARLANMASI
	IT1=1;						//INT1 DUSEN KENAR TETIKLEMELI AYARLANMASI
	EA = 1;

	stopAlarmP=1;
	startAlarmP=1;
	lcdinit();				//LCD BASLANGIC AYARLAMALARININ YAPILMASI
	
	lcd_cmdwr(satir1);
	lcd_yaz("GUVENLIK");
	lcd_cmdwr(satir2);
	lcd_yaz("AKTIF!");	//EKRANA BASLANGIC IÇIN GÜVENLIK AKTIF MESAJININ VERILMESI
	
	
	while(1){
		
		if(stopAlarmP==0 && startAlarmP==1){ 
			
			while(1){
			stopAlarmF();		//ALARMI KAPATAN FONKSIYON ÇAGRILMASI			}
			
			}
		}
	}	
}
		
		
		




