- 👋 Hi, I’m @KodMancerX
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
KodMancerX/KodMancerX is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
 243 değişiklik: 126 ekleme ve 117 silme243 
arduino/bitkiKordArduino/bitkiKordArduino.ino
Orijinal dosya satır numarası	Farklı satır numarası	Fark satırı değişikliği
@@ -1.117 +1.126 @@
sabit  int HavaDeğeri = 410 ;
sabit  int SuDeğeri = 188 ;
sabit  int seriGöndermeZamanı = 5000 ;
sabit  int nemÖrnekOranı = 1200 ;
sabit  int pompaSuSüresi = 50 ;
sabit  int pompaPini = 2 ;
sabit  int pumpPrimeButtonPin = 4 ;
sabit  int fanPin = 6 ;
sabit bayt VERİ_MAKS_BOYUTU = 32 ;
char veri[VERİ_MAKS_BOYUTU];

int toprakNemDizisi [] = { 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 } ;
int toprakNemDiziUzunluğu = 15 ;
imzasız  uzun seriGönderZamanlayıcısı = 0 ;
int pumpPrimeButtonState = 0 ;


geçersiz  kurulum () {
  pinMode (fanPin, ÇIKIŞ);
  pinMode (pompaPin, ÇIKIŞ);
  pinMode (pumpPrimeButtonPin, GİRİŞ_ÇEKME);
  Seri. başla ( 9600 );
}
boş  döngü () {
  int mevcutToprakNemi = MevcutToprakNeminiAl ();
  if ( millis () > seriSendTimer || millis () < (serialSendTimer - seriSendTime)){
    Seri. print ( " m= " );
    Seri.println ( currentSoilMoisture);
    seriGönderZamanlayıcı = milis () + seriGönderZamanı;
  }
  veri al ();
  tutamakPompa ();
  tutamakFan ();
  gecikme (moistureSampleRate);
}

boş  tutamakFan (){  
  eğer ( Dize (veri[ 0 ]) == " f " ){
    // Serial.println("Fan komutu algılandı");
    char *p = &veri[ 1 ];
    int fanPower = atoi (p);
    eğer (fanPower < 0 ){
      fanGücü = 0 ;
    } değilse  eğer (fanPower > 255 ){
      fanGücü = 255 ;
    }    
    analogWrite (fanPin, fanGücü);
    // Serial.println(fanGücü);
  }
}

void  tutamakPompa (){
  eğer ( digitalRead (pumpPrimeButtonPin) == YÜKSEK){
    eğer (pumpPrimeButtonState == 1 ){
      digitalWrite (pompaPin, DÜŞÜK);
      pumpPrimeButtonState = 0 ;
    }
    eğer ( Dize (veri) == " wtr " ){
      // su komutu alındı
      digitalWrite (pompaPin, YÜKSEK);
      gecikme (pompaSuSüresi);
      digitalWrite (pompaPin, DÜŞÜK);
    }
  } başka {
    eğer (pumpPrimeButtonState == 0 ){
      digitalWrite (pompaPin, YÜKSEK);
      pumpPrimeButtonState = 1 ;
    }
  }
}

int  getCurrentSoilMoisture (){
  int toprakNemDeğeri = analogOkuma (A0);
  Seri.println ( toprakNemDeğeri);
  int toprakNemYüzdesi = map (toprakNemDeğeri, HavaDeğeri, SuDeğeri, 0 , 100 );
  eğer (toprakNemYüzdesi >= 100 ){
    toprakNemYüzdesi = 100 ;
  } değilse  eğer (toprakNemYüzdesi <= 0 ){
    toprakNemYüzdesi = 0 ;
  }
  // yeni değeri nem dizisine gönder
  int i = toprakNemDiziUzunluğu- 1 ; i > 0 ; i--) için {
    toprakNemDizisi[i] = toprakNemDizisi[i - 1 ];
  }
  toprakNemDizisi[ 0 ] = toprakNemYüzdesi;
  // dizi ortalamasını döndür
  int toprakNemYüzdesiOrtalama = 0 ;
  int i = 0 ; i < toprakNemDiziUzunluğu; i++) için {
    toprakNemYüzdesiOrtalama += toprakNemDizisi[i];
    // Serial.print(toprakNemDizisi[i]);
    // Seri.print(",");
  }
  // Seri.println("]");
  toprakNemYüzdesiOrtalama = toprakNemYüzdesiOrtalama / toprakNemDiziUzunluğu;
  soilMoisturePercentAverage değerini döndür ;
}

geçersiz  veri al () {
  statik  char endMarker = ' \n ' ;
  char alındıChar;
  int ndx = 0 ;
  // veri arabelleğini temizle
  memset (veri, 32 , sizeof (veri));
  (Seri.mevcut ( ) > 0 ) iken {
    receivedChar = Serial.okuma ( );
    eğer (alınanKarakter == bitişİşaretleyicisi) {
      veri[ndx] = ' \0 ' ;
      geri dönmek ;
    }
    veri[ndx] = alınanKarakter;
    ndx++;
    eğer (ndx >= DATA_MAX_SIZE) {
      kırmak ;
    }
  }
  memset (veri, 32 , sizeof (veri));
}
sabit  int HavaDeğeri = 410 ;
sabit  int SuDeğeri = 188 ;
sabit  int seriGöndermeZamanı = 5000 ;
sabit  int nemÖrnekOranı = 1200 ;
sabit  int pompaSuSüresiMin = 10 ;
sabit  int pompaSuSüresiMaksimum = 100 ;
sabit  int pompaPini = 2 ;
sabit  int pumpPrimeButtonPin = 4 ;
sabit  int fanPin = 6 ;
sabit bayt VERİ_MAKS_BOYUTU = 32 ;
char veri[VERİ_MAKS_BOYUTU];

int toprakNemDizisi [] = { 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 } ;
int toprakNemDiziUzunluğu = 15 ;
imzasız  uzun seriGönderZamanlayıcısı = 0 ;
int pumpPrimeButtonState = 0 ;


geçersiz  kurulum () {
  pinMode (fanPin, ÇIKIŞ);
  pinMode (pompaPin, ÇIKIŞ);
  pinMode (pumpPrimeButtonPin, GİRİŞ_ÇEKME);
  Seri. başla ( 9600 );
}
boş  döngü () {
  int mevcutToprakNemi = MevcutToprakNeminiAl ();
  if ( millis () > seriSendTimer || millis () < (serialSendTimer - seriSendTime)){
    Seri. print ( " m= " );
    Seri.println ( currentSoilMoisture);
    seriGönderZamanlayıcı = milis () + seriGönderZamanı;
  }
  veri al ();
  tutamakPompa ();
  tutamakFan ();
  gecikme (moistureSampleRate);
}

boş  tutamakFan (){  
  eğer ( Dize (veri[ 0 ]) == " f " ){
    // Serial.println("Fan komutu algılandı");
    char *p = &veri[ 1 ];
    int fanPower = atoi (p);
    eğer (fanPower < 0 ){
      fanGücü = 0 ;
    } değilse  eğer (fanPower > 255 ){
      fanGücü = 255 ;
    }    
    analogWrite (fanPin, fanGücü);
    // Serial.println(fanGücü);
  }
}

void  tutamakPompa (){
  eğer ( digitalRead (pumpPrimeButtonPin) == YÜKSEK){
    eğer (pumpPrimeButtonState == 1 ){
      digitalWrite (pompaPin, DÜŞÜK);
      pumpPrimeButtonState = 0 ;
    }
    eğer ( Dize (veri[ 0 ]) == " w " ){
      // su komutu alındı
      char *p = &veri[ 1 ];
      int pompaSuSüresi = atoi (p);
      // bir şeyler ters giderse diye min max tanımlandı.
      eğer (pompaSuSüresi < pompaSuSüresiMin){
        pompaSuZamanı = pompaSuZamanıMin;
      } else  if (pompaSuSüresi > pompaSuSüresiMaksimum){
        pompaSuSüresi = pompaSuSüresiMaksimum;
      }    
      digitalWrite (pompaPin, YÜKSEK);
      gecikme (pompaSuSüresi);
      digitalWrite (pompaPin, DÜŞÜK);
    }
  } başka {
    eğer (pumpPrimeButtonState == 0 ){
      digitalWrite (pompaPin, YÜKSEK);
      pumpPrimeButtonState = 1 ;
    }
  }
}

int  getCurrentSoilMoisture (){
  int toprakNemDeğeri = analogOkuma (A0);
  Seri.println ( toprakNemDeğeri);
  int toprakNemYüzdesi = map (toprakNemDeğeri, HavaDeğeri, SuDeğeri, 0 , 100 );
  eğer (toprakNemYüzdesi >= 100 ){
    toprakNemYüzdesi = 100 ;
  } değilse  eğer (toprakNemYüzdesi <= 0 ){
    toprakNemYüzdesi = 0 ;
  }
  // yeni değeri nem dizisine gönder
  int i = toprakNemDiziUzunluğu- 1 ; i > 0 ; i--) için {
    toprakNemDizisi[i] = toprakNemDizisi[i - 1 ];
  }
  toprakNemDizisi[ 0 ] = toprakNemYüzdesi;
  // dizi ortalamasını döndür
  int toprakNemYüzdesiOrtalama = 0 ;
  int i = 0 ; i < toprakNemDiziUzunluğu; i++) için {
    toprakNemYüzdesiOrtalama += toprakNemDizisi[i];
    // Serial.print(toprakNemDizisi[i]);
    // Seri.print(",");
  }
  // Seri.println("]");
  toprakNemYüzdesiOrtalama = toprakNemYüzdesiOrtalama / toprakNemDiziUzunluğu;
  soilMoisturePercentAverage değerini döndür ;
}

geçersiz  veri al () {
  statik  char endMarker = ' \n ' ;
  char alındıChar;
  int ndx = 0 ;
  // veri arabelleğini temizle
  memset (veri, 32 , sizeof (veri));
  (Seri.mevcut ( ) > 0 ) iken {
    receivedChar = Serial.okuma ( );
    eğer (alınanKarakter == bitişİşaretleyicisi) {
      veri[ndx] = ' \0 ' ;
      geri dönmek ;
    }
    veri[ndx] = alınanKarakter;
    ndx++;
    eğer (ndx >= DATA_MAX_SIZE) {
      kırmak ;
    }
  }
  memset (veri, 32 , sizeof (veri));
}
 39 değişiklik: 39 ekleme ve 0 silme39 
komutlar/rain.js
Orijinal dosya satır numarası	Farklı satır numarası	Fark satırı değişikliği
@@ -0,0 +1,39 @@
const  { MessageEmbed }  =  require ( "discord.js" ) ;
sabit  yapılandırma  =  gerekli ( '../config.json' ) ;
sabit  yerelleştirme  =  gerekli ( '../yerelleştirme/  ' +  yapılandırma .yerelleştirme_dosyası ) ;

modül . ihracatlar . info  =  {
	"başlık" : yerelleştirme . komutlar . yağmur . başlık . değiştir ( "<bitki_adı> " ,  yapılandırma .bitki_adı ) ,
	"isim" : yerelleştirme . komutlar . yağmur . isim ,
	"desc" : yerelleştirme . komutlar . yağmur . desc . replace ( "<bitki_adı> " ,  config .bitki_adı ) ,
	"renk" : yerelleştirme . komutlar . yağmur . renk ,
	"alan" : yerelleştirme . komutlar . yağmur . alan ,
	"nem_düşük" : yerelleştirme . komutlar . yağmur . nem_düşük ,
	"nem_yüksek" : yerelleştirme . komutlar . yağmur . nem_yüksek ,
	"önerilen_nem" : yerelleştirme . komutlar . yağmur . önerilen_nem ,
	"sipariş" : 500
}

modül . ihracatlar . yürütme  =  ( istemci ,  mesaj )  =>  {
	eğer  ( ! config . rain_role . some ( rol  =>  mesaj . üye . roller . önbellek . sahip ( rol ) ) )  mesaj . yanıtla ( yerelleştirme . komutlar . rain . üye olmayan ) döndür 
	istemci . su_sayacı ++ ;
	istemci . yardımcılar . arduinoBridge . waterThePlant ( config . rain_command_pump_time ) ;
	sabit  yerleştir  =  yeni  MesajGömme ( )
		.setTitle ( bu .bilgi .başlık )​​​
		.setColor ( bu .bilgi .renk )​​​
		. altbilgiyi ayarla ( {
			metin : mesaj . üye . displayName ,
			iconURL : mesaj . yazar . displayAvatarURL ( {  dynamic : true  } ) ,
		} )
		.zamandamgasınıayarla ( ) ;​

	eğer  ( istemci.yardımcılar.arduinoKöprüsü.nem ( ) < config.nem_dakikası ) {​​​​​​​​   
		embed.addField ( this . info.field + " : % " + client.helpers.arduinoBridge.getMoisture ( ) , this . info.nem_düşük + " " + config.emoji_üzgün + " " + this . info.önerilen_nem + " : % " + config.nem_min + " - % " + config.nem_maksimum )​​​​​​​​​                     
	}  else  if  ( istemci . yardımcılar . arduinoBridge . getMoisture ( )  >  config . nem_maksimum )  {
		embed.addField ( this . info.field + " : % " + client.helpers.arduinoBridge.getMoisture ( ) , this . info.nem_yüksek + " " + config.emoji_üzgün + " " + this . info.önerilen_nem + " : % " + config.nem_min + " - % " + config.nem_maksimum )​​​​​​​​​                     
	}  başka  {
		embed . addField ( this . info . field  +  ": %"  +  client . helpers . arduinoBridge . getMoisture ( ) ,  config . emoji_happy )
	}

	mesaj . kanal . gönder ( {  göm : [ göm ]  } ) ;
}
  3 değişiklik: 2 ekleme ve 1 silme3 
komutlar/storm.js
Orijinal dosya satır numarası	Farklı satır numarası	Fark satırı değişikliği
@@ -14,11 +14,12 @@ module.exports.info = {
	"fan_already_on" : yerelleştirme . komutlar . fırtına . fan_already_on ,
	"fan_already_on_field" : yerelleştirme . komutlar . fırtına . fan_already_on_field ,
	"fan_speed" : yerelleştirme . komutlar . storm . fan_speed ,
	"sipariş" : 500
	"sipariş" : 501
}

modül . ihracatlar . yürütme  =  ( istemci ,  mesaj )  =>  {
	eğer  ( ! config.storm_rolü.some ( rol = > mesaj.üye.roller.önbellek.sahip ( rol ) ) ) mesaj.cevap ( yerelleştirme.komutlar.fırtına.üye olmayan ) döndür​​​​​​​​​​​​​​​​​​    
	istemci . rüzgar_sayacı ++ ;
	sabit  yerleştir  =  yeni  MesajGömme ( )
		.setTitle ( bu .bilgi .başlık )​​​
		.setColor ( bu .bilgi .renk )​​​
		. altbilgiyi ayarla ( {
			metin : mesaj . üye . displayName ,
			iconURL : mesaj . yazar . displayAvatarURL ( {  dynamic : true  } ) ,
		} )
		.zamandamgasınıayarla ( ) ;​
	eğer  ( istemci . fan_speed  <  100 )  {
		istemci . fan_speed  +=  config . storm_command_increase_percentage
		eğer  ( istemci.yardımcılar.arduinoKöprüsü.nem ( ) < config.nem_dakikası ) {​​​​​​​​   
			embed.addField ( this . info . field + " " + this . info . fan_speed + " : %" + client . fan_speed , this . info . humid_low + " " + config . emoji_sad + " " + this . info . recommended_moisture + ": %" + config . humid_min + " - %" + config . humid_max )                         
		}  else  if  ( istemci . yardımcılar . arduinoBridge . getMoisture ( )  >  config . nem_maksimum )  {
			embed.addField ( this . info . field + " " + this . info . fan_speed + " : %" + client . fan_speed , this . info . humid_high + " " + config . emoji_happy + " " + this . info . recommended_moisture + ": %" + config . humid_min + " - %" + config . humid_max )                         
		}  başka  {
			embed .addField ( this . info . field + " " + this . info . fan_speed + " : %" + client . fan_speed , config . emoji_happy )         
		}
	}  başka  {
		göm . addField ( this . info . fan_already_on  +  " "  +  this . info . fan_speed  +  ": %"  +  client . fan_speed ,  this . info . fan_already_on_field )
	}
	mesaj . kanal . gönder ( {  göm : [ göm ]  } ) ;
}
  2 değişiklik: 1 ekleme ve 1 silme2 
komutlar/su.js
Orijinal dosya satır numarası	Farklı satır numarası	Fark satırı değişikliği
@@ -15,7 +15,7 @@ module.exports.info = {

modül . ihracatlar . yürütme  =  ( istemci ,  mesaj )  =>  {
	istemci . su_sayacı ++ ;
	istemci . yardımcılar . arduinoBridge . waterThePlant ( ) ;
	istemci . yardımcılar . arduinoBridge . waterThePlant ( config . water_command_pump_time ) ;
	sabit  yerleştir  =  yeni  MesajGömme ( )
		.setTitle ( bu .bilgi .başlık )​​​
		.setColor ( bu .bilgi .renk )​​​
		. altbilgiyi ayarla ( {
			metin : mesaj . üye . displayName ,
			iconURL : mesaj . yazar . displayAvatarURL ( {  dynamic : true  } ) ,
		} )
		.zamandamgasınıayarla ( ) ;​
	eğer  ( istemci.yardımcılar.arduinoKöprüsü.nem ( ) < config.nem_dakikası ) {​​​​​​​​   
		embed.addField ( this . info.field + " : % " + client.helpers.arduinoBridge.getMoisture ( ) , this . info.nem_düşük + " " + config.emoji_üzgün + " " + this . info.önerilen_nem + " : % " + config.nem_min + " - % " + config.nem_maksimum )​​​​​​​​​                     
	}  else  if  ( istemci . yardımcılar . arduinoBridge . getMoisture ( )  >  config . nem_maksimum )  {
		embed.addField ( this . info.field + " : % " + client.helpers.arduinoBridge.getMoisture ( ) , this . info.nem_yüksek + " " + config.emoji_üzgün + " " + this . info.önerilen_nem + " : % " + config.nem_min + " - % " + config.nem_maksimum )​​​​​​​​​                     
	}  başka  {
		embed . addField ( this . info . field  +  ": %"  +  client . helpers . arduinoBridge . getMoisture ( ) ,  config . emoji_happy )
	}
	mesaj . kanal . gönder ( {  göm : [ göm ]  } ) ;
}
  4 değişiklik: 4 ekleme ve 0 silme4 
yapılandırma.örnek.json
Orijinal dosya satır numarası	Farklı satır numarası	Fark satırı değişikliği
@@ -1,5 +1,6 @@
{
  "bitki_adı" : " BitkiAdıBurada " ,
  "bitki_türü" : " BitkiTürüBurada " ,
  "nem_dakika" : 15 ,
  "nem_maksimum" : 20 ,
  "önek" : " ! " ,
@@ -23,11 +24,14 @@
  "fotoğraf_çekme_aralığı_günlük_saat" : 14 ,
  "arduino_portu" : " " ,
  "fırtına_rolü" : [ " 165911303720796160 " , " 936685978947567698 " ],
  "yağmur_rolü" : [ " 165911303720796160 " , " 936685978947567698 " ],
  "emoji_mutlu" : " 😊 " ,
  "emoji_üzgün" : " 🥺 " ,
  "fan_azalma_zaman_aralığı" : 1000 ,
  "rüzgar_komutu_artış_yüzdesi" : 10 ,
  "storm_command_increase_percentage" : 25 ,
  "su_komut_pompası_zamanı" : 20 ,
  "yağmur_komutu_pompa_zamanı" : 40 ,
  "fan_speed_min_value" : 100 ,
  "fan_hızı_maksimum_değeri" : 255 ,
  "fan_speed_send_arduino_time_interval" : 5000
}
 1 değişiklik: 1 ekleme ve 0 silme1 
sayaç_veri.csv
Orijinal dosya satır numarası	Farklı satır numarası	Fark satırı değişikliği
@@ -1 +1,2 @@
su,rüzgar
3,2
  4 değişiklik: 2 ekleme ve 2 silme4 
yardımcılar/arduinoBridge.js
Orijinal dosya satır numarası	Farklı satır numarası	Fark satırı değişikliği
@@ -30,9 +30,9 @@ modül.dışa aktarmalar.getFanState = () => {
}


modül . ihracatlar . suThePlant  =  ( )  =>  {
modül . ihracatlar . suThePlant  =  ( pompa_süresi )  =>  {
	eğer  ( port  !=  null )  {
		port . write ( ' wtr \n' ,  ( hata )  =>  {
		port . yaz ( ' w' + pompa_süresi + ' \n' , ( hata ) => {       
			eğer  ( hata )  {
				return  console .log ( 'Arduino portuna yazarken hata oluştu: ' , err . message ) ; 
			}
			//console.log('Sulama tesisi mesajı gönderildi');
		} ) ;
	}
}
modül . ihracatlar . fanspeed  =  ( fan_speed )  =>  {
	//console.log("arduino'ya düşünüldü");
	//console.log(fanSpeedMap(fan_speed));
	eğer  ( port  !=  null )  {
		port . yaz ( 'f'  +  fanSpeedMap ( fan_speed )  +  '\n' ,  ( hata )  =>  {
			eğer  ( hata )  {
				return  console .log ( 'Arduino portuna yazarken hata oluştu: ' , err . message ) ; 
			}
		} ) ;
	}
}
fonksiyon  fanSpeedMap ( yüzde_fan_hızı )  {
	fanSpeedMapped  =  config.fan_speed_min_value + ( yüzde_fan_speed / 100 * ( config.fan_speed_max_value - config.fan_speed_min_value ) ) ;​ ​​​​​       
  12 değişiklik: 12 ekleme ve 0 silme12 
yerelleştirme/en.json
Orijinal dosya satır numarası	Farklı satır numarası	Fark satırı değişikliği
@@ -27,6 +27,18 @@
            "moisture_high" : " Lütfen beni bir daha sulamayın, beni öldürebilirsiniz. " ,
            "recommended_moisture" : " Önerilen nem " ,
            "nem" : " Nem "
        },
		"yağmur" : {
            "title" : " Biraz yağmur yarattın! 💧💧 " ,
            "isim" : " yağmur " ,
            "desc" : " Bana iki damla su ver. (Sadece X rolü için) " ,
            "renk" : " MAVİ " ,
            "tarla" : " Toprak nemi " ,
            "moisture_low" : " Bana biraz daha su verebilir misiniz? Toprağım çok kuru. " ,
            "moisture_high" : " Lütfen beni bir daha sulamayın, beni öldürebilirsiniz. " ,
            "recommended_moisture" : " Önerilen nem " ,
			"non_member" : " Bu fonksiyonu kullanmak için youtube üyeliğinizin olması gerekir " ,
            "nem" : " Nem "
        },
        "rüzgâr" : {
            "title" : " Rüzgar hızını artırıyorsunuz💨 " ,
            "isim" : " rüzgar " ,
            "desc" : " Fan hızını %<fan_speed> kadar artırmak için. Bu, topraktaki suyun buharlaşmasına yardımcı olabilir. " ,
            "renk" : " KIRMIZI " ,
            "alan" : " Fan hızı %<fan_speed> artırıldı " ,
            "moisture_high" : " Teşekkürler! Toprağım çok nemli, biraz rüzgar faydalı olabilir! " ,
            "moisture_low" : " Toprağım zaten çok kuru, keşke rüzgar olmasaydı. " ,
            "recommended_moisture" : " Önerilen nem " ,
            "fan_already_on" : " Fan zaten maksimum hızda. " ,
            "fan_already_on_field" : " Önce %100'ün altına düşmesini bekleyin. " ,
            "wind_stopped" : " Rüzgar durdu. " ,
            "fan_speed" : " Fan Hızı "
        },
        "fırtına" : {
            "title" : " Bir fırtına yarattın 💨 " ,
            "isim" : " fırtına " ,
            "desc" : " Fan hızını %<fan_speed> kadar artırmak için. (Sadece X rolü için) " ,
            "renk" : " KIRMIZI " ,
            "alan" : " Fan hızı %<fan_speed> artırıldı " ,
            "moisture_high" : " Teşekkürler! Toprağım çok nemli, biraz rüzgar faydalı olabilir! " ,
            "moisture_low" : " Toprağım zaten çok kuru, keşke rüzgar olmasaydı. " ,
            "recommended_moisture" : " Önerilen nem " ,
            "fan_already_on" : " Fan zaten maksimum hızda. " ,
            "fan_already_on_field" : " Önce %100'ün altına düşmesini bekleyin. " ,
            "non_member" : " Bu fonksiyonu kullanmak için youtube üyeliğinizin olması gerekir " ,
            "storm_stopped" : " Fırtına durdu. " ,
            "fan_speed" : " Fan Hızı "
        },
        "ping" : {
            "başlık" : " Pong! 🏓 " ,
            "isim" : " ping " ,
            "desc" : " Masa tenisi " ,
            "renk" : " SARI " ,
            "latencyfield" : " Gecikme " ,
            "apifield" : " API Gecikmesi "
        },
        "çalışma süresi" : {
            "başlık" : " Çalışma Süresi " ,
            "isim" : " çalışma süresi " ,
            "desc" : " Çalışma Süresi Alınıyor " ,
            "renk" : " KIRMIZI " ,
            "alan" : " Çalışma Süresi : "
        },
        "fotoğraf" : {
            "title" : " İşte bir selfie 📷 " ,
            "isim" : " fotoğraf " ,
            "desc" : " Resmimi almak için " ,
            "renk" : " GRİ "
        },
        "bilgi" : {
            "başlık" : " Bilgi 🌱 " ,
            "isim" : " bilgi " ,
            "desc" : " Mevcut toprak nemi için " ,
            "renk" : " YEŞİL " ,
            "field0" : " Mevcut toprak nemi " ,
            "field1" : " Önerilen nem " ,
            "field2" : " Hayran durumu " ,
            "field3" : " Deneyin uzunluğu " ,
            "time_field" : " deneyin başlangıcından bu yana geçen gün sayısı " ,
            "fan_on" : " Açık " ,
            "fan_off" : " Kapalı "
        } ,
        "grafik" : {
            "başlık" : " Grafik " ,
            "isim" : " grafik " ,
            "desc" : " Ayrıntılı bilgi grafiği " ,
            "renk" : " MAVİ " ,
            "field1" : " Sulama sayısı " ,
            "field2" : " Nem ortalaması " ,
            "time_field" : " deneyin başlangıcından bu yana geçen gün sayısı "
        },
        "not_found" : " Bu komut mevcut değil "
    }
}
  12 değişiklik: 12 ekleme ve 0 silme12 
yerelleştirme/tr.json
Orijinal dosya satır numarası	Farklı satır numarası	Fark satırı değişikliği
@@ -27,6 +27,18 @@
            "moisture_high" : " Lütfen bana daha çok su verme. Toprağım çok nemli, beni öldürebilirsin. " ,
            "recommended_moisture" : " önerilen nem " ,
            "nem" : " Nem "
        },
		"yağmur" : {
            "title" : " Ufak bir yağmur başladı! 💧💧 " ,
            "isim" : " yağmur " ,
            "desc" : " Bana iki damla su vermek için. (YT'ye katıl Üyelerne özel) " ,
            "renk" : " MAVİ " ,
            "alan" : " Toprak Nemi " ,
            "moisture_low" : " Bana biraz daha su verir yanlış mı? Toprağım çok kuru. " ,
            "moisture_high" : " Lütfen bana daha çok su verme. Toprağım çok nemli, beni öldürebilirsin. " ,
            "recommended_moisture" : " önerilen nem " ,
			"non_member" : " Bunu kesmek için YouTube kanalına üye olmalısınız. Değilseniz su durumunda kullanabilirsiniz. " ,
            "nem" : " Nem "
        },
        "rüzgâr" : {
            "title" : " Burada biraz eskimeye başladı 💨 " ,
            "isim" : " rüzgar " ,
            "desc" : " Fanların %<fan_speed> katmanı için. (saniyede %1 azalacaktır) " ,
            "renk" : " KIRMIZI " ,
            "field" : " Fanların %<fan_speed> arttırılmasının! " ,
            "moisture_high" : " Teşekkürler! Toprağım çok nemli, biraz rüzgarın yardımı dokunabilir. " ,
            "moisture_low" : " Toprağım zaten çok kuru, keşke hiç rüzgar içermez... " ,
            "recommended_moisture" : " önerilen nem " ,
            "fan_already_on" : " Fan hızını %100'ün üzerindeyken daha yükseltemezsiniz. " ,
            "fan_already_on_field" : " Fan hızının azalmasını bekleyin. " ,
            "wind_stopped" : " Rüzgar durdu. " ,
            "fan_speed" : " Fan hızı "
        },
        "fırtına" : {
            "title" : " Buradaki baya esmeye başladı 💨💨💨 " ,
            "isim" : " fırtına " ,
            "desc" : " Fanların %<fan_speed> boyutu için. (YT katılım üyelerine özel) " ,
            "renk" : " KIRMIZI " ,
            "field" : " Fanların %<fan_speed> arttırılmasının! " ,
            "moisture_high" : " Teşekkürler! Toprağım çok nemli, biraz rüzgarın yardımı dokunabilir. " ,
            "moisture_low" : " Toprağım zaten çok kuru, keşke hiç rüzgar içermez... " ,
            "recommended_moisture" : " önerilen nem " ,
            "fan_already_on" : " Fan hızını %100'ün üzerindeyken daha yükseltemezsiniz. " ,
            "fan_already_on_field" : " Fan hızının azalmasını bekleyin. " ,
            "non_member" : " Bunu kesmek için YouTube kanalına üye olmalısınız. Değilseniz rüzgar durumunda kullanabilirsiniz. " ,
            "storm_stopped" : " Fırtına durdu. " ,
            "fan_speed" : " Fan hızı "
        },
        "ping" : {
            "başlık" : " Pong! 🏓 " ,
            "isim" : " ping " ,
            "desc" : " Masa tenisi " ,
            "renk" : " SARI " ,
            "latencyfield" : " Gecikme " ,
            "apifield" : " API Gecikmesi "
        },
        "çalışma süresi" : {
            "title" : " Çalışma Süresi " ,
            "isim" : " çalışma süresi " ,
            "desc" : " Çalışma süresi bilgileri için. " ,
            "renk" : " KIRMIZI " ,
            "alan" : " Çalışma Süresi : "
        },
        "fotoğraf" : {
            "title" : " Merhaba! 📷 " ,
            "isim" : " fotoğraf " ,
            "desc" : " Selfie'yi çekmek için. " ,
            "renk" : " GRİ "
        },
        "bilgi" : {
            "başlık" : " Veriler 🌱 " ,
            "isim" : " bilgi " ,
            "desc" : " Şu anki toprakları öğrenmek için. " ,
            "renk" : " YEŞİL " ,
            "field0" : " Şu anki toprak nem oranı " ,
            "field1" : " önerilen nem aralığı " ,
            "field2" : " Fan hızı " ,
            "field3" : " Deney Uzunluğu " ,
            "time_field" : " oranları devam ediyor. " ,
            "fan_on" : " Açık " ,
            "fan_off" : " Kapalı "
        },
        "grafik" : {
            "başlık" : " Grafik " ,
            "isim" : " grafik " ,
            "desc" : " Detaylı durum grafiği " ,
            "renk" : " MAVİ " ,
            "field1" : " Sulama Sayıları " ,
            "field2" : " Nem mesafeleri " ,
            "time_field" : " oranları devam ediyor. "
        },
        "not_found" : " Böyle bir komut hatası oluştu! "
    }
}  
