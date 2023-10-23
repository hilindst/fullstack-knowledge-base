# Services & Dependency Injection

## Services

### Hierarchical Injector

if service applied into appmodule, available application wide

appcomponent available for all compoenents (but not other services)

any other component, available to that component and its children

Services can be put into other Services

but putting it in sister components could create seperate instances



# Questions
@Component providers:[] for services?  not necessary in newer angular