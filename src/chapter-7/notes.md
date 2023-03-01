# Ders Notları

```
(define-record-procedures top
  make-top
  top?
  (top-x
   top-y))
```
```
(: change (top string -> top))

(check-expect (change (make-top 100 100) "left") (make-top 90 100))
(check-expect (change (make-top 100 100) "right") (make-top 110 100))
(check-expect (change (make-top 100 100) "up") (make-top 100 90))
(check-expect (change (make-top 100 100) "down") (make-top 100 110))
(check-expect (change (make-top 0 200) "r") (make-top 100 100))

(define change
  (lambda (t a-key)
  (cond
    [(key=? a-key "left")  (make-top (- (top-x t) 10) (top-y t))]
    [(key=? a-key "right") (make-top (+ (top-x t) 10) (top-y t))]
    [(key=? a-key "up")    (make-top (top-x t) (- (top-y t) 10))]
    [(key=? a-key "down")  (make-top (top-x t) (+ (top-y t) 10))]
    [(key=? a-key "r")  (make-top 100 100)]
    [else t])))
```
```
(: jump (top integer integer string -> top))

(check-expect (jump (make-top 100 100) 50 60 "drag") (make-top 50 60))
(check-expect (jump (make-top 100 100) 50 60 "move") (make-top 100 100))
(check-expect (jump (make-top 100 100) 50 60 "button-up") (make-top 100 100))
(check-expect (jump (make-top 100 100) 50 60 "button-down") (make-top 50 60))

(define jump
  (lambda (t x y m-event)
    (cond
      [(mouse=? m-event "drag") (make-top x y)]
      [(mouse=? m-event "button-down") (make-top x y)] 
      [else t])))
```
```
(: draw (top -> image))

(check-expect (draw (make-top 100 100)) (place-image (circle 10 "solid" "red") 100 100 (empty-scene 200 200)))

(define draw
  (lambda (t)
    (place-image 
     (circle 10 "solid" "red")
     (top-x t) (top-y t)
     (empty-scene 200 200))))
```
```
(big-bang
    (make-top 100 100)
  (on-key change)
  (on-mouse jump)
  (to-draw draw 200 200))
```