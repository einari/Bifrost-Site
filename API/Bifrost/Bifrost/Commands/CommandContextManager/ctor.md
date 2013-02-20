**ctor**

Initializes a new instance of [Bifrost.Commands.CommandContextManager](Bifrost.Commands.CommandContextManager)

#Parameters#


##uncommittedEventStreamCoordinator##
A [Bifrost.Events.IUncommittedEventStreamCoordinator](Bifrost.Events.IUncommittedEventStreamCoordinator) to use for coordinator an [Bifrost.Events.UncommittedEventStream](Bifrost.Events.UncommittedEventStream)

##sagaLibrarian##
A [Bifrost.Sagas.ISagaLibrarian](Bifrost.Sagas.ISagaLibrarian) for saving sagas to

##processMethodInvoker##
A [Bifrost.Events.IProcessMethodInvoker](Bifrost.Events.IProcessMethodInvoker) for processing events

##executionContextManager##
A [Bifrost.Execution.IExecutionContextManager](Bifrost.Execution.IExecutionContextManager) for getting execution context from

##eventStore##
A [Bifrost.Events.IEventStore](Bifrost.Events.IEventStore) that will receive any events generated
