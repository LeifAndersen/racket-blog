    Title: How many occurrences of car in the PLT source code? 
    Date: 2007-09-06T10:30:00
    Tags: Robby Findler, code-stats, hacking

Lets play a guessing game. See who can guess:

* How many occurrences of the identifier `car` there are in the PLT tree (when using `read` and just counting the symbols that come out)?
* Where does `car` rank on the list of the most commonly used identifiers?
* What is the most common identifier, and how many occurrences of it are there?

UPDATE: The two files `raw-hattori` and `raw-kajitani.ss` are generated files
containing solutions to Paint by Numbers problems and about 30,000 occurrences
of `x` and `o`. Discounting them, this is the list of the top ten identifiers and
the number of occurrences:

```
((define 25294)
 (quote 24101)
 (lambda 18883)
 (let 14796)
 (send 14349)
 (x 11877)
 (if 11118)
 (... 8474)
 (car 7610)
 (syntax 6537))
```

The identifier `cdr` ranks 21st with 5,259 occurrences, `let*` has 3,066 which,
when combined with `let` comes out at 17,862, still not enough to pass `lambda`.
Speaking of combining, `λ` has 2,271 occurrences, which is also not enough to
move `lambda`. Finally `map` comes in 32nd with 3,853 occurrences and `foldl` beats
out `foldr` (1168th place with 75 occurrences vs 1451st place with 58
occurrences).
