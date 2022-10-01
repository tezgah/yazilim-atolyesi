# Ders 1

## DrRacket: Kurulum ve Ayarlar

Ilk olarak asagidaki baglantiyi kullanarak `DrRacket` programini indirelim, kuralim ve ayarlarini yapalim:

```
https://download.racket-lang.org/
```

DrRacket istenirse `Almanca` olarak da kullanilabilir.

```
Help > Deutsche Benutzeroberflache fur DrRacket > In Ordnung - Beenden
```

Bu ayari yaptiktan sonra DrRacket programini yeniden baslatmaniz gerekmektedir.

DrRacket, programlamaya giris egitimini kolaylastirmak icin bizlere yardimci diller ve egitim paketleri sunar. Simdi bu ayarlari yapalim:

```
Language > Choose Language > Die Macht der Abstraktion - Anfänger
Language > Add Teachpack > Image2.ss
```

Dil ve egitim paketi sectikten sonra `Run` tusuna basarak yaptigimiz degisiklikleri kullanima alalim.

DrRacket 2 ayri pencereden olusur. Bunlar `Definition Window` (Tanimlama Penceresi) ve `Interaktion Window` (Etkilesim Penceresi) olarak adlandirilir.

Etkilesim penceresini kullanarak Racket dilini tanimaya baslayalim.

## Basit Hesaplamalar

Etkilesim penceresini bir hesap makinasi gibi dusunebilirsiniz. Haydi ona basit bir seyler soralim:

```racket
42
```

Ya da:

```racket
3.141592653
```

Daha karisik bir soru:

```racket
(+ 40 2)
```

Yukarida farkinda olmadan ilk fonksiyon cagiriminizi yapmis oldunuz. `+` fonksiyonun adi, `40` ve `2` parametreleri.

> Kural:
> Fonksiyonlar parantez icinde yazilir.

> Kural:
> Fonksiyon adi parametrelerden once yazilir. (Prefix Notation)

> Kural:
> Fonksiyon adi ve parameteleri arasina bosluk karakteri yazilir.
> (function argument1 argument2 argument3 ...)

[//]: # (TODO: Expression [Ausdruck] vs Evaluation [Auswertung])

## Daha Karmasik Hesaplamalar

Ic ice gecmis, daha kompleks sorular da sorabiliriz.

```racket
(odd? (+ 40 2))
(+ (+ 20 20) (+ 1 1))
```

> Kural:
> Hesaplar iceriden disariya dogru yapilir. En once en icteki parantezin degeri hesaplanir. Daha sonra cikan sonuc ile hesaba devam edilir.

Birlikte hasaplayalim:

```racket
5 + 10 / 2 - 2 * 4
```

Matematikte kural olarak carpma ve bolme islemi toplama ve cikarmadan once yapilir:

```racket
5 + (10 / 2) - (2 * 4)
```

Yukaridaki hesaplamayi `Racket` dilinde yazmak istersek:

```racket
(- (+ 5 (/ 10 2)) (* 2 4)) # ya da
(* 5 (- (/ 10 2) (* 2 4)))
```

## Sadece Sayilar mi var?

Racket dilinin yapi taslari arasinda sayilardan baska `turler` de var. 

Mesela asagidaki bir `Metin` (String):

```racket
"Fatih"
```

`odd` ingilizce de **tek sayi**lari ifade etmek icin kullaniliyor. `odd?` fonksiyonu ise Racket dilinde eger verilen sayi tek ise `#t` (True: yani dogru), cift ise `#f` (False: yani yanlis) degerini donuyor. `#t` ve `#f` degerlerini `Boolean` (mantiksal degerler) denir.

```racket
(odd? 42)
```

Racket'da bulabileceginiz bir baska deger ise `Resim` (Image). Asagidaki ifadeyi calistirirsaniz size 50 yaricapinda, ici dolu, kirmizi bir daire resmi uretir.

```racket
(circle 50 "solid" "red")
```

> IPUCU: DrRacket etkilesim penceresinde `ESC+P` tus kombinasyonu ile en son yazdiginiz satiri geri cagirabilirsiniz.

Iste bunlara `Literals` (Literale) ya da `Constants` diyoruz. Yani Yapıtaşları. En basit değerler. Daha fazla basitleştirilemezler.

| Literal |  | Signature |
|:---|---|---|
| #t #f | Mantiksal degerler (dogru, yanlis) | boolean |
| "Fatih" "x" " " | Yazi | string |
| 0 1983 -42 007 | Tam Sayilar | integer | 
| 0.42 3.1415 -273.15 | Virgullu sayilar | rational |
| &#x2730; | Resimler | image |


String turunu kullanarak da hesaplamalar yapabiliriz:

```racket
(string-append "Merhaba" "Fatih")
(string-append "Merhaba" " " "Fatih")
(string-length "Fatih")
(string->number "42")
```

Ya da resim kullanarak:

```racket
(circle 20 "solid" "red")
(rectangle 100 50 "solid" "blue")
(above (circle 20 "solid" "red") (rectangle 100 50 "solid" "blue"))
```

## Ifadeleri isimlendirme: define

Yazdigimiz ifadelere bir isim vererek onlari daha sonra kullanabiliriz:

```racket
(define pi 3.141592653)
(define fatih-tel "+49 111222333")
(define mavi-daire (circle 100 "solid" "blue"))
(define gun-dakika (* 24 60))
```

`(define ... ...)` ozel bir form. Bu form hesaplanan bir sey degil, bir efekti var. Ikinci parametredeki ifadeyi birinci parametredeki isme bagliyor.

```racket
(define isim ifade)
```

> Kural:
> isimler ( ) [ ] { } " , ' ` ; # | \ karakterleri ile baslayamaz.

> Kural: 
> isimler bir sayi ile esit olamaz

> Kural:
> Isimler space, tab, return karakterleri iceremez.

> Kural:
> daha once baska bir degere baglanmis olmamali.

> Kural: Buyuk/kucuk karakter farketmiyor.

Asagidaki ornegin yukaridaki kurallara uyup uymadigina inceleyelim:

```racket
(define eu->us$ 1.02)
```

## Alistirmalar

Simdi ogrendiklerimizi kullanarak Almanya'nin bayragini yapmaya calisalim. Siyah, kirmizi ve altin rengi olmak uzere 3 tane dikdortgene ihtiyacimiz var. Bunlari ust uste koyarsak oldu demektir:

```racket
(define siyah-dikdortgen (rectangle 500 100 "solid" "black"))
(define kirmizi-dikdortgen (rectangle 500 100 "solid" "red"))
(define altin-dikdortgen (rectangle 500 100 "solid" "gold"))
(define alman-bayragi (above siyah-dikdortgen kirmizi-dikdortgen altin-dikdortgen))
```

Bu tanimlamalari yaptiktan sonra `alman-bayragi` isimli ifadeyi calistirirsak sonucu gorebiliriz:

```racket
alman-bayragi
```

## Fonksiyonlar (Prosedurler): lambda

Simdi birlikte elektrik faturamizi hesaplayabilmek icin bir formul yazalim. Benim kullandigim elektrik sirketinin fiyatlandirma kurali su sekilde:

```racket
Kullanim Bedeli (Arbeitspreis) = 17,45 ct/kWh
Taban Fiyat (Grundpreis)       = 10,16 €/Monat
```

Bu sirket taban fiyat olarak aylik 10,16 Euro aliyor. Bunun uzerine kullandigimiz her kWh elektrik icin 17.45 Cents oduyoruz. Matematiksel olarak ifade etmek gerekirse;

```
Aylik Fatura = 10.16 + ((17.45 * KULLANIM-MIKTARI) / 100))
```

Ornegin, aylik 350 kWh elektrik kullanildiginda;

```
17.45 * 350 = 6107.5 Cents
6107.5 Cents / 100 = 61.075 Euro
61.075 + 10.16 = 71.235 Euro
```

Bunu `Racket` dilinde yazalim:

```racket
(+ 10.16 (/ (* 17.45 KULLANIM-MIKTARI) 100))
```

Bu ifadeyi bu calistirabilmek icin `KULLANIM-MIKTARI` yazan yere gercek degerler vermemiz gerekecek. Ornegin, yine 350 kWh elektrik kullanildigini varsayarsak:

```racket
(+ 10.16 (/ (* 17.45 350) 100))
```

... sonucun 71.235 oldugunu gorebiliriz.

Peki bu ifadeyi icinde degisken olacak sekilde kullanmak istesek? Yani kullanilan elektrik miktari degistikce hesaplayabilen bir fonksiyon yazmak istesek. `lambda` fonksiyonu tam da bu is icin var:

```racket
(lambda ... ...)
```

`lambda` birinci parametrede verdigimiz degisken isimlerini ikinci parametredeki ifadece yerine koyarak bize bir fonksiyon (prosedur) geri doner.

```racket
(lambda (x) (+ x 1))
```

Uretilen bu fonksiyona `define` kullanrak bir isim verebiliriz.

```racket
(define bir-ekle (lambda (x) (+ x 1)))
```

Ornegimize geri donecek olursak:

```racket
(define fatura-hesapla
  (lambda (x)
    (+ 10.16 (/ (* 17.45 350) 100))))
```

## Kaynaklar

Bu dersi daha iyi anlayabilmek icin asagidaki kaynaklardan faydalanabilirsiniz:

- Kitap (Almanca): [Schreibe Dein Programm!](https://www.deinprogramm.de/sdp/) Sayfa 40'a kadar.
- Video (Almanca): [DrRacket, REPL, Auswertung, Literale, komplexe Ausdrücke](https://www.youtube.com/watch?v=96QmmOUEduM)
- Video (Almanca): [Spezialform define, Identifier, Definitionsfenster](https://www.youtube.com/watch?v=_n6HZkiC3aM)
- Video (Almanca): [Lambda-Abstraktion, Funktionsdefinition, Applikation](https://www.youtube.com/watch?v=vwdEO0hzTGg)