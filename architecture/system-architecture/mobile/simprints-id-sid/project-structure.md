# Project structure

Simprints ID project is comprised of a large number of relatively small and self-contained Gradle modules. There are several benefits of such separation:

* Limited module scope simplifies understanding of the implementation details.
* The self-contained module allows us to reason on a module interaction level instead of the class/function level.
* Many small modules use Gradle caching and incremental build to speed up local build times - only the changed modules are rebuilt on each run.

## Convention plugins

Such modularity comes with a trade-off in configuration overhead for each module, which is why the `build-logic` module exists. [Gradle-way ](https://docs.gradle.org/current/samples/sample\_convention\_plugins.html#compiling\_convention\_plugins)to fight increasing overhead is by creating “convention plugins” to define subsets of configuration for specific features/functionality that could be applied to project modules in a composable way.&#x20;

### Notable convention plugins in SID:

* Base Android framework configuration.
  * e.g. `simprints.android.application` and `simprints.android.library`
* Library specific configuration.
  * e.g. `simprints.android.hilt` and `simprints.library.room`
* Shared configuration plugins.
  * e.g. `simprints.testing.unit` and `simprints.config.cloud`
* SID-specific module plugins.
  * e.g. `simprints.infra` and `simprints.feature`

## Module types

### Foundational

Modules that only use base and library convention plugins and provide some specific functionality.

`id` module also falls into this category as it is the main application module of the project and is built using `simprints.android.application` plugin.

Other examples of such modules are - `infra:logging`, `infra:network`.

### Infrastructure

Modules that use `simprints.infra` as a base convention plugin, often with some specific library configuration.&#x20;

Infrastructure modules contain business logic that should be logically isolated for cleanliness and reuse.  Their goal is to encapsulate some part of the domain layer of the application and make it reusable to other components.&#x20;

Infrastructure modules have a couple of rules:

* The module should have a single access point, the contract for the module's behaviour. This usually is a top-level interface file that exposes the module's functionality. All other files (except those related to building) should be marked internal and not exposed to its users.
* The module should be completely unaware of any UI components. Therefore, they cannot depend on feature modules but on other infrastructure modules  (keep in mind the dependencies can’t be circular).

### Feature modules

Modules that use `simprints.feature` as a base convention plugin.&#x20;

Feature modules contain UI (or UI-adjacent) components and business logic in a self-contained way. For example, these modules will contain the activities, fragments, views, layouts, and navigation components of a specific feature in SID.

Feature modules will have a couple of rules:

* The module should not contain reusable business logic or things relating to infrastructure outside of the UX of the feature; those should all be inside infrastructure modules.&#x20;
* UI feature modules should expose a navigation graph with a navigable root destination, a way to build navigation arguments for this destination and a `Parcelable` result class (with `@Keep` annotation). The rest of the module should be marked internal or private.
* UI-adjacent feature modules should expose a single component to be used in other feature modules, for example `ViewModel`. Note that the public `ViewModel` can have an injectable internal constructor to keep all other implementation details internal/private.

#### UI module interaction

UI modules are designed to be reusable from any point in the app, providing the same contract that integrates with the AndroidX Navigation and small wrappers for returning results via the navigation graph interactions.&#x20;

The following example uses login module contact, but the same approach applies to any other UI module.

```kotlin
// To use the login screen from any other module, it must be added to gradle.build file
implementation(project(":feature:login"))

// Then the login graph must be included in the current modules graph with the 
// appropriate navigation action (can be global or from a specific destination)
<include app:graph="@navigation/graph_alert" />
<action
    android:id="@+id/action_orchestratorFragment_to_alert"
    app:destination="@id/graph_alert" />


// Screen result handling must be added to calling fragments onViewCreated callback
// NOTE: Handler is triggered when the calling screen is restored, 
//       but it does not prevent the rest of the callback's execution.
//       Therefore, extra calls to VM must be handled explicitly.
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    // ...
    findNavController().handleResult(
         viewLifecycleOwner, 
         R.id.orchestratorRootFragment, 
         destination
    ) { result -> }
    // ...
    vm.start() // This should be able to handle extra calls
}

// Navigate to the login screen from anywhere in the fragment
findNavController().navigate(
    R.id.action_orchestratorFragment_to_login,
    LoginContract.toArgs(request.projectId, request.userId),
)
```

## Modalities

Each biometric modality is separated into a subfolder but follows the same principles - UI in the feature module and non-UI business logic in the infrastructure modules.
