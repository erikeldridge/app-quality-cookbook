# Fractional deployment

## Goals

* Define fractional deployment mechanism
* Use fractional deployment mechanism

## Motivation

Fractional deployment will let us deploy a feature in an "off" state and then gradually expose it.

## Prerequisites

* Dynamic configuration

## Steps

### Honor fractional availability

Our dynamic configuration should support percentage values rather than just binary.

For example, supporting values 0-10 will allow us to gradually expose in increments of 10%.

Given this support, we can use our existing dynamic configuration system and application helpers to fractionally deploy a feature.

### Define feature to deploy

Wrap a feature in dynamic configuration guards and deply

### Gradually expose

Using our dynamic configuration, increase exposure 0 -> 5 -> 10.

At each step, pause to monitor the health of the client.
