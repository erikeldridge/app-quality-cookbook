# Dependency injection

## Motivation

Dependency injection can help us simplify our app and testing code.

## Goals

* Identify appropriate tooling
* Use DI in our code

## Prerequisites

* Spaghetti code

## Steps

### Tooling

DI can be a concept and a framework.

A simple conceptual example:

```
Env env = Env.DEV;
// ...
void isEnabled(){ // not injected
	if (env.equals(Env.PROD)) {
		// ...
	}
}
// vs
void isEnabled(Env env){ // injected
	if (env.equals(Env.DEV)) {
		// ...
	}
}
```

Frameworks like Guice and Dagger provide more sophisticated approaches to injection.

For example:
```
@Provides Env provideEnv() {
  return Env.DEV;
}
// ...
class Foo {
	@Inject
	public Foo(String env){ // injected
		// ...
	}
}
```

## Reflect

* Best-practice: let DI manage singletons, or just pass them as constructor args
* Did DI make it more easy or difficult test?
