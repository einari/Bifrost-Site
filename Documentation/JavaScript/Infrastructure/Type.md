One of the missing pieces of JavaScript is dependency management. Numerous solutions exist to this problem, but in our oppinion they are all too involved getting to work. We believe highly in conventions and that most of the time conventions will cover the need. Therefor, dependency resolvement is by default following this. Allthough, it is possible to add your conventions if the built in one does not suit you. In fact, you can have multiple.

With Type, one can define types and give dependencies in the sense that any of the parameters going into the function that defines the type is used for its name as the dependency. The default resolver of these parameters will look for dependencies matching the name of the parameter in the same namespace as the type that asks for it exist in. If it does not exist there, it will move up one chain in the namespace and try there and recurse this till there is no more namespace. After this it will move on to the next resolver registered. When using the AssetsManager server side part of this, Bifrost in the client will discover all JavaScripts that exist and lay them out according to convention by mapping the file path and namespace, this is also overridable. When it knows about what is on the server and it is not able to resolve it from the namespace, it will go and load the dependency if it exists on the server. For loading it uses [require.js](http://www.requirejs.com).

Defining your type :

	Bifrost.namespace("My.namespace", {
		MyType : Bifrost.Type.extend(function(firstDependency, secondDependency) {
			var self = this;

			this.someFunction();
		})
	});


To create an instance, synchronously : 

	var instance = My.namespace.MyType.create();

If there are dependencies that are only possible to resolve through loading the dependency, Bifrost will throw an exception if one uses the synchronous function for creating an instance. If you're not sure if things are loaded, you can instead use the following pattern : 

	My.namespace.MyType.beginCreate().continueWith(function(nextPromise, instance) {
		// do what you need to with the instance
	});


It is also possible, for instance in unit tests / specifications, to specify instances to use instead of Bifrost resolving them : 

	My.namespace.MyType.create({
		firstDependency : "Hello",
		secondDependency : "World"
	});


Another thing that Type makes more consistent and in our oppinion easier, is inheritance and still maintain dependencies for each level in the inheritance chain without the need for passing dependencies from one chain to another.

	Bifrost.namespace("My.namespace", {
		MyType : Bifrost.Type.extend(function(firstDependency, secondDependency) {
		})
	});

	Bifrost.namespace("My.other.namespace", {
		MyOtherType : My.namespace.MyType.extend(function(otherDependency) {
		});
	});

It will walk up the chain of inheritance to the root and start resolving from that point and use the prototypic inheritance in JavaScript with each level in the chain as the instance used as prototype to the next level.



