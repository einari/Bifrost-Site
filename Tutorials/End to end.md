# The Goal
The purpose of this tutorial is to take you through the entire stack of Bifrost, all the steps you need
and can do to successfully build software using Bifrost. The philosophy behind Bifrost is somewhat 
different than what you find in more established architectures. For instance, you'll hardly see any
talk about data and doing good old CRUD (Create Read Update Delete), instead we talk about behaviors
and their consequences which in most cases result in data being generated, but not necessarily.
Often we also find that data is just one of the consequences of a behavior going through the system.
Another good point is that we don't necessarily believe in the one size fits all when it comes to
data in a system. Some data is best suited to sit inside a relational database, while some might be
better off in a document based, and many times its perfectly fine to have it be directly generated
to a static file sitting on the file system. Also, any combinations are also in many cases valid.
This opens for a few opportunities of focusing in on every feature of your software instead of 
looking at it as a whole and trying to optimize for the edge cases; leave the edge cases behind 
and model them explicitly when needed.

Building software is hard, and trying to focus on an entire application all the time really does not
make any sense in our oppinion. Instead we believe that applications are actually composed of smaller
more focused applications brought together in a bigger setting. When approaching software in this
manner you end up with code that is more decoupled, more maintainable and less prone to errors and
regression. 

### The user story
In this tutorial we're going to create a simple feature in a corner of an imagined system.
The feature we're looking at creating is the ability to register an employee. We're not going
to even try to model this remotely close to anything real, as that is too involved in this tutorial,
but rather try to capture the process of creating an end to end feature from the top-down.


### Getting started

### Projects
The way we recommend working is to have multiple projects that are focused on the different parts.
With separating it out and be very clear what their purpose is, we believe you end up with 
something that is more maintainable and easy to understand were to put things.
So, lets get started by setting up a couple of projects. We're going to need the following C# class 
library projects : 

* Concepts
* Domain
* Events
* Read

Once you've created the projects some of these projects will need to reference some of the others.
The Domain and the Read project should have a reference to the Concepts and the Events projects.
The Events project represents the contract between the Domain and the Read side.

For the client that we are going to make, we will be needing a totally empty ASP.net project 
without any files in it, except for Web.config.

Name the project "Web".

### Nuget
Bifrost is available on Nuget, so we will be using Nuget to pull down Bifrost and any dependencies
it has.

For the Concepts, Domain, Events and Read projects, we need to add a dependency only to Bifrost, 
the core part:

	PM> Install-Package Bifrost


For the Web project we will need more, as we are going to have to configure things. Bifrost has support
for all kinds of combinations of IOC containers, database choices and so forth, but for this tutorial
we will be very specific and use a set of extensions to Bifrost that we have neatly packed into a 
Nuget package called Bifrost.Defaults.

So select the Web project and do:

	PM> Install-Package Bifrost.Defaults


### Frontend
We're fond of doing top-down development, starting in the frontend and move downwards.

The feature we're going to create is going to the registration of Employees.
Inside your web project, add a folder called Features and inside it add another folder called Employees. 
Now we're going to add the view for the registration. Add an HTML file called register.html inside the
Employees folder. We're going to add some HTML within the body tag in the newly created file.

	<fieldset>
				

	</fieldset>

Now we're going to add a viewmodel that will be associated with your feature.

	Bifrost.namespace("Features.Employees", {
		register: Bifrost.Type.extend(function() {
		})
	});

For now we're going to leave it at that. We will revisit this when we have the necessary bits ready in the
C# code.


# Domain
The domain is were you define your systems behaviors, and its what represents the vocabulary of your system, 
the verbs and the nouns. We will be structuring our domain according to principles found in 
[Domain Driven Design](http://en.wikipedia.org/wiki/Domain-driven_design).

### Bounded Contexts
A bounded context is a context that contains its own vocabulary or own representations of elements found in
the domain in your particular system. For instance if you were doing an e-commerce solution, something that
tends to show up is the term product. But in most places within an e-commerce business, product is hardly
used by the people in the different bounded contexts. Take the warehouse, they do not see products but 
rather the boxes and the characteristics of those boxes such as dimensions and weight, while the name of 
the product inside the box is irrelevant to them. Another bounded context in e-commerce would be the
purchasing department were they see other aspects of the product, they are mostly interested in margins
and other economical data. While for the user hitting the e-commerce site, they are interested in a lot of
details about the product. The purpose of establishing the bounded contexts for these are then to be able
to model what is relevant to the different parts of an organization like this.

For this tutorial we will have one bounded context, its going to be called "Human Resources".

### Modules
Within a bounded context, we talk about modules. These are more technical in nature. Its a way of grouping
relevant elements within a bounded context together. Take the e-commerce example, anything related to 
Products within a bounded context could for instance be put into a module with the name of Products.
We consider Modules as optional, but helpful in structuring an application. One of the things that we 
highly recommend to do is to not try to make one big application, but rather see them as many small ones
and compose them together. Modules is part of this fragmentation and helps focusing on decoupling your
software.

### Concepts
A concept is a lightweight representation of something you have in your domain vocabulary. Instead of losing
what things are by using primitives such as booleans, integers or strings, we encapsulate these parts of 
the vocabulary into what we call concepts. Bifrost has a baseclass to help you out with this and something
that is supported throughout Bifrost; ConceptAs<>. We recommend making concepts available for both your 
read side and your execution / behavior side, this is were the Concepts project we created earlier comes
into play. 

For this particular tutorial we will be needing one Concept. We are working with persons, more specifically
employees that we are going to register, a natural key that represent a person is the social security number
assigned at birth.
 
Inside the Concept project create a folder called Persons. 
Add a new class inside the new folder called SocialSecurityNumber.

	using Bifrost.Concepts;

	namespace Concepts.Persons
	{
		public class SocialSecurityNumber : ConceptAs<string>
		{
	        public static implicit operator SocialSecurityNumber(string socialSecurityNumber)
	        {
	            return new SocialSecurityNumber { Value = socialSecurityNumber };
	        }
		}
	}
	
The concept can now be used and be capable of going back and forth from primitives, something you'll see
be handy when we get to applying events. We only need to implement the implicit operator for going from
the underlying type; string, to the concept itself. The other implicit operator sits at the base-class.


### Command
A command represents the behavior you want to happen in your system. It represents the intent of the user
and is optimized for one purpose and the idea is to not reuse commands across the system. 
With a command, one can also express so much more than just what it is doing, but also the why, its
all just a question of how you name the command. And don't be afraid to dive in and make many commands
that effectively is doing the same thing, remember that inheritance is fine and you can have a base
one representing the actual behavior that you want to happen in the system but be very explicit on
the deriviatives of the users intentions. For our tutorial we will not be all too creative, we will
simply be adding a RegisterEmployee command

	using Bifrost.Commands;
	using Concepts.Persons;

	namespace Domain.HumanResources.Employees
	{
		public class RegisterEmployee : Command
		{
			public SocialSecurityNumber SocialSecurityNumber { get; set; }
			public string FirstName { get; set; }
			public string LastName { get; set; }
		}
	}

### Input validation

	using Bifrost.Validation;
	using FluentValidation;

	namespace Domain.HumanResources.Employees
	{
		public class RegisterEmployeeInputValidator : 
						CommandInputValidator<RegisterEmployee>
		{
			public RegisterEmployeeInputValidator()
			{
				RuleFor(c=>c.FirstName)
					.NotEmpty()
						.WithMessage("You have to specify the first name");

				RuleFor(c=>c.LastName)
					.NotEmpty()
						.WithMessage("You have to specify the last name");
			}
		}
	}

### Business validation

	using Bifrost.Validation;
	using FluentValidation;

	namespace Domain.HumanResources.Employees
	{
		public class RegisterEmployeeBusinessValidator : 
						CommandBusinessValidator<RegisterEmployee>
		{
		}
	}


### Security

### Events

	using System;
	using Bifrost.Events;

	namespace Events.HumanResources.Employees
	{
		public class EmployeeRegistered : Event
		{
			public EmployeeRegistered(Guid eventSourceId) : base(eventSourceId) {}

			public string SocialSecurityNumber { get; set; }
			public string FirstName { get; set; )
			public string LastName { get; set; }
		}
	}

### Aggregated Root

	using System;
	using Bifrost.Domain;
	using Events.HumanResources.Employee;

	namespace Domain.HumanResources.Employees
	{
		public class Employee : AggregatedRoot
		{
			public Employee(Guid id) : base(id) {}

			public void Register(
							SocialSecurityNumber socialSecurityNumber, 
							string firstName, 
							string lastName)
			{
				Apply(new EmployeeRegistered(Id)
				{
					SocialSecurityNumber = socialSecurityNumber,
					FirstName = firstName,
					LastName = lastName
				}):
			}
		}
	}

### Command Handler

	using Bifrost.Commands;
	using Bifrost.Domain;

	namespace Domain.HumanResources.Employees
	{
		public class CommandHandlers : IHandleCommands
		{
			IAggregatedRootRepository<Employee> _repository;

			public CommandHandlers(IAggregatedRootRepository<Employee> repository)
			{
				_repository = repository;
			}

			public void Handle(RegisterEmployee command)
			{
				var employee = _repository.Get(Guid.NewGuid());
				employee.Register(
					command.SocialSecurityNumber,
					command.FirstName,
					command.LastName);
			}
		}
	}

### ReadModel

	using System;
	using Bifrost.Read;

	namespace Read.HumanResources.Employees
	{
		public class Employee : IReadModel
		{
			public Guid Id { get; set; }
			public SocialSecurityNumber	SocialSecurityNumber { get; set; }
			public string FirstName { get; set; }
			public string LastName { get; set; }
		}
	}

### Event subscriber

	using Bifrost.Events;
	using Bifrost.Read;
	using Events.HumanResources.Employees;

	namespace Read.HumanResources.Employees
	{
		public class EventSubscribers : IProcessEvents
		{
			IReadModelRepositoryFor<Employee> _repository;

			public EventSubscribers(IReadModelRepositoryFor<Employee> repository)
			{
				_repository = repository;
			}

			public void Process(EmployeeRegistered @event)
			{
				_repository.Insert(new Employee
				{
					Id = @event.EventSourceId,
					SocialSecurityNumber = @event.SocialSecurityNumber,
					FirstName = @event.FirstName,
					LastName = @event.LastName
				});
			}
		}
	}

### Query

	using Bifrost.Read;

	namespace Read.HumanResources.Employees
	{
		public class AllEmployees : IQueryFor<Employee>
		{
			IReadModelRepositoryFor<Employee> _repository:
			
			public AllEmployees(IReadModelRepositoryFor<Employee> repository)
			{
				_repository = repository;
			}

			public IQueryable<Employee> Query 
			{
				get 
				{
					return _repository.Query;
				}
			}
		}
	}


# Going back up into the frontend
With all the artifacts we now in C#, Bifrost produces a set of proxies at runtime that the JavaScript can take advantage of.



