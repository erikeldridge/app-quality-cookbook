# Staged deployment

We can decrease risk by deploying to pools of decreasingly risk tolerant audiences.

## Steps

### Strategies

#### Alpha/Beta

* opt-in pools
* deploy to alpha, then to beta, then to prod
* Example: Play store

#### Staging

* deploy to staging, then to prod
* aka "nightly"

#### Canary

* deploy to a percentage of prod traffic

#### Fractional

* deploy to prod, but gradually increase exposure
* explored in more depth in the [fractional deployment section](deployment/fractional.md)

#### Consistency

* the less we change between stages, the more we can predict the behavior of each stage

### Staging

1. Identify a staging environment
1. Deploy to staging
1. Test staging
1. Deploy to production
1. Test production

### Canary

1. Identify canary environment
1. Deploy to canary
1. Monitor
1. Deploy to full production

## Related

* [Google's article on hermetic servers]( http://googletesting.blogspot.com/2012/10/hermetic-servers.html)

