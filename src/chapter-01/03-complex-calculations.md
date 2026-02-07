# Daha Karmaşık Hesaplamalar

İç içe geçmiş, daha kompleks sorular da sorabiliriz.

```racket
(+ (+ 20 20) (+ 1 1))
```

> Kural:
> Hesaplama içeriden dışarıya doğru yapılır. İlk önce en içteki parantezin değeri hesaplanır. Daha sonra çıkan sonuç ile hesaba devam edilir.

Mesela, aşağıdaki matematiksel ifadeyi aşama aşama `racket` dilinde yazmaya çalışalım.

```racket
10 / 2 - 2 * 4
```

Matematikte kural olarak çarpma ve bölme işleminin önceliği vardır:

```racket
(10 / 2) - (2 * 4)
```

Daha sonra da toplama ve çıkarma işlemleri yapılır:

```
((10 / 2) - (2 * 4))
```

`racket` dilinde yazmak istersek:


```racket
(- (/ 10 2) (* 2 4)) 
```
