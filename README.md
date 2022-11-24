# LiveData-Overview
This repo is all about Live data 


What is the flow ?
A flow is a stream of data that can be computed asynchronously
A flow is like live data in it’s job but different in the way it works

What is the difference between flow and liveData?
Flow is cold observer “produces data if subscribers are interested in the data”
Live data is hot Observer “produces data if even no subscribers are interested in the data”
LiveData is a lifecycle aware observable data holder it means if lifecycle is destroyed liveData will be destroyed too
Flow isn't a lifecycle aware observable data holder so we can make flow LifeCycle aware by put into repeatOnLifecycle()

The main difference between LiveData and Flow: A flow continuously emits results while Livedata will update when all the data is fetched and return all the values at once. 
Flow is not lifeCycler aware [make repeateOnLifeCycle()]  but LiveData is lifecycle aware
Flow is sort of a reactive stream like rxJava
Flow has bunch of different operator like .map, buffer() but LiveData doesn’t have
Flow is a part of Kotlin and LiveData is part of the androidx.lifecycle lubrary

How can we create a Flow?
We can create a flow by using flow Builder like: flowOf(...) functions to create a flow from a fixed set of values- flowOf(4,2,5,1,7).collect{ print(it.String())}

Flow{...} builder function to construct arbitrary flows from sequential calls to emit function: flow{ (0..10){forEach{ emit(it}}.collect{print(it.toString())} 

asFlow() extension functions on various types to convert them into flows: (1..5).asFlow().collect{print(it.toString())}

###############################
##################################


#how live data work internally : 

After diving in the Android Room code, I found out some things:

Room annotation processor generates code from Room annotations (@Query, @Insert...) using javapoet library

Depending on the result type of the query (QueryMethodProcessor), it uses a "binder" or another one. In the case of LiveData, it uses LiveDataQueryResultBinder.

LiveDataQueryResultBinder generates a LiveData class that contains a field _observer of type InvalidationTracker.Observer, responsible of listen to database changes.

Then, basically, when there is any change in the database, LiveData is invalidated and client (your repository) is notified.




