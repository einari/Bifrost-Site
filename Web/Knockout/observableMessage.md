Within Bifrost, there sits a simple publish / subscribe mechanism for allowing to decouple components. To simplify manners when using [Knockout](http://www.knockoutjs.com), Bifrost provides an extension called observableMessage. 

	this.myValue = ko.observableMessage("topic/message", defaultValue);

The first parameter to the function is a topic or name of message that it will work with.

With this, it will observe messages sent through the Bifrost.messageing.Messenger and it will also publish messsages to the same topic/message. 

The observable that is returned can be used directly in any Knockout binding.

