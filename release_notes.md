# Version 1.0.0.12
* XAML Visual Tree extensions
* Adding better check for wether or not an assembly is a .NET assembly during location instead of relying on BadImageException
* Temporarily disabling Windows Phone version of Client - will return in a future release
* DocumentDB strategies for collection storage - default implementation puts all entity types in same collection called entities. Query support for getting correct entities when asking for a specific type.
* Added FromMethod MarkupExtension for XAML clients for pointing directly to a method on your ViewModel for the Command property of things like Button. 
* Enabling strong name signing - for same reason this was not enabled
* Introducing a ViewModel MarkupExtension for XAML clients that takes the type of ViewModel and it will use the container to instantiate it. Used for the DataContext property typically.
* Started work on a better way to filter assemblies you don't want to discover types for. Early days.
* Putting in place DesktopConfiguration for the WPF client
* Changing one of the overloads for IOC containers for Bind when binding to a Func. It now takes the type being bound to. 

# Version 1.0.0.11
* Support for derived query types (#538)
* Required rule more robust
* Fixed generation of Queryable extensions for .Skip and .Take (#540)
* Started work on DocumentDB implementation - EventStore is up and running (#531)
* Started work on new validation and business rules engine based on a new underlying rule engine
* FluentValidation moved into its own components - Bifrost.FluentValidation - available as its own NuGet package
* Improving the naive file system based EventStore and EntityContexts
* Improved QuickStart to showcase more features - this still need to be worked on a lot to actually cover all features.
* Started work on query validation support (#382)
* ICanResolvePrinicpal implementation automatically discovered (#549)
* Added ViewModelService for XAML based clients
* Support for SimpleInjector (#568 - pull request)
* Handling of missing properties for mapping in JavaScript fixed (#585 - pull request)
* Improving XML comments in general
* Added MethodInfoConverter to JSON component
* Started work on mapping support for C# - needed for DocumentDB support, amongst others
* Heavy work on the new control, binding and object model for JavaScript
* Improved isObject() check when checking "undefined" for JavaScript
* Code quality work with the help of NDepend
* Updated 3rd party references
  * Newtonsoft Json - 6.0.8
  * Ninject - 3.2.2.0
  * SignalR - 2.2.0

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

In Bifrost we have something called ITypeImporter and ITypeDiscoverer. The importer imports the types as instances, that means the implementation uses the IOC container to resolve an instance when calling the .Import*<>() methods. The discoverer is responsible for just discovering the types when calling the Find*<>() methods and has nothing to do with instances. With this release, we're introducing something that makes this whole thing more explicit, clearer and easier to use. There is now an interface called IInstancesOf<> that you can take a dependency on. The interface inherits IEnumerable<> and as a consequence the implementation InstancesOf<> will do the instantiation when its enumerated. The way you can use this is:

	using Bifrost.Execution;

	namespace MyNamespace
	{
		public class MySystem
		{
			public MySystem(IInstancesOf<SomeAbsractOrInterfaceType> instances)
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