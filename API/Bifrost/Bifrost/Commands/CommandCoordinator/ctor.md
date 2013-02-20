**ctor**

Initializes a new instance of the [Bifrost.Commands.CommandCoordinator](Bifrost.Commands.CommandCoordinator)

#Parameters#


##commandHandlerManager##
A [Bifrost.Commands.ICommandHandlerManager](Bifrost.Commands.ICommandHandlerManager) for handling commands

##commandContextManager##
A [Bifrost.Commands.ICommandContextManager](Bifrost.Commands.ICommandContextManager) for establishing a [Bifrost.Commands.CommandContext](Bifrost.Commands.CommandContext)

##commandSecurityManager##
A [Bifrost.Commands.ICommandSecurityManager](Bifrost.Commands.ICommandSecurityManager) for dealing with security and commands

##commandValidationService##
A [Bifrost.Validation.ICommandValidationService](Bifrost.Validation.ICommandValidationService) for validating a [Bifrost.Commands.ICommand](Bifrost.Commands.ICommand) before handling

##dynamicCommandFactory##
A [Bifrost.Commands.IDynamicCommandFactory](Bifrost.Commands.IDynamicCommandFactory) creating dynamic commands

##localizer##
A [Bifrost.Globalization.ILocalizer](Bifrost.Globalization.ILocalizer) to use for controlling localization of current thread when handling commands
