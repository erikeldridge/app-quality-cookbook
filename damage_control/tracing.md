# Tracing

## Motivation

Having a unique identifier for a multi-step process can be extremely helpful for investigating a problem.

For example, suppose an Android app calls an HTTP API cluster, which calls a couple internal services to compose a response. Linking each of these internal requests with the initial request would be much easier if the Android app defined a header with a unique value and each of the internal services included the value in their logs.

## Goals

* Define a unique trace id
* Pass the trace id along
* Log the trace id

## Steps

### Define the trace id

1. Define a random, sixteen character string

### Define method of transmission

1. Define an X-Trace-Id header
1. If the header exists, add it to all internal requests

### Log the trace id

1. When logging, include the trace id as a component

## Verify