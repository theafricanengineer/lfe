% lfe_clj(3)
% Tim Dysinger, Duncan McGreggor, Eric Bailey
% 2015-2016


# NAME

clj - LFE Clojure interface library.


# SYNOPSIS

This module provides Clojure-inpired functions and macros for use in LFE.


# EXPORTS

## Function Composition

**(compose f g)**

Right to left function composition.

**(compose fs x)**

Compose a list of functions ``fs``, right to left, and apply the resulting
function to ``x``.

**(compose f g x)**

Equivalent to ``(funcall (compose f g) x)``.

**(compose fs)**

Compose a list of functions ``fs`` from right to left.

**(compose)**

Equivalent to ``#'identity/1``.


### Usage

The following examples assume ``#'1+/1`` is defined:

```lfe
> (defun 1+ (x) (+ x 1))
1+
```


```lfe
> (funcall (clj:compose #'math:sin/1 #'math:asin/1) 0.5)
0.49999999999999994
> (funcall (clj:compose (list #'1+/1 #'math:sin/1 #'math:asin/1) 0.5))
1.5
```

Or used in another function call:

```lfe
> (lists:filter (compose #'not/1 #'zero?/1)
                '(0 1 0 2 0 3 0 4))
(1 2 3 4)
```

The usage above is best when ``compose`` will be called from in functions like
``(lists:foldl ...)`` or ``(lists:filter ...)``, etc. However, one may also call
``compose`` in the following manner, best suited for direct usage:

```lfe
> (compose #'math:sin/1 #'math:asin/1 0.5)
0.49999999999999994
> (compose (list #'1+/1 #'math:sin/1 #'math:asin/1) 0.5)
1.5
```


## Partial Application

**(partial f args)**

**(partial f arg-1)**

Partially apply ``f`` to a given argument ``arg-1`` or list of ``args``.


### Usage

```lfe
> (set f (partial #'+/2 1))
#Fun<clj.3.121115395>
> (funcall f 2)
3
> (set f (partial #'+/3 1))
#Fun<clj.3.121115395>
> (funcall f '(2 3))
6
> (set f (partial #'+/3 '(2 3)))
#Fun<clj.3.121115395>
> (funcall f 4)
9
> (set f (partial #'+/4 '(2 3)))
#Fun<clj.3.121115395>
> (funcall f '(4 5))
14
```


## Other Functions

**(identity x)**

Identity function.