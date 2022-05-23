---
layout: post
title:  "Accumulating With reduce"
date:   2013-10-26
categories: jekyll update
---

Recently, I discovered that I needed a way to accumulate results of function calls. My first attempt utilized a recursive function that accumulated the results in a parameter. However, after looking at my solution I noticed that the same behavior can be achieved with Clojure’s built-in function reduce.

To illustrate this point, I’ll use the following example: An ordered list of dates is processed until a specified date. To each matching date a function is applied. In addition, the results of the function calls are accumulated and returned.

My first attempt is shown below:

{% highlight clojure %}
(ns accumulator.core
  (:require [clj-time.core :as t]))

(def all-dates (reverse (map #(t/date-time 2013 %) (range 1 13))))

(defn get-month [date]
  (t/month date))

(defn process-dates-accumulator [all-dates until f]
  (loop [dates all-dates
         result []]
    (if (t/before? (first dates) until)
      result
      (recur (rest dates)
             (conj result (f (first dates)))))))

(process-dates-accumulator all-dates (t/date-time 2013 06) get-month)
{% endhighlight %}

The `process-dates-accumulator` iterates over the range of dates. For each date more recent than until the function `f` is applied. In this example `get-month` is used. Furthermore, the accumulation is done in results.

## Accumulating With reduce

Being a functional language Clojure embraces the use of generic combinator functions like `map` and `reduce`.

Consider this implementation of the example described above:

{% highlight clojure %}
(defn process-dates-reduce [all-dates until f]
  (reduce #(conj %1 (f %2))
          []
          (filter #(not (t/after? until %)) all-dates)))
{% endhighlight %}

First, the initial list of dates is filtered to include all candidates. Afterwards, reduce accumulates the results of applying the function f to each candidate.

## Conclusion

As shown above standard functions like reduce offers powerful abstractions. The advantage of using these functions is that those are highly optimized. Moreover, they can be applied to a wide range of situations.
