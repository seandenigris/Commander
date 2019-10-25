# Commander
Commander models application actions as first class objects\.

Every type of action is implemented as a separate command class \(subclass of `CmdCommand`\) with an `#execute` method and all state required for execution\.

Commands are reusable objects and applications provide various ways to access them (shortcuts, context menu, buttons, etc.)\.
This information is attached to command classes as activation strategies\. Currently there are three types of activations:

- `CmdShortcutActivation`
- `CmdContextMenuActivation`
- `CmdDragAndDropActivation`

Strategies are specified as class annotations. Check out the [ClassAnnotation](https://github.com/dionisiydk/ClassAnnotation) project for details. Here are two examples:

The following method will allow `RenamePackageCommand` to be executed by shortcut in the system browser:
```Smalltalk
RenamePackageCommand class>>packageBrowserShortcutActivation
    <classAnnotation>
    ^ CmdShortcutActivation by: $r meta for: ClyPackageContextOfFullBrowser
```
And for context menu:
```Smalltalk
RenamePackageCommand class>>packageBrowserMenuActivation
    <classAnnotation>
    ^ CmdContextMenuActivation byRootGroupItemFor: ClyPackageContextOfFullBrowser
```
An activation strategy should always include an application context where it can be applied \(`ClyPackageContextOfFullBrowser` in the example\)\.
The application to be extended should provide such contexts as subclasses of `CmdToolContext` with information about application state\. Browse the hierarchy of `CmdToolContext` subclasses for examples in the image.
Every widget can provide its own context to interact with the application as a separate tool\.
For example, the system browser shows multiple panes which provide a package context, class context and method context\.
For each context, the browser can show a different menu and provide different shortcuts\.

## Installation
```Smalltalk
Metacello new
  baseline: 'Commander';
  repository: 'github://dionisiydk/Commander';
  load.
```
Use following snippet for stable dependency in your project baseline:
```Smalltalk
spec
    baseline: 'Commander'
    with: [ spec repository: 'github://dionisiydk/Commander:v0.3.x' ]
```
