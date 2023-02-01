# Ev Ödevi

1. Bir image listesi alan ve listedeki tüm image'ları üste üste koyarak yeni bir image oluşturan bir fonksiyon yazalım. (İpucu: Derste yaptığımız string listesi alarak tüm stringleri birleştiren fonksiyona benzer bir çözümü var).

2. Bir sayı listesi ve bir sayı alan, listedeki sayıları tek tek ikinci parametrede aldığı sayıya ekleyen bir fonsiyon yazalım.
   Örnekler:
   - [] 0 -> 0
   - [1] 0 -> 1
   - [2, 5] 4 -> 11


3. Bir nokta listesi alan ve listedeki noktaları toplayıp toplamı yeni bir nokta olarak dönen bir fonksiyon yazalım. (Hatırlatma: İki noktanın toplamı x ve y koordinatlarının ayrı ayrı toplanması ile elde edilir. (x1, y1) + (x2, y2) = (x1 + x2, y1 + y2).
  Örnekler:
  - [] - (0, 0)
  - [(1, 4)] -> (1, 4)
  - [(1, 3), (3, 6)] -> (4, 9)


```sh
(define-record-procedures nokta
  make-nokta
  nokta?
  (nokta-x
   nokta-y))
```

4. Bir `boolean` listesi alan ve eğer tüm değerler `#t` ise `#t`, herhangi bir değer `#f` ise `#f` dönen bir fonksiyon yazalım.
  Örnekler:
  - [] -> #t
  - [#t] -> #t
  - [#f] -> #f
  - [#t, #t] -> #t
  - [#t, #f] -> #f