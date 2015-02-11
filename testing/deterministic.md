# Deterministic build environment

We tend to disable tests if they fail unpredictably

## Steps

1. Introduce non-deterministic bug in your tests
1. Run tests until the bug manifests
1. Identify appropriate tooling for skipping tests
1. Fix test instead of skipping