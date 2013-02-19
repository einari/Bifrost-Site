**WithPropertiesFrom**

Adds properties from a given type and optionally exluding properties from a given type that is found in the inheritance chain

#Parameters#


##container##
[Bifrost.CodeGeneration.JavaScript.Container](Bifrost.CodeGeneration.JavaScript.Container) to add properties to

##type##
[System.Type](System.Type) to get properties from

##excludePropertiesFrom##
Optional [System.Type](System.Type) to use as basis for excluding properties

##assignmentVisitor##
Optional [System.Action`1](System.Action`1) that gets called for every assignment for any property
