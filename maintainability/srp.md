# Doing one thing well

[Unix philosophy](http://www.faqs.org/docs/artu/ch01s06.html) recommends we "do one thing and do it well".

Code that performs a single action is relatively easy to reason about and maintain.

To paraphrase the principles of [separation of concerns](http://en.wikipedia.org/wiki/Separation_of_concerns) and [single-responsibility](http://en.wikipedia.org/wiki/Single_responsibility_principle), each piece of our code should perform a distinct role.

While refactoring our code to use the compose method pattern, we broke it up into several functions, each performing a single action.

If you're having trouble naming a function, the function is probably trying to do several things.

Some single-purpose functions can be so basic we won't need to test them. For example:

    int addOne(int i) {
        return i + 1;
    }

However, other functions can be single-purpose, but still relatively complex:

    public boolean isPercentageEnabled(String featurePercentage, Integer inputId) {
        return featurePercentage == null || inputId % 100 < Integer.valueOf(featurePercentage);
    }

Review your code with single-purpose functions and classes in mind. Is there anything still trying to do too much?