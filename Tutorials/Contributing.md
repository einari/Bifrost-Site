# Introduction

Bifrost is a completely open-source project, not just a source opened. We do accept contributions, allthough we reserve our rights to decline a submission or engage in a dialog to get changes to a submission that makes it comply with what how we want to have the code.

## The Source

Bifrost is hosted on GitHub, a social hub for software projects. 

## Git


## Building

## Documentation

### XML

### Tutorials

### Guides

## Specifications


### C-Sharp

All the C# code has been specified by using  [MSpec](https://github.com/machine/machine.specifications) with an adapted style. 

Since we're using this for specifying units as well, we have a certain structure to reflect this. The structure is reflected in the folder structure and naming of files. 


The basic folder structure we have is :  

	(project to specify).Specs  
		(namespace)  
			for_(unit to specify)  
				given  
					a_(context).cs  
				when_(behavior to specify).cs  


A concrete sample of this would be : 

	Bifrost.Specs  
		Commands  
			for_CommandContext  
				given  
					a_command_context_for_a_simple_command_with_one_tracked_object.cs  
				when_committing.cs  

The implementation can then look like this :


	public class when_committing : given.a_command_context_for_a_simple_command_with_one_tracked_object_with_one_uncommitted_event
	{
    	static UncommittedEventStream   event_stream;

    	Establish context = () => event_store_mock.Setup(e=>e.Save(Moq.It.IsAny<UncommittedEventStream>())).Callback((UncommittedEventStream s) => event_stream = s);

    	Because of = () => command_context.Commit();

    	It should_call_save = () => event_stream.ShouldNotBeNull();
    	It should_call_save_with_the_event_in_event_stream = () => event_stream.ShouldContainOnly(uncommitted_event);
    	It should_commit_aggregated_root = () => aggregated_root.CommitCalled.ShouldBeTrue();
	}

We are really focused on making our specifications read out clearly in plain English, which makes the code look very different from what we do for our units. For instance we use underscore (_) as space in type names, variable names and the specification delegates. We also want to keep things as one-liners, so your Establish, Because and It statements should preferably be on one line. There are some cases were this does not make any sense, when you need to verify more complex scenarios. This also means that an It statement should be one assert. 


We've used [Moq](http://code.google.com/p/moq/) for handling mocking of objects.




### JavaScript

All JavaScript code has been specified using [Jasmine](http://pivotal.github.com/jasmine/) with a similar style and structure as with the C# code. The folder structure is pretty much the same, except for the given folder. And since we've for now settled on Jasmine, the code is a bit different.


	describe("when creating with configuration", function () {
    	var options = {
        	error: function () {
            	print("Error");
        	},
        	success: function () {
        	}
    	};
    	var command = Bifrost.commands.Command.create(options);

    	it("should create an instance", function () {
        	expect(command).toBeDefined();
    	});

    	it("should include options", function () {
        	for (var property in options) {
            	expect(command.options[property]).toEqual(options[property]);
        	}
    	});
	});   



### Other platforms

Bifrost has support for several .net based platforms, such as Silverlight, WinRT, and its important to keep these in sync. Most code can be shared between these platforms. And all the code files that is shared from the Bifrost core project into these, or similarily when it is an extension and files from the core project is shared with the platform specific project, the files are added as references from the core project.


