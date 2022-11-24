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


#######################interview copy from website ,,, not my ##############
https://climbtheladder.com/android-livedata-interview-questions/
###################

Android LiveData Interview Questions and Answers
Here are 20 commonly asked Android LiveData interview questions and answers to prepare you for your interview:

1. What is Android LiveData?
Android LiveData is a lifecycle-aware data holder class that can be used to store and manage UI-related data in a lifecycle-conscious way. LiveData follows the observer pattern, allowing data to be observed by UI components in a lifecycle-safe way. This means that data will only be updated when the UI component is in an active state, and will not be updated if the UI component is not in an active state. This helps to prevent data loss and memory leaks.

2. Can you explain how to create a new instance of Android LiveData?
You can create a new instance of Android LiveData by using the LiveData.create() method. This method takes in a LiveData.LifecycleOwner and a LiveData.Observer as parameters. The LifecycleOwner will be used to automatically manage the LiveData according to the Android Lifecycle, and the Observer will be notified of any changes to the LiveData.



3. How do you remove an observer from the list of observers in Android LiveData?
You can remove an observer from the list of observers in Android LiveData by using the removeObserver() method.

4. What’s the difference between MutableLiveData and LiveData?
MutableLiveData is a subclass of LiveData that allows you to set the value of a LiveData object. LiveData is an observable data holder class.

5. Can you explain what LifecycleOwner is in the context of Android LiveData?
LifecycleOwner is an interface implemented by classes that have a Lifecycle, which is used by LiveData to observe changes in the lifecycle of its owner. LiveData will only update its observers when the LifecycleOwner is in the appropriate state, such as when it is started or resumed. This helps to ensure that LiveData only delivers updates when its observers are able to receive them.

6. Why should we use LiveData instead of RxJava or other similar libraries?
LiveData has several advantages over similar libraries like RxJava. First, LiveData is designed to work with the Android Lifecycle, which means that it will automatically stop emitting data when the LifecycleOwner is not in an active state, and start again when the LifecycleOwner is active again. This helps to prevent memory leaks and other issues. Second, LiveData is designed to be used with the ViewModel class, which helps to keep the data separate from the UI code. This makes the code more modular and easier to test. Finally, LiveData can be used with the new Architecture Components, which provide a number of benefits, including the ability to easily save and restore data.



7. Do all classes that extend Android ViewModel have to implement onCleared()? If yes, then why?
Yes, all classes that extend ViewModel must implement onCleared(). This is because the ViewModel may be holding onto data that is no longer needed once the ViewModel is no longer in use. By implementing onCleared(), the ViewModel can clean up this data and avoid any memory leaks.

8. Can you show me an example of using MediatorLiveData with two different instances of LiveData?
Yes, you can use MediatorLiveData to merge two different instances of LiveData. For example, if you have one LiveData that is emitting data from a local database and another LiveData that is emitting data from a remote API, you can use MediatorLiveData to merge the two streams of data and emit a single stream of data to the UI.

9. Is it possible to convert an existing observable into a LiveData object? If yes, can you give me some examples?
Yes, it is possible to convert an existing observable into a LiveData object. For example, you can use the LiveData.fromObservable() method to do this.

10. Can you explain the purpose of the Transformations class?
The Transformations class is a helper class that provides common transformation functions for LiveData objects. For example, you can use the map() function to transform a LiveData object from one type to another. You can also use the switchMap() function to transform a LiveData object into another LiveData object based on a certain condition.



11. What are some common scenarios where you would want to use LiveData objects?
LiveData objects are commonly used in Android applications to provide real-time updates to the UI. For example, if you have a LiveData object that is being populated by a database query, then any time the database is updated, the LiveData object will also be updated and the UI will reflect those changes.

12. What does the setValue() function do when used with LiveData? How does this differ from postValue()?
The setValue() function is used to set the value of a LiveData object. This will trigger any observers that are observing the LiveData object to be notified of the new value. The postValue() function is used to post a new value to a LiveData object. This will trigger any observers that are observing the LiveData object to be notified of the new value, but will do so on the main thread.

13. When is the best time to use an Observer Object?
The best time to use an Observer Object is when you want to be notified of changes to a LiveData object.

14. How can you persist data across configuration changes with LiveData?
One way to do this is to use the ViewModel class, which is designed to hold data in a lifecycle-aware fashion. The ViewModel class has a method called onCleared() that is called when the ViewModel is no longer needed. In this method, you can clean up any persistent data that you are holding.



15. How do you integrate Room with LiveData?
Room can be easily integrated with LiveData to provide an observable data source. LiveData can be used to observe changes to data in Room, and react to those changes in the UI. This allows for a reactive UI that automatically updates when the underlying data changes.

16. Can you explain how the notifyObservers() function works when used in conjunction with LiveData?
The notifyObservers() function is used to notify all observers that are currently subscribed to a LiveData object that the data has been updated. This will trigger the observers to re-evaluate the data and update their UI accordingly.

17. What are the advantages of using live data over regular broadcast receivers?
LiveData is lifecycle-aware, meaning that it will automatically unsubscribe from updates when the activity or fragment it is attached to is destroyed. This helps to prevent memory leaks. Additionally, LiveData can be used to easily update the UI when data changes, as it is automatically synchronized with the UI.

18. What happens if there are multiple observers for a single LiveData object?
If there are multiple observers for a single LiveData object, then they will all be notified of any changes to the LiveData object.

19. In which ways can one observe LiveData events?
There are two ways to observe LiveData events. The first is to use an Observer, which will be notified of any changes to the LiveData. The second is to use a LiveDataReactiveStreams, which will allow you to receive a stream of updates from the LiveData.

20. Can you explain what map and switchMap operators are?
The map operator is used to transform the data in a LiveData object, while the switchMap operator is used to switch to a new LiveData object based on the data in the original LiveData object.



###############################


https://medium.com/androiddevelopers/migrating-from-livedata-to-kotlins-flow-379292f419fb

https://alexzh.com/livedata-under-the-hood/

https://medium.com/android-news/exploring-livedata-architecture-component-f9375d3644ee





