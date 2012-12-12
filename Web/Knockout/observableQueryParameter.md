With Bifrost you can observe changes to the URL, one of these observables that can be used is for the query parameters. Bifrost is utilizing [Baluptons History.js](https://github.com/balupton/History.js/) as an abstraction for dealing with URL changes and also pushing state back. 

The observable can be used as follows : 

	var bookNumber = ko.observableQueryParameter("bookNumber", "0");

The first parameter is the name of the query parameter and the second is the default value used if the query parameter is not specified in the URL. With this observable you can also push changes back to the URL. Whenever the value changes, it will push the state back to the URL. The observable returned can be used as any other Knockout observable in bindings in the view.