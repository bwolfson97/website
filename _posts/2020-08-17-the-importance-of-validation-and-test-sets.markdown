---
layout: post
title:  "The Importance of Validation and Test Sets"
comments: true
---
So you're studying machine learning and keep hearing about validation and test sets. You know they're important, 
but you're not sure *why*. That's what this article is about. Let's dive in!

![data-split]({{ site.baseurl }}/images/articles/importance-of-val-and-test-sets/data-split.png "Splitting the data into train, validation and test sets."){:height="350px"}

## Why are Validation and Test Sets Important?

Here's the big picture: Validation sets help you prevent **overfitting** and test sets tell you if your model will **generalize** to new data. Let's unpack that. Overfitting is when your model performs well on the data it trained on, but poorly on new data. A model is said to generalize when it performs similarly on both training data and new data.

The whole point of training a model is to use it on new data (if we made an animal classifier that only worked on animals the model had previously seen, it wouldn't be very useful!). We want a model that generalizes, which means avoiding overfitting is a key concern. But why does overfitting even happen in the first place?

## Why Does Overfitting Occur?

When our model trains, it's just learning to map inputs to outputs in the train set. Initially, it doesn't know anything and makes terrible predictions. After seeing the data a few times though, it learns to pick out general features in the inputs that help it predict the outputs. Finally, with enough passes through the data, the model learns to map perfectly. It no longer has to rely on the general features it found earlier. Instead, it now knows exactly what each input in the training data looks like, and has memorized its corresponding output. Great, right?

Not quite! The problem here is that our goal is different than the model's goal! Our goal is to have a model that generalizes to *new* data, but the model's goal is to perform perfectly on the *training* data! In other words, the model's goal is to overfit! It doesn't care about or even know about other data.

![natural-progression-of-training]({{ site.baseurl }}/images/articles/importance-of-val-and-test-sets/natural-progression-of-training.png "Over the course of training, a model naturally progresses from underfitting -> learning general features -> overfitting."){:height="600px"}

So how do we stop our model from overfitting? If we had another set of data that the model didn't train on, we could periodically test its performance on that data. That would let us find the point when the model stopped learning general features (which help classify new data) and when the model started memorizing its training data. That's exactly what a validation set is!

## Validation Sets Prevent Overfitting on the Training Set

By testing our model's performance on a validation set after each epoch, we're able to see when it starts to get worse. Right before this happens is when our model can generalize to new data the best! Not only that, we can use our validation set to adjust our other hyperparameters (learning rate, regularization, architecture, etc.), not just the number of training epochs, to get a model that can perform even better on the validation set! So a validation set gives us two key advantages:

1. We can see when our model stops generalizing and starts overfitting
2. We can tune the model's hyperparameters so that it achieves better performance on the validation set before #1 happens

Great, we've used our validation set to tune our hyperparameters. Now we know our model will generalize the best, right?

Not quite! We can't be sure how our model will perform on new data yet. Why? Your model tuned its parameters using feedback it got from the train set. This presented the opportunity for the model to overfit to the train set. But you tuned the model's hyperparameters using feedback from the validation set. This means that you may have unknowingly caused the model to overfit to the validation set. The model might just be tuned to perform well on the validation set, but might not generalize to completely new data!

![overfitting]({{ site.baseurl }}/images/articles/importance-of-val-and-test-sets/overfitting.png "Anytime you use feedback from the data to improve your model, you might introduce overfitting!"){:height="600px"}

That's where the test set comes in.

## The Test Set Tells if You Overfit on the Validation Set

By testing the model's performance on truly unseen data once we're completely done tuning it, we get an accurate estimate of how it will perform in production!

> ## 2 Interesting Implications
> 1. We could still get an accurate estimate of how our model would perform in production without a validation set, using only a train and test set. However, this would be bad 
> because we'd lose the two advantages of the validation set. We wouldn't know if our model was overfitting on the train set, which means any hyperparameter tuning would be 
> meaningless. We'd have no way to tell if it was making the model more general, or just making it overfit more.
> 2. **You can only test on the test set once!** If you test your model on the test set and then go back and tune it to perform better, the test set becomes the same as the 
> validation set! You won't be able to tell if any increase in performance is because of a more general model or because of overfitting to the test set! That's why Kaggle has 
> the private leaderboard. Since they allow multiple submissions, people can tune their model to perform better on the public leaderboard. This introduces the potential of 
> overfitting to it. By withholding some of the test data for the private leaderboard, Kaggle can see which models generalize the best.


## How Should You Pick Your Validation and Test Sets?
The way you pick what data goes into your validation and test sets is extremely important. If the data your model encounters in production is in some way fundamentally different than the data you had in your test set, then your test set won't provide an accurate estimate of your model's performance. The main principle to keep in mind is this: Pick data that are as similar as possible to the type of data your model will see in production (while preventing any overlap in data between the train, validation, and test sets). For a great write-up on this, check out Fast.ai co-founder Rachel Thomas's article[here](https://www.fast.ai/2017/11/13/validation-sets/).

Enjoy the article? Let me know your thoughts in the comments below! :)
