curl -O https://repo1.maven.org/maven2/org/clojure/clojure/1.8.0/clojure-1.8.0.zip
unzip clojure-1.8.0.zip
cd clojure-1.8.0
java -cp clojure-1.8.0.jar clojure.main

(defn times-two [x]
  (* 2 x))

;; comment
;; list
(+ 1 2 3 4)
;; map
{:key1 "value1", :key2 "value2"}
;; vector
[1 2 2 3 4]
;; set
#{1 2 3 4}

"Extensible Data Notation" - # gives us ways (baked in or customized) to get
other data structures

"commas are whitespace"
def sets a variable, defn sets a function

 (def mm {:a 1 :b 2})
 (:a mm)
 (mm :a)
 ::a
   -> :user/a

Phil Bagwell's Data Structures

Generative testing
(fn [x] (+ 1 x)) is the lambda

'(+ 1 2 3)
(quote (+ 1 2 3)

vim plugin for repl

Data Oriented programming
========================
data >functions >macros
hiccup / reagent

-> is a "threading macro" - like unix pipe?

clojurescript - compiles to javascript
lumo ( a repl in javascript)

target deskto with electron
react native for ios android

joy of clojure (not a beginner)
clojure from the ground up (a bit academic)
clojure for the brave and true

