# Tracing

Having a unique identifier for a multi-step process can be extremely helpful for investigating a problem.

For example, suppose an Android app calls an HTTP API cluster, which calls a couple internal services to compose a response. Linking each of these internal requests with the initial request would be much easier if the Android app defined a header with a unique value and each of the internal services included the value in their logs.

## Steps

1. Define the trace id
1. Define method of transmission, eg a header
1. Log the trace id
