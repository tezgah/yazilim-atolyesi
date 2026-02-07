# Ders Notları

```
;; x ve y koordinatlarını tanımlar
(define-record koor
  koor-oluştur
  (koor-x number)
  (koor-y number))

(koor-oluştur 50 60)
(koor-oluştur 20 70)
```
```
;; Yılan oyunu için gerekli veri yapısını tanımlar
(define-record yılan
  yılan-oluştur
  (yılan-gövde (list-of koor))
  (yılan-yön string)
  (yem-x number)
  (yem-y number))

(yılan-oluştur (cons (koor-oluştur 100 20) empty)  "sağ" 50 60)
(yılan-oluştur (cons (koor-oluştur 100 50) (cons (koor-oluştur 100 20) empty)) "sol" 20 50)
(yılan-oluştur empty "yukarı" 130 70)
(yılan-oluştur (cons (koor-oluştur 100 50) (cons (koor-oluştur 100 20) (cons (koor-oluştur 0 75) empty))) "aşağı" 20 50)
```

```
;; Bir yılan ve klavyeden basılan tuşu berlirten bir string alır. Eğer basılan tuş yön tuşlarından
;; biri ise yılanın yönünü uygun bir şekilde değiştirir:
;; "up" -> "yukarı"
;; "down" -> "aşağı"
;; "left" -> "sol"
;; "right" -> "sağ"

(: yön-değiştir (yılan string -> yılan))

(check-expect (yön-değiştir (yılan-oluştur (cons (koor-oluştur 0 0) empty) "sağ" 0 0) "up")  (yılan-oluştur (cons (koor-oluştur 0 0) empty) "yukarı" 0 0))
(check-expect (yön-değiştir (yılan-oluştur (cons (koor-oluştur 0 0) empty) "yukarı" 0 0) "left")  (yılan-oluştur (cons (koor-oluştur 0 0) empty) "sol" 0 0))
(check-expect (yön-değiştir (yılan-oluştur empty "sol" 0 0) "down")  (yılan-oluştur empty "aşağı" 0 0))
(check-expect (yön-değiştir (yılan-oluştur empty "aşağı" 0 0) "right")  (yılan-oluştur empty "sağ" 0 0))
(check-expect (yön-değiştir (yılan-oluştur empty "aşağı" 0 0) "up")  (yılan-oluştur empty "aşağı" 0 0))
(check-expect (yön-değiştir (yılan-oluştur empty "yukarı" 0 0) "down")  (yılan-oluştur empty "yukarı" 0 0))
(check-expect (yön-değiştir (yılan-oluştur empty "sol" 0 0) "right")  (yılan-oluştur empty "sol" 0 0))
(check-expect (yön-değiştir (yılan-oluştur empty "sağ" 0 0) "left")  (yılan-oluştur empty "sağ" 0 0))

(define yön-değiştir
  (lambda (yln tuş)
    (cond
      [(and (key=? tuş "left") (not (string=? (yılan-yön yln) "sağ"))) (yılan-oluştur (yılan-gövde yln) "sol" (yem-x yln) (yem-y yln))]
      [(and (key=? tuş "right") (not (string=? (yılan-yön yln) "sol"))) (yılan-oluştur (yılan-gövde yln) "sağ" (yem-x yln) (yem-y yln))]
      [(and (key=? tuş "up") (not (string=? (yılan-yön yln) "aşağı"))) (yılan-oluştur (yılan-gövde yln) "yukarı" (yem-x yln) (yem-y yln))]
      [(and (key=? tuş "down") (not (string=? (yılan-yön yln) "yukarı"))) (yılan-oluştur (yılan-gövde yln) "aşağı" (yem-x yln) (yem-y yln))]
      [else yln])))
```

```
;; Bir yılan alır ve 200x200 boyutlarında boş bir sahneye yılanın x ve y koordinatlarına gelecek sekilde
;; 10x10 boyutlarında siyah içi dolu bir kare çizer.

(: çiz (yılan -> image))

(check-expect (çiz (yılan-oluştur empty "sol" 50 50)) (place-image (rectangle 10 10 "solid" "red") 50 50 (empty-scene 200 200)))
(check-expect (çiz (yılan-oluştur (cons (koor-oluştur 50 50) (cons (koor-oluştur 100 100) empty)) "sol" 50 50)) (place-image (rectangle 10 10 "solid" "red") 50 50 (place-image (rectangle 10 10 "solid" "black") 100 100 (empty-scene 200 200))))

(define çiz
  (lambda (yln)
    (place-image
     (rectangle 10 10 "solid" "red")
     (yem-x yln) (yem-y yln)
     (yılan-çiz (yılan-gövde yln)))))
```

```
;; Bir koor listesi alır ve 200x200 boyutlarinda bir sahneye çizer

(: yılan-çiz ((list-of koor)  -> image))

(check-expect (yılan-çiz empty) (empty-scene 200 200))
(check-expect (yılan-çiz (cons (koor-oluştur 50 50) empty)) (place-image (rectangle 10 10 "solid" "black") 50 50 (empty-scene 200 200)))
(check-expect (yılan-çiz (cons (koor-oluştur 50 50) (cons (koor-oluştur 40 50) empty))) (place-image (rectangle 10 10 "solid" "black") 40 50 (place-image (rectangle 10 10 "solid" "black") 50 50 (empty-scene 200 200))))

(define yılan-çiz
  (lambda (gövde)
    (cond
     [(empty? gövde) (empty-scene 200 200)]
     [else (place-image (rectangle 10 10 "solid" "black") (koor-x (first gövde)) (koor-y (first gövde)) (yılan-çiz (rest gövde)))])))
```

```
;; Bir liste alir ve son elemanını silerek yeni bir liste döner

(: son-elemanı-sil ((list-of koor) -> (list-of koor)))

(check-expect (son-elemanı-sil empty) empty)
(check-expect (son-elemanı-sil (cons (koor-oluştur 100 100) empty)) empty)
(check-expect (son-elemanı-sil (cons (koor-oluştur 100 100) (cons (koor-oluştur 110 100) empty))) (cons (koor-oluştur 100 100) empty))

(define son-elemanı-sil
  (lambda (lst)
    (cond
     [(empty? lst) empty]
     [(empty? (rest lst)) empty]
     [else (cons (first lst) (son-elemanı-sil (rest lst)))])))
```

```
;; Bir yılan alır ve yılanın yönüne uygun bir şekilde bir sonraki sahnede nerede olması gerektiğini hesaplar.
;; Bu hesap sonucunda oluşan yeni yılanı döner.

(: ilerle (yılan -> yılan))

(check-expect (ilerle (yılan-oluştur empty "sağ" 0 0)) (yılan-oluştur empty "sağ" 0 0))
(check-expect (ilerle (yılan-oluştur (cons (koor-oluştur 100 100) empty) "sağ" 0 0)) (yılan-oluştur (cons (koor-oluştur 110 100) empty) "sağ" 0 0))
(check-expect (ilerle (yılan-oluştur (cons (koor-oluştur 100 100) (cons (koor-oluştur 90 100) empty)) "sağ" 0 0)) (yılan-oluştur (cons (koor-oluştur 110 100) (cons (koor-oluştur 100 100) empty)) "sağ" 0 0))

(define ilerle
  (lambda (yln)
    (cond
      [(empty? (yılan-gövde yln)) yln]
      [(string=? (yılan-yön yln) "sağ") (yılan-oluştur (cons (koor-oluştur (+ (koor-x (first (yılan-gövde yln))) 10)  (koor-y (first (yılan-gövde yln)))) (son-elemanı-sil (yılan-gövde yln))) (yılan-yön yln) (yem-x yln) (yem-y yln))]
      [(string=? (yılan-yön yln) "sol") (yılan-oluştur (cons (koor-oluştur (- (koor-x (first (yılan-gövde yln))) 10)  (koor-y (first (yılan-gövde yln)))) (son-elemanı-sil (yılan-gövde yln))) (yılan-yön yln) (yem-x yln) (yem-y yln))]
      [(string=? (yılan-yön yln) "yukarı") (yılan-oluştur (cons (koor-oluştur (koor-x (first (yılan-gövde yln)))  (- (koor-y (first (yılan-gövde yln))) 10)) (son-elemanı-sil (yılan-gövde yln))) (yılan-yön yln) (yem-x yln) (yem-y yln))]
      [(string=? (yılan-yön yln) "aşağı") (yılan-oluştur (cons (koor-oluştur (koor-x (first (yılan-gövde yln)))  (+ (koor-y (first (yılan-gövde yln))) 10)) (son-elemanı-sil (yılan-gövde yln))) (yılan-yön yln) (yem-x yln) (yem-y yln))]
      [else yln])))
```

```
;; Bir yılanın alır ve koordinatlarını kontrol eder. Eger yılan sahnenin dışına çıktı ise #true döner. Aksi takdirde #false döner.

(: oyun-bitti? (yılan -> boolean))

(check-expect (oyun-bitti? (yılan-oluştur (cons (koor-oluştur 100 100) empty) "sağ" 0 0)) #f)
(check-expect (oyun-bitti? (yılan-oluştur (cons (koor-oluştur 201 100) empty) "sağ" 0 0)) #t)
(check-expect (oyun-bitti? (yılan-oluştur (cons (koor-oluştur 100 201) empty) "sağ" 0 0)) #t)
(check-expect (oyun-bitti? (yılan-oluştur (cons (koor-oluştur 300 100) empty) "sol" 0 0)) #t)

(define oyun-bitti?
  (lambda (yln)
    (cond
      [(< (koor-x (first (yılan-gövde yln))) 5) #t]
      [(> (koor-x (first (yılan-gövde yln))) 195) #t]
      [(< (koor-y (first (yılan-gövde yln))) 5) #t]
      [(> (koor-y (first (yılan-gövde yln))) 195) #t]
      [else #f])))
```

```
;; Bir yılan alır ve oyunun son sahnesini gösteren bir resim döner.

(: son-sahne (yılan -> image))

(check-expect (son-sahne (yılan-oluştur (cons (koor-oluştur 201 100) empty) "sağ" 0 0)) (place-image (text "Oyun Bitti!" 30 "red") 100 100 (empty-scene 200 200)))

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
;; Yılanın yemi yeyip yemedigini soöyler. Eğer yilanin ilk karesinin (baş kısmının) koordinatları
;; yemin koordinatları ile üst üste gelirse (aynı olursa) #true döner. Aksi halde #false döner.

(: yemi-yedi-mi? (yılan -> boolean))

(check-expect (yemi-yedi-mi? (yılan-oluştur empty "sağ" 100 200)) #false)
(check-expect (yemi-yedi-mi? (yılan-oluştur (cons (koor-oluştur 15 5) (cons (koor-oluştur 5 5) empty)) "sağ" 100 200)) #false)
(check-expect (yemi-yedi-mi? (yılan-oluştur (cons (koor-oluştur 15 5) (cons (koor-oluştur 5 5) empty)) "sol" 15 5)) #true)
(check-expect (yemi-yedi-mi? (yılan-oluştur (cons (koor-oluştur 15 5) (cons (koor-oluştur 5 5) empty)) "sağ" 5 5)) #false)

(define yemi-yedi-mi?
  (lambda (yln)
    (cond
      [(empty? (yılan-gövde yln)) #false]
      [(and (= (koor-x (first (yılan-gövde yln))) (yem-x yln)) (= (koor-y (first (yılan-gövde yln))) (yem-y yln))) #true]
      [else #false])))
```

```
;; Bir yılan alır, yılan yemi yedi ise yılanı uzatır ve ilerletir, eger yemedi ise sadece ilerletir.
(: kontrol-et-ilerle (yılan -> yılan))

(check-expect (kontrol-et-ilerle (yılan-oluştur empty "sağ" 10 20)) (yılan-oluştur empty "sağ" 10 20))
(check-expect (kontrol-et-ilerle (yılan-oluştur (cons (koor-oluştur 15 5) (cons (koor-oluştur 5 5) empty)) "sağ" 100 200)) (yılan-oluştur (cons (koor-oluştur 25 5) (cons (koor-oluştur 15 5) empty)) "sağ" 100 200))
;;(check-expect (kontrol-et-ilerle (yılan-oluştur (cons (koor-oluştur 15 5) (cons (koor-oluştur 5 5) empty)) "sağ" 15 5)) (yılan-oluştur (cons (koor-oluştur 25 5) (cons (koor-oluştur 15 5) (cons (koor-oluştur 5 5) empty))) "sağ" 195 195))

(define kontrol-et-ilerle
  (lambda (yln)
    (cond
      [(yemi-yedi-mi? yln) (yılanı-uzat (ilerle yln) (yılan-kuyruk yln))]
      [else (ilerle yln)])))
```

```
;; Bir yılan alır ve kuyruk kısmına bir kare daha ekler.

(: yılanı-uzat (yılan koor -> yılan))

;;(check-expect (yılanı-uzat (yılan-oluştur empty "sağ" 10 20) (koor-oluştur 15 5)) (yılan-oluştur (cons (koor-oluştur 15 5) empty) "sağ" 195 195))
;;(check-expect (yılanı-uzat (yılan-oluştur (cons (koor-oluştur 15 5) empty) "sol" 100 200) (koor-oluştur 5 5)) (yılan-oluştur (cons (koor-oluştur 15 5) (cons (koor-oluştur 5 5) empty)) "sol" 195 195)) 

(define yılanı-uzat
  (lambda (yln kuyruk)
    (yılan-oluştur (append (yılan-gövde yln) (cons kuyruk empty)) (yılan-yön yln) (+ 5 (* (random 20) 10)) (+ 5 (* (random 20) 10)))))
```

```
;; Bir yılan alır ve kuyrugunun koordinatını döner

(: yılan-kuyruk (yılan -> koor))

(check-expect (yılan-kuyruk (yılan-oluştur empty "sağ" 10 20)) (koor-oluştur 5 5))
(check-expect (yılan-kuyruk (yılan-oluştur (cons (koor-oluştur 15 5) empty) "sağ" 10 20)) (koor-oluştur 15 5))
(check-expect (yılan-kuyruk (yılan-oluştur (cons (koor-oluştur 15 5) (cons (koor-oluştur 5 5) empty)) "sağ" 15 5)) (koor-oluştur 5 5))

(define yılan-kuyruk
  (lambda (yln)
    (cond
      ((empty? (yılan-gövde yln)) (koor-oluştur 5 5))
      (else (first (reverse (yılan-gövde yln)))))))
```

```
;; Yılanı (50,100) noktasından yönü sağ tarafa doğru olacak sekilde yerleştirerek simulasyonu başlatır.

(big-bang
    (yılan-oluştur (cons (koor-oluştur 15 5) (cons (koor-oluştur 5 5) empty)) "sağ" rastgele rastgele)
  (on-key yön-değiştir)
  (on-tick kontrol-et-ilerle 0.1)
  (stop-when oyun-bitti? son-sahne)
  (to-draw çiz 200 200))
```