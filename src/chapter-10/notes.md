# Ders Notları

```
(define-record-procedures yılan
  yılan-oluştur
  (yılan-x
   yılan-y
   yılan-yön
   yem-x
   yem-y))

(yılan-oluştur 0 0 "sağ" 50 60)
(yılan-oluştur 100 50 "sol" 20 50)
(yılan-oluştur 80 210 "yukarı" 130 70)
(yılan-oluştur 0 75 "aşağı" 20 50)
```

```
;; Bir yılan ve klavyeden basılan tuşu berlirten bir string alır. Eğer basılan tuş yön tuşlarından
;; biri ise yılanın yönünü uygun bir şekilde değiştirir:
;; "up" -> "yukarı"
;; "down" -> "aşağı"
;; "left" -> "sol"
;; "right" -> "sağ"

(: yön-değiştir (yılan string -> yılan))

(check-expect (yön-değiştir (yılan-oluştur 0 0 "sağ" 0 0) "up")  (yılan-oluştur 0 0 "yukarı" 0 0))
(check-expect (yön-değiştir (yılan-oluştur 100 50 "yukarı" 0 0) "left")  (yılan-oluştur 100 50 "sol" 0 0))
(check-expect (yön-değiştir (yılan-oluştur 80 20 "sol" 0 0) "down")  (yılan-oluştur 80 20 "aşağı" 0 0))
(check-expect (yön-değiştir (yılan-oluştur 10 90 "aşağı" 0 0) "right")  (yılan-oluştur 10 90 "sağ" 0 0))
(check-expect (yön-değiştir (yılan-oluştur 10 90 "aşağı" 0 0) "up")  (yılan-oluştur 10 90 "aşağı" 0 0))
(check-expect (yön-değiştir (yılan-oluştur 10 90 "yukarı" 0 0) "down")  (yılan-oluştur 10 90 "yukarı" 0 0))
(check-expect (yön-değiştir (yılan-oluştur 10 90 "sol" 0 0) "right")  (yılan-oluştur 10 90 "sol" 0 0))
(check-expect (yön-değiştir (yılan-oluştur 10 90 "sağ" 0 0) "left")  (yılan-oluştur 10 90 "sağ" 0 0))

(define yön-değiştir
  (lambda (yln tuş)
    (cond
      [(and (key=? tuş "left") (not (string=? (yılan-yön yln) "sağ"))) (yılan-oluştur (yılan-x yln) (yılan-y yln) "sol" (yem-x yln) (yem-y yln))]
      [(and (key=? tuş "right") (not (string=? (yılan-yön yln) "sol"))) (yılan-oluştur (yılan-x yln) (yılan-y yln) "sağ" (yem-x yln) (yem-y yln))]
      [(and (key=? tuş "up") (not (string=? (yılan-yön yln) "aşağı"))) (yılan-oluştur (yılan-x yln) (yılan-y yln) "yukarı" (yem-x yln) (yem-y yln))]
      [(and (key=? tuş "down") (not (string=? (yılan-yön yln) "yukarı"))) (yılan-oluştur (yılan-x yln) (yılan-y yln) "aşağı" (yem-x yln) (yem-y yln))]
      [(key=? tuş "r") (yılan-oluştur 50 50 "sağ" (yem-x yln) (yem-y yln))]
      [else yln])))
```

```
;; Bir yılan alır ve 200x200 boyutlarında boş bir sahneye yılanın x ve y koordinatlarına gelecek sekilde
;; 10x10 boyutlarında siyah içi dolu bir kare çizer.

(: çiz (yılan -> image))

(check-expect (çiz (yılan-oluştur 100 100 "sol" 50 50)) (place-image (rectangle 10 10 "solid" "red") 50 50 (place-image (rectangle 10 10 "solid" "black") 100 100 (empty-scene 200 200))))

(define çiz
  (lambda (yln)
    (place-image
     (rectangle 10 10 "solid" "red")
     (yem-x yln) (yem-y yln)
     (place-image 
      (rectangle 10 10 "solid" "black")
      (yılan-x  yln) (yılan-y yln)
      (empty-scene 200 200)))))
```

```
;; Bir yılan alır ve yılanın yönüne uygun bir şekilde bir sonraki sahnede nerede olması gerektiğini hesaplar.
;; Bu hesap sonucunda oluşan yeni yılanı döner.

(: ilerle (yılan -> yılan))

(check-expect (ilerle (yılan-oluştur 100 100 "sağ" 0 0)) (yılan-oluştur 110 100 "sağ" 0 0))
(check-expect (ilerle (yılan-oluştur 100 100 "sol" 0 0)) (yılan-oluştur 90 100 "sol" 0 0))
(check-expect (ilerle (yılan-oluştur 100 100 "yukarı" 0 0)) (yılan-oluştur 100 90 "yukarı" 0 0))
(check-expect (ilerle (yılan-oluştur 100 100 "aşağı" 0 0)) (yılan-oluştur 100 110 "aşağı" 0 0))

(define ilerle
  (lambda (yln)
    (cond
      [(string=? (yılan-yön yln) "sağ") (yılan-oluştur (+ (yılan-x yln) 10) (yılan-y yln) (yılan-yön yln) (yem-x yln) (yem-y yln))]
      [(string=? (yılan-yön yln) "sol") (yılan-oluştur (- (yılan-x yln) 10) (yılan-y yln) (yılan-yön yln) (yem-x yln) (yem-y yln))]
      [(string=? (yılan-yön yln) "yukarı") (yılan-oluştur (yılan-x yln) (- (yılan-y yln) 10) (yılan-yön yln) (yem-x yln) (yem-y yln))]
      [(string=? (yılan-yön yln) "aşağı") (yılan-oluştur (yılan-x yln) (+ (yılan-y yln) 10) (yılan-yön yln) (yem-x yln) (yem-y yln))]
      [else yln])))
```

```
;; Bir yılanın alır ve koordinatlarını kontrol eder. Eger yılan sahnenin dışına çıktı ise #true döner. Aksi takdirde #false döner.

(: oyun-bitti? (yılan -> boolean))

(check-expect (oyun-bitti? (yılan-oluştur 100 100 "sağ" 0 0)) #f)
(check-expect (oyun-bitti? (yılan-oluştur 201 100 "sağ" 0 0)) #t)
(check-expect (oyun-bitti? (yılan-oluştur 100 201 "sağ" 0 0)) #t)
(check-expect (oyun-bitti? (yılan-oluştur 300 100 "sol" 0 0)) #t)

(define oyun-bitti?
  (lambda (yln)
    (cond
      [(< (yılan-x yln) 5) #t]
      [(> (yılan-x yln) 195) #t]
      [(< (yılan-y yln) 5) #t]
      [(> (yılan-y yln) 195) #t]
      [else #f])))
```

```
;; Bir yılan alır ve oyunun son sahnesini gösteren bir resim döner.

(: son-sahne (yılan -> image))

(check-expect (son-sahne (yılan-oluştur 201 100 "sağ" 0 0)) (place-image (text "Oyun Bitti!" 30 "red") 100 100 (empty-scene 200 200)))

(define son-sahne
  (lambda (yln)
    (place-image (text "Oyun Bitti!" 30 "red") 100 100 (empty-scene 200 200))))
```

```
;; 5,15,25...195 sayılarından birini rastgele olarak seçer ve döner.

(: rastgele integer)

(define rastgele (+ 5 (* (random 20) 10)))
```

```
;; Yılanı (50,100) noktasından yönü sağ tarafa doğru olacak sekilde yerleştirerek simulasyonu başlatır.

(big-bang
    (yılan-oluştur 5 5 "sağ" rastgele rastgele)
  (on-key yön-değiştir)
  (on-tick ilerle 0.1)
  (stop-when oyun-bitti? son-sahne)
  (to-draw çiz 200 200))
```