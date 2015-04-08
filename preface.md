# Preface

## tl;dr

Simplify & automate

## Purpose

This book is intended to introduce a few common tools and concepts we can use to build high-quality applications.

My goal is to help more developers ship code with confidence.

## Methodology

The recommendations in this book come from my personal experience, interviews with colleagues, recommendations from site reliability engineers, guidelines from architecture groups, and publications.

I'd prefer to keep the book terse, providing enough theory and examples to ground a concept, but linking out for deeper dives. None of the ideas in the book are mine, and most have been explained in detail by talented authors many times, eg unix, git, java, etc. I want to provide references to these ideas in the context of quality engineering.

## Structure

I call this a "cookbook" because it focuses on a set of verifiable outcomes, like the recipes in a cookbook.

## Biases

This book biases toward accommodating developers who may be inexperienced with a given topic, developing applications, cutting scope over quality, holding systems responsible, collaborating, iterating, and having fun.

### Accessibility

A broad range of developers can use the tools in this book, but to ensure accessibility, I include the basics for getting started.

### App development

I have experience with quality engineering as a Web and Android application developer, and write from this perspective, but I also keep portability in mind.

### Cutting scope over quality

If the cost of shipping something is high, then I place more emphasis on up-front quality. For example, hacking on something for myself is relatively low cost, and I skip a lot of quality tooling. Developing a physical product for a paying customer, however, is high cost - the product _must_ work - and investment in quality tooling is justified. In a pinch, I recommend erring on the side of quality, i.e., "cut scope, not quality."

### Responsible systems

I recommend relying on systems rather than individuals. A team can deterministically improve a system, and that system can outlive the individuals on the team. Given it's human to make mistakes, I prefer to work in an environment that minimizes damage.

### Collaboration

I recommend against developing code in isolation to reduce stress and increase quality. Software development is often a collaborative activity.

### Iteration

I recommend shipping small changes frequently to reduce risk and facilitate maintenance and damage control. With this in mind, this book is organized such that each page provides incremental value.

I think of software development as a series of fundamental activities, reminiscent of the scientific method: making a change, observing the result, reflecting on the outcome, planning for the next change. Each section in this book is organized in this way.

### Enjoyment

Building products can be enjoyable, but shipping unpredictable applications via unforgiving systems is a terrible experience. Quality engineering helps me focus on the former.

## Acknowledgements

I consulted with several engineers while researching quality engineering. I thank the following people in particular for maintaining a high bar and taking time to talk:

* Ashwin Raghav (@ashwinraghav)
* Chad Weider (@cweider)
* Eric Frohnhoefer (@EricFrohnhoefer)
* Israel Camacho (@rallat)
* Jeremie Castagna (@jcasts)
* Jon Bettcher (@pufferfish)
* Lien Tran (@lientm)
* Marcelo Hernandez (@mhernand40)
* Mark Roseboom (@markroseboom)
* Noah Tye (@noahlt)
