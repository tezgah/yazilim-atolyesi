# Ders Notları

```racket
;; sayi-listesi
;; - empty
;; - (cons sayi sayi-listesi)
```

```racket
;; Bir sayı-listesi alır ve bu sayıları 2 ile çarparak yeni bir sayı listesi döner
;; [] -> []
;; [1] -> [2]
;; [3, 7] -> [6, 14]

;; (: liste-carpi-2 (sayi-listesi -> sayi-listesi))

(check-expect (liste-carpi-2 empty) empty)
(check-expect (liste-carpi-2 (cons 1 empty)) (cons 2 empty))
(check-expect (liste-carpi-2 (cons 3 (cons 7 empty))) (cons 6 (cons 14 empty)))
(check-expect (liste-carpi-2 (cons 2 (cons 8 (cons 21 empty)))) (cons 4 (cons 16 (cons 42 empty))))

(define liste-carpi-2
  (lambda (lst)
    (cond
      ((empty? lst) empty)
      (else (cons (* 2 (first lst)) (liste-carpi-2 (rest lst)))))))
```

```racket
;; Bir sayı listesi alır ve her bir elemanın karesini hesaplayarak yeni bir liste döner.
;; empty -> empty
;; [1] -> [1](cons 1 (cons 2 empty))
;; [1, 2] -> [1, 4]
;; [2, 3, 5] -> [4, 9, 25]

;; ( : liste-karesi (sayi-listesi -> sayi-listesi))

(check-expect (liste-karesi empty) empty)
(check-expect (liste-karesi (cons 1 empty)) (cons 1 empty))
(check-expect (liste-karesi (cons 1 (cons 2 empty))) (cons 1 (cons 4 empty)))
(check-expect (liste-karesi (cons 2 (cons 3 (cons 5 empty)))) (cons 4 (cons 9 (cons 25 empty))))

(define liste-karesi
  (lambda (lst)
    (cond
      ((empty? lst) empty)
      (else (cons (karesi (first lst)) (liste-karesi (rest lst)))))))


(define karesi
  (lambda (x)
    (* x x )))
```

```racket
;; string-listesi
;; - empty
;; - (cons string string-listesi)
```

```racket
;; Bir string listesi alır ve liste içerisindeki tüm stringleri ard arda ekler.
;; empty -> ""
;; ["fatih"] -> "fatih"
;; ["fatih", "koksal"] -> "fatih koksal"
;; ["mehmet", "fatih", "koksal"] -> "mehmet fatih koksal"

;; (: liste-birlestir (string-listesi -> string))

(check-expect (liste-birlestir empty) "")
(check-expect (liste-birlestir (cons "fatih" empty)) "fatih ")
(check-expect (liste-birlestir (cons "fatih" (cons "koksal" empty))) "fatih koksal ")
(check-expect (liste-birlestir (cons "mehmet" (cons "fatih" (cons "koksal" empty)))) "mehmet fatih koksal ")

(define liste-birlestir
  (lambda (lst)
    (cond
      ((empty? lst) "")
      (else (string-append (first lst) " " (liste-birlestir (rest lst)))))))
```