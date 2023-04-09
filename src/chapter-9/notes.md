# Ders Notları

```
(define-record-procedures yılan
  yılan-oluştur
  (yılan-x
   yılan-y
   yılan-yön))

(yılan-oluştur 0 0 "sağ")
(yılan-oluştur 100 50 "sol")
(yılan-oluştur 80 210 "yukarı")
(yılan-oluştur 0 75 "aşağı")
```

```
;; Bir yılan ve klavyeden basılan tuşu berlirten bir string alır. Eğer basılan tuş yön tuşlarından
;; biri ise yılanın yönünü uygun bir şekilde değiştirir:
;; "up" -> "yukarı"
;; "down" -> "aşağı"
;; "left" -> "sol"
;; "right" -> "sağ"

(: yön-değiştir (yılan string -> yılan))

(check-expect (yön-değiştir (yılan-oluştur 0 0 "sağ") "up")  (yılan-oluştur 0 0 "yukarı"))
(check-expect (yön-değiştir (yılan-oluştur 100 50 "yukarı") "left")  (yılan-oluştur 100 50 "sol"))
(check-expect (yön-değiştir (yılan-oluştur 80 20 "sol") "down")  (yılan-oluştur 80 20 "aşağı"))
(check-expect (yön-değiştir (yılan-oluştur 10 90 "aşağı") "right")  (yılan-oluştur 10 90 "sağ"))
(check-expect (yön-değiştir (yılan-oluştur 10 90 "aşağı") "up")  (yılan-oluştur 10 90 "aşağı"))
(check-expect (yön-değiştir (yılan-oluştur 10 90 "yukarı") "down")  (yılan-oluştur 10 90 "yukarı"))
(check-expect (yön-değiştir (yılan-oluştur 10 90 "sol") "right")  (yılan-oluştur 10 90 "sol"))
(check-expect (yön-değiştir (yılan-oluştur 10 90 "sağ") "left")  (yılan-oluştur 10 90 "sağ"))

(define yön-değiştir
  (lambda (yln tuş)
    (cond
      [(and (key=? tuş "left") (not (string=? (yılan-yön yln) "sağ"))) (yılan-oluştur (yılan-x yln) (yılan-y yln) "sol")]
      [(and (key=? tuş "right") (not (string=? (yılan-yön yln) "sol"))) (yılan-oluştur (yılan-x yln) (yılan-y yln) "sağ")]
      [(and (key=? tuş "up") (not (string=? (yılan-yön yln) "aşağı"))) (yılan-oluştur (yılan-x yln) (yılan-y yln) "yukarı")]
      [(and (key=? tuş "down") (not (string=? (yılan-yön yln) "yukarı"))) (yılan-oluştur (yılan-x yln) (yılan-y yln) "aşağı")]
      [(key=? tuş "r") (yılan-oluştur 50 50 "sağ")]
      [else yln])))
```

```
;; Bir yılan alır ve 200x200 boyutlarında boş bir sahneye yılanın x ve y koordinatlarına gelecek sekilde
;; 10x10 boyutlarında siyah içi dolu bir kare çizer.

(: çiz (yılan -> image))

(check-expect (çiz (yılan-oluştur 100 100 "sol")) (place-image (rectangle 10 10 "solid" "black") 100 100 (empty-scene 200 200)))

(define çiz
  (lambda (yln)
    (place-image 
     (rectangle 10 10 "solid" "black")
     (yılan-x  yln) (yılan-y yln)
     (empty-scene 200 200))))
```

```
;; Bir yılan alır ve yılanın yönüne uygun bir şekilde bir sonraki sahnede nerede olması gerektiğini hesaplar.
;; Bu hesap sonucunda oluşan yeni yılanı döner.

(: ilerle (yılan -> yılan))

(check-expect (ilerle (yılan-oluştur 100 100 "sağ")) (yılan-oluştur 105 100 "sağ"))
(check-expect (ilerle (yılan-oluştur 100 100 "sol")) (yılan-oluştur 95 100 "sol"))
(check-expect (ilerle (yılan-oluştur 100 100 "yukarı")) (yılan-oluştur 100 95 "yukarı"))
(check-expect (ilerle (yılan-oluştur 100 100 "aşağı")) (yılan-oluştur 100 105 "aşağı"))

(define ilerle
  (lambda (yln)
    (cond
      [(string=? (yılan-yön yln) "sağ") (yılan-oluştur (+ (yılan-x yln) 5) (yılan-y yln) (yılan-yön yln))]
      [(string=? (yılan-yön yln) "sol") (yılan-oluştur (- (yılan-x yln) 5) (yılan-y yln) (yılan-yön yln))]
      [(string=? (yılan-yön yln) "yukarı") (yılan-oluştur (yılan-x yln) (- (yılan-y yln) 5) (yılan-yön yln))]
      [(string=? (yılan-yön yln) "aşağı") (yılan-oluştur (yılan-x yln) (+ (yılan-y yln) 5) (yılan-yön yln))]
      [else yln])))
```

```
;; Yılanı (50,100) noktasından yönü sağ tarafa doğru olacak sekilde yerleştirerek simulasyonu başlatır.
(big-bang
    (yılan-oluştur 50 100 "sağ")
  (on-key yön-değiştir)
  (on-tick ilerle)
  (to-draw çiz 200 200))
```