# Tomb

This is a fork of [Gustavo Niemeyer's](http://niemeyer.net/) [Tomb](https://pkg.go.dev/gopkg.in/tomb.v2) package. The Tomb package tracks goroutines, allowing them to transition from an alive state to a dying state to a dead state. This is useful for background goroutines that need to delay context cancelation until they cleanly terminate.

A `tomb.Tomb` provides:

1. A way for goroutines to signal to the owner of the `tomb.Tomb` that they're dying, whether due to an error or by explicitly dying
2. A way for goroutines to wait for their dying signals to be acknowledged by the owner of the `tomb.Tomb` before the goroutines die
3. A way for the cause of death (i.e. the goroutine's final `error`) to be captured

To learn more, read these blog posts:

[_Death of goroutines under control_](https://blog.labix.org/2011/10/09/death-of-goroutines-under-control) (Niemeyer 2011-2014)

[_Context isn’t for cancellation_](https://dave.cheney.net/2017/08/20/context-isnt-for-cancellation) (Cheney 2017)

## My fork compared to Niemeyer's package

1. My fork is a Go module; Niemeyer's work predates Go modules. Some CI tools at my workplace have bugs when dealing with non-module depedencies, which motivated me to fork upstream.
1. I removed the v1 package and only provide v2.
1. I removed support for now-unmaintained Go versions.
1. The upstream unit tests didn't pass, so I modified the tests to pass.

## tomb.Tomb compared to sync.WaitGroup

The `sync.WaitGroup` type in the standard library is the idiomatic way to track goroutines.

In most cases you should use `sync.WaitGroup` over `tomb.Tomb`. A notable limitation of `tomb.Tomb` is it assumes it has a single owner, whereas `sync.WaitGroup` can be shared across multiple owners.

The singular advantage of `tomb.Tomb` is that goroutines can wait for their dying state to be acknowledged before they die. 
