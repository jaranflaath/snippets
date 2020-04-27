# Android

## Contents

* [Component hacks](#component_hacks)
  * [BottomSheetDialogFragment](#bottomsheetdialogfragment)
* [Messaging](#messaging)

## Component hacks
### `BottomSheetDialogFragment`

#### Initializing peek height of `BottomSheetDialogFragment`

```kotlin
override fun onStart() {
    super.onStart()

    val bottomSheet = dialog?.findViewById<View>(R.id.design_bottom_sheet)

    bottomSheet?.layoutParams?.height = ViewGroup.LayoutParams.MATCH_PARENT

    view?.let { view ->
        view.post {
            val parent = view.parent as View
            val params = parent.layoutParams as CoordinatorLayout.LayoutParams
            val behavior = params.behavior
            val bottomSheetBehavior = behavior as BottomSheetBehavior
            bottomSheetBehavior.peekHeight = view.measuredHeight
            (bottomSheet?.parent as View).setBackgroundColor(Color.TRANSPARENT)

        }
    }
}
```

## Messaging

### A simple message bus

A simple message bus for communicating between activities, services and fragments in your app. 

```kotlin
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.ExperimentalCoroutinesApi
import kotlinx.coroutines.channels.BroadcastChannel
import kotlinx.coroutines.channels.ReceiveChannel
import kotlinx.coroutines.launch
import kotlin.coroutines.CoroutineContext

sealed class Message
class MyMessage : Message()

object MessageBus : CoroutineScope {

    private val bus = BroadcastChannel<Message>(1)

    @OptIn(ExperimentalCoroutinesApi::class)
    fun <T : Message> publish(message: T) {
        launch { bus.send(message) }
    }

    @OptIn(ExperimentalCoroutinesApi::class)
    fun observe(): ReceiveChannel<Message> =
        bus.openSubscription()

    override val coroutineContext: CoroutineContext
        get() = Dispatchers.Default

}

```

To subscribe to messages, use the following code: 

```kotlin
launch {
    messageCollector = MessageBus.observe()
    messageCollector.receiveAsFlow().filter { it is MyMessage }.collect {
        // Do something
    }
}
```

Remember to unsubscribe from the `messageCollector` in your `onDestroy` method by calling `messageCollector.cancel()`

