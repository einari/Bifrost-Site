# Version 1.0.0.10

* Making everything compile against .net 4.5
* Updated 3rd party references
  * Newtonsoft Json - 6.0.5
  * Ninject - 3.2.0.0
  * RavenDB - 2.5.2916
  * MongoDB - 1.9.2
  * SignalR - 2.1.2
  * Unity - 3.5.1404.0
  * AutoFac - 3.5.2
* Exposing StructureMap IOC support as Nuget package
* Exposing Unity IOC support as Nuget package
* Exposing AutoFac IOC support as Nuget package
* Exposint Windsor IOC support as Nuget package
* Retiring dedicated SignalR component - merged into the Web component. Consequence is SignalR is now front and center of everything moving forward.
* Introducing a simple JSON File based EventStore implementation - mostly for demo purposes
* Introducing a simple JSON File based EntityContext implementation - mostly for demo purposes
* Cleaning up QuickStart
* Easier and more convenient way to import types (#534)
* Fixing bugs related to client mapping of objects (#524, #522)
* Fixed deserialization of Concepts based on Guids (#513)
* Fixing IE issues with mapping and type recognition. IE does not hold a property called name for the constructor property. Regex to the rescue.
* Started work on the new rule engine. First implementation targets queries and validation of these. This is the first step in moving away from FluentValidation.
* Fixing a lot of "system" properties to be prefixed with an underscore - avoiding naming conflicts
* Rules namespace holding a specification implementation is now called Specifications both in C# and JavaScript
* BundleTables routes from System.Web.Optimization are now recognized and will be ignored for the SPA HTTP request handling

## More convenient way to import instances of types - #534

In Bifrost we have something called ITypeImporter and ITypeDiscoverer. The importer imports the types as instances, that means the implementation uses the IOC container to resolve an instance when calling the .Import*<>() methods. The discoverer is responsible for just discovering the types when calling the Find*<>() methods and has nothing to do with instances. With this release, we're introducing something that makes this whole thing more explicit, clearer and easier to use. There is now an interface called IInstancesOf<> that you can take a dependency on. The interface inherits IEnumerable<> and as a consequence the implementation InstanceOf<> will do the instantiation when its enumerated. The way you can use this is:

	using Bifrost.Execution;

	namespace MyNamespace
	{
		public class MySystem
		{
			public MySystem(IInstanceOf<SomeAbsractOrInterfaceType> instances)
			{
				// The enumerator will now instantiate through the IOC container
				// giving you an instance with correct lifecycle
				foreach( var instance in instances ) 
				{
				}
			}
		}
	}


## SignalR support

We are now generating Bifrost proxies for hubs - these are placed in the namespace corresponding to the namespace maps set up for the application. Once you have created a hub like so:

	using Microsoft.AspNet.SignalR;

	namespace Web.BoudedContext.Module
	{
		public class MyHub : Hub
		{
			public int DoSomething(int someInteger, string someString)
			{
				return 42;
			}
		}
	}


From your JavaScript - for instance a ViewModel, you can now do this:

	Bifrost.namespace("Web.BoundedContext.Module", {
		myFeature: Bifrost.views.ViewModel.extend(function(myHub) {

			myHub.server.doSomething(43,"fourty three")
					.continueWith(function(result) {
				// Result should be 42...
			});
		});
	});


The "myHub" dependency will automatically be injected by convention. All hubs are placed in the closest matching namespace. If none is found, there is a global namespace called hubs that will hold it. The last dependency resolver in the chain will be the one that recognizes the global namespace and it will always be found.

With this you don't have to think about starting the SignalR Hub connection like one would have to do with a bare SignalR. Bifrost manages this on the first hub being used.

You might notice that instead of what might be expected when working with SignalR for results coming back from calls to the server:

	.done(function() {}) 

We are using the Bifrost promise instead. This is because we want consistency with Bifrost, not with SignalRs proxies. In fact, we are not using the SignalR proxies at all, but our own proxy generation engine as we use for the generation of proxies for Commands, Queries, ReadModels, Security, Validation and more.

If you want to set up client funtions that you want for the particular Hub instances, you can do this by doing the following:

	myHub.client(function(client) {
		client.doSomething = function(someString, someNumber) {
			// Do things in the client called from the server
		};
	});

This will subscribe to any method calls from the server corresponding to the name of the function. The above code can be called anytime, there is no ordering to think of as with vanilla SignalR were you have to do this prior to starting the Hub connection. 