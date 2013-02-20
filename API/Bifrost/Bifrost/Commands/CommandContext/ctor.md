**ctor**

Initializes a new [Bifrost.Commands.CommandContext](Bifrost.Commands.CommandContext)

#Parameters#


##command##
The [Bifrost.Commands.ICommand](Bifrost.Commands.ICommand) the context is for

##executionContext##
The [Bifrost.Execution.IExecutionContext](Bifrost.Execution.IExecutionContext) for the command

##eventStore##
A [Bifrost.Events.IEventStore](Bifrost.Events.IEventStore) that will receive any events generated

##uncommittedEventStreamCoordinator##
The [Bifrost.Events.IUncommittedEventStreamCoordinator](Bifrost.Events.IUncommittedEventStreamCoordinator) to use for coordinating the committing of events
