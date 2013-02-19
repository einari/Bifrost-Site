**#ctor**

Initializes an instance of [Bifrost.Validation.CommandValidatorProvider](Bifrost.Validation.CommandValidatorProvider) CommandValidatorProvider

#Parameters#


##typeDiscoverer##
An instance of ITypeDiscoverer to help identify and register [Bifrost.Validation.ICommandInputValidator](Bifrost.Validation.ICommandInputValidator) implementations
            and [Bifrost.Validation.ICommandBusinessValidator](Bifrost.Validation.ICommandBusinessValidator) implementations

##container##
An instance of [Bifrost.Execution.IContainer](Bifrost.Execution.IContainer) to manage instances of any [Bifrost.Validation.ICommandInputValidator](Bifrost.Validation.ICommandInputValidator)
