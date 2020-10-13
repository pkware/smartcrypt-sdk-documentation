# Observables

The Smartcrypt Key Management component relies heavily on Observables. These may be unfamiliar, but are easy to get a
hang of or convert to a more familiar paradigm. If unfamiliar with reactive programming, we recommend taking a look at
the [ReactiveX website](http://reactivex.io/tutorials.html) for tutorials, guides, and more. 

=== "Java"
    ```java
    import io.reactivex.Observable;

    // Observables emit a stream of events. This is a push model rather than a pull model.
    Observable<Integer> observable = Observable.range(0, 5);

    // Observables can be transformed
    Observable<String> intsAsStrings = observable.map(Object::toString);

    // Use common stream/observable operators
    Observable<String> filteredInts = intsAsStrings.filter(number -> !"3".equals(number));

    // Observables only process when subscribed to. All of the previous "work" is only setup - none of it has
    // run yet. When we subscribe now, the work will happen.
    Disposable subscription = filteredInts.subscribe(System.out::println);

    // Subscriptions must be disposed when we're done with them, otherwise the stream will continue processing
    // when we no longer need it
    subscription.dispose();

    /*
     * Observables can also be made blocking, effectively making them a single, synchronous call
     */
    Integer lastNumberBlocking = Observable.range(0, 4).blockingLast();
    System.out.println("With a blocking call: " + lastNumberBlocking);
    ```

=== "Kotlin"
    ```kotlin
    import io.reactivex.Observable

    // Observables emit a stream of events. This is a push model rather than a pull model.
    val observable = Observable.range(0, 5)

    // Observables can be transformed
    val intsAsStrings = observable.map(Int::toString)

    // Use common stream/observable operators
    val filteredInts = intsAsStrings.filter {"3" != it }

    // Observables only process when subscribed to. All of the previous "work" is only setup - none of it has
    // run yet. When we subscribe now, the work will happen.
    val subscription = filteredInts.subscribe(::println)

    // Subscriptions must be disposed when we're done with them, otherwise the stream will continue processing
    // when we no longer need it
    subscription.dispose()

    /*
    * Observables can also be made blocking, effectively making them a single, synchronous call
    */
    val lastNumberBlocking = Observable.range(0, 4).blockingLast()
    println("With a blocking call: $lastNumberBlocking")
    ```