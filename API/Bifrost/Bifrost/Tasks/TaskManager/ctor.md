**ctor**

Initializes a new instance of the [Bifrost.Tasks.TaskManager](Bifrost.Tasks.TaskManager)

#Parameters#


##taskRepository##
A [Bifrost.Tasks.ITaskRepository](Bifrost.Tasks.ITaskRepository) to load / save [Bifrost.Tasks.Task](Bifrost.Tasks.Task)

##taskScheduler##
A [Bifrost.Tasks.ITaskScheduler](Bifrost.Tasks.ITaskScheduler) for executing tasks and their operations

##typeImporter##
A [Bifrost.Execution.ITypeImporter](Bifrost.Execution.ITypeImporter) used for importing [Bifrost.Tasks.ITaskStatusReporter](Bifrost.Tasks.ITaskStatusReporter)

##container##
A [Bifrost.Execution.IContainer](Bifrost.Execution.IContainer) to use for getting instances
