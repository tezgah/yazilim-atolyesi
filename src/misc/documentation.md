# Dokümantasyon

## above

```racket
(above i1 i2 is ...) → image?
  i1 : image?
  i2 : image?
  is : image?
```

Verilen tüm resimleri merkezleri boyunca hizalanmış dikey bir sıraya yerleştirerek yeni bir resim oluşturur.

## circle

```racket
(circle yarıçap mod renk) → image?
  yarıçap : (and/c real? (not/c negatıve?))
  mod : mode?
  renk : image-color?
```

Verilen yarıçap, mod ve renk argümanlarını kullanarak bir daire oluşturur.

Mod, `"solid"` ya da `"outline"` değerlerinden biri olabilir.

Renk isimleri için büyük/küçük harf farketmez. "black" ve "Black" aynı renktir. Ayrıca boşluk karakteri de önemsenmez. Yani "light red" ve "lightred" aynı renktir. Kullanabileceğiniz tüm renklerin listesi için [buraya](https://docs.racket-lang.org/draw/color-database___.html) bakabilirsiniz.

## overlay

```racket
(overlay i1 i2 is ...) → image?
  i1 : image?
  i2 : image?
  is : image?
```

Verilen tüm resimleri üst üste koyarak tek bir resim oluşturur. Birinci resim ikincinin üzerine, o da üçüncünün üzerine ... şeklinde devam eder. Tüm resimler orta noktalarından sabitlenir.

## overlay/xy

```racket
(overlay/xy i1 x y i2) → image?
  i1 : image?
  x : real?
  y : real?
  i2 : image?
```

i1'i i2'nin üstüne yerleştirerek bir `image` oluşturur. Görüntüler başlangıçta sol üst köşelerinden sabitlenir ve ardından i2, x piksel sağa ve y piksel aşağı kaydırılır.

## place-image

```racket
(place-image image x y scene) → image?

  image : image?
  x : real?
  y : real?
  scene : image?
  ```

  Görüntüyü sahneye, merkezi (x,y) koordinatlarında olacak şekilde yerleştirir ve ortaya çıkan görüntüyü sahneyle aynı boyutta olacak şekilde kırpar. Koordinatlar sahnenin sol üst köşesine göredir.

## rectangle

```racket
(rectangle genişlik yükseklik mod renk) → image?
  genişlik : (and/c real? (not/c negatıve?))
  yükseklik : (and/c real? (not/c negatıve?))
  mod : mode?
  renk : image-color?
```

Verilen genişlik, yükseklik, mod ve renk değerlerini kullanarak bir dikdörtgen oluşturur.

## star

```racket
(star kenar-uzunluğu mod renk) → image?
  kenar-uzunluğu : (and/c real? (not/c negatıve?))
  mod : mode?
  renk : image-color?
```

Beş noktalı bir yıldız oluşturur. Kenar uzunluğu argümanı, çevreleyen beşgenin kenar uzunluğunu belirler.
