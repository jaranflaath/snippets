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
import android.os.Handler
import android.os.Looper
import androidx.lifecycle.LifecycleOwner
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.Observer

sealed class Message
object MyCustomMessage : Message()

object MessageBus {

    private val bus = MutableLiveData<Message>()

    fun <T : Message> publish(message: T) {
        Handler(Looper.getMainLooper()).post {
            bus.setValue(message)
        }
    }

    fun observe(lifecycleOwner: LifecycleOwner, callback: (Message?) -> Unit) {
        bus.observe(lifecycleOwner, Observer { callback(bus.value) })
    }

}

```
_Thanks to [Christopher Dambakk](https://github.com/Dambakk) for improvements_

