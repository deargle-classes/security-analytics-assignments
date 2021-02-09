---
title: "Why is one hot encoding necesssary?"
id: "one-hot-encode-why"
---


> Q: I feel like this isn't that complicated but I don't really understand how one hot encoding helps improve a model


I too had this question once. It doesn't improve anything, but a statistical model cannot deal with text -- only numbers. It can
only assign static weights that get multiplied by feature values.

What would `.246 * blue` be? `.246 * red`?

You might think to keep all the colors in the same column, and assign them number codes, but that only works if, say, `blue (2)` is
conceptually twice the amount of `yellow (1)`, which of course doesn't make sense. But since models can only assign one weight per
feature (column), that's what it would be.

One hot encoding gives you one column/feature per string/factor value, so that the model can assign different weights to each value. And each column has either a `0` or a `1` value, so each factor level gets its own weight. And that weight is, for a
logistic regression, just the average of all values of the dependent variable that have the given factor level. (The
average in the presence of all other variables in the model).

And R does it for you (one hot encoding) as long as the feature is cast as type factor

Sklearn, you have to do it, but it's easy enough to create a preprocessing pipeline that applies one hot encoding to each string columntype.
