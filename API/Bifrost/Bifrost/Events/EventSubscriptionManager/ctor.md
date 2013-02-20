**ctor**

Initializes an instance of [Bifrost.Events.EventSubscriptionManager](Bifrost.Events.EventSubscriptionManager)

#Parameters#


##repository##
A [Bifrost.Events.IEventSubscriptionRepository](Bifrost.Events.IEventSubscriptionRepository) that will be used to maintain subscriptions from a datasource

##typeDiscoverer##
A [Bifrost.Execution.ITypeDiscoverer](Bifrost.Execution.ITypeDiscoverer) for discovering [Bifrost.Events.IEventSubscriber](Bifrost.Events.IEventSubscriber) s in current process

##container##
A [Bifrost.Execution.IContainer](Bifrost.Execution.IContainer) for creating instances of objects/services

##localizer##
A [Bifrost.Globalization.ILocalizer](Bifrost.Globalization.ILocalizer) for controlling localization while executing subscriptions
