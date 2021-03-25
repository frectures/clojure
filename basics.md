# Clojure Basics by Example

## Functions

```clj
(defn square
  "Multiplies x by itself"
  [x]
  (* x x))

(defn cube [x]
  (* x x x))
```

### Higher-order functions

```clj
(defn twice
  "Returns a function that applies f twice"
  [f]
  (fn [x]
    (f (f x))))

(def hypercube
  (twice square))
```

### Multi-arity & variadic functions

```clj
(defn sum
  ([] 0)
  ([x] x)
  ([x y] (+ x y))
  ([x y & more] (apply + x y more)))
```

## Bindings

### let

```clj
(defn twice
  "Returns a function that applies f twice"
  [f]
  (fn [x]
    (let [y (f x)
          z (f y)]
      z)))
```

### for

```clj
(def deck
  (for [suit ["diamonds" "hearts" "spades" "clubs"]
        rank [7 8 9 10 "jack" "queen" "king" "ace"]]
    (str rank " of " suit)))
```

### doseq

```clj
(defn print-deck []
  (doseq [suit ["diamonds" "hearts" "spades" "clubs"]
          rank [7 8 9 10 "jack" "queen" "king" "ace"]]
    (println rank "of" suit)))
```

### with-open

```clj
(defn read-first-line [filename]
  (with-open [rdr (clojure.java.io/reader filename)]
    (.readLine rdr)))
```

## Control flow

### if

```clj
(defn minimum [x y]
  (if (< x y)
    x
    y))
```

### do

```clj
(defn minimum [x y]
  (if (< x y)
    (do
      (println "returning x")
      x)
    (do
      (println "returning y")
      y)))
```

### case

```clj
(defn rotate-left [direction]
  (case direction
    :east :north
    :north :west
    :west :south
    :south :east
    (throw (new IllegalArgumentException (str direction)))))
```

### Tail recursion

```clj
(defn fix
  "Finds an attractive fixed point by repeatedly applying f to x"
  [f x]
  (let [y (f x)]
    (if (= x y)
      x
      (recur f y))))
```

### while

```clj
(while (< 0.1 (Math/random))
  (println "I'm feeling lucky!"))
```

## Collections

### PersistentList

```clj
'(4 8 15 16 23 42)

(def a (list 15 16 23 42))

(def b (conj a 8 4))

(def c (pop (pop b)))

(identical? a c)
```

### PersistentVector

```clj
[4 8 15 16 23 42]

(def a (vector 4 8 15 16))

(def b (conj a 23 42))

(def c (pop (pop b)))

(identical? a c)

(b 0)

(get b 6 :nope)
```

### PersistentHashSet

```clj
#{:east :north :west :south}

(def a (hash-set :west :east))

(def b (conj a :north :south :center))

(def c (disj b :center))

(b :center)

(c :center)

(get c :center :nope)
```

### PersistentHashMap

```clj
{:west 180 :east 0}

(def a (hash-map :west 180 :east 0))

(def b (assoc a :north 90 :south 270 :center 360))

(def c (dissoc b :center))

(b :center)

(c :center)

(get c :center :nope)
```
