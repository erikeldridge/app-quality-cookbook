# Fractional deployment

Fractional deployment will let us deploy a feature in an "off" state and then gradually expose it.

## Prerequisites

* Dynamic configuration

## Steps

1. Define method to determine availability
1. Define application helpers
1. Define feature to deploy
1. Wrap a feature in dynamic configuration guards and deply
1. Using our dynamic configuration, increase exposure 0 -> 5 -> 10.
1. At each step, pause to monitor the health of the client.
