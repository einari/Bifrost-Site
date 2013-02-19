**EstablishForSaga**

Establish a [Bifrost.Commands.ICommandContext](Bifrost.Commands.ICommandContext) for a specific [Bifrost.Commands.ICommand](Bifrost.Commands.ICommand) in the
            context of a Saga.
            This will be the current command context, unless something else establishes a new context

#Parameters#


##saga##
[Bifrost.Sagas.ISaga](Bifrost.Sagas.ISaga) to be in context of

##command##
[Bifrost.Commands.ICommand](Bifrost.Commands.ICommand) to establish for
