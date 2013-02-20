**ctor**

Initializes an instance of the [Bifrost.Sagas.SagaCommandContext](Bifrost.Sagas.SagaCommandContext) for a saga

#Parameters#


##saga##
[Bifrost.Sagas.ISaga](Bifrost.Sagas.ISaga) to start the context for

##command##
[Bifrost.Commands.ICommand](Bifrost.Commands.ICommand) that will be applied

##executionContext##
A [Bifrost.Execution.IExecutionContext](Bifrost.Execution.IExecutionContext) that is the context of execution for the [Bifrost.Commands.ICommand](Bifrost.Commands.ICommand)

##eventStore##
A [Bifrost.Events.IEventStore](Bifrost.Events.IEventStore) that will receive any events generated

##uncommittedEventStreamCoordinator##
A [Bifrost.Events.IUncommittedEventStreamCoordinator](Bifrost.Events.IUncommittedEventStreamCoordinator) to use for coordinating a [Bifrost.Events.UncommittedEventStream](Bifrost.Events.UncommittedEventStream)

##processMethodInvoker##
A [Bifrost.Events.IProcessMethodInvoker](Bifrost.Events.IProcessMethodInvoker) for processing events on the [Bifrost.Sagas.ISaga](Bifrost.Sagas.ISaga)

##sagaLibrarian##
A [Bifrost.Sagas.ISagaLibrarian](Bifrost.Sagas.ISagaLibrarian) for dealing with the [Bifrost.Sagas.ISaga](Bifrost.Sagas.ISaga) and persistence
