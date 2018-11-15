[@types/fdc3-api](../README.md) > [DesktopAgent](../interfaces/desktopagent.md)

# Interface: DesktopAgent

A Desktop Agent is a desktop component (or aggregate of components) that serves as a launcher and message router (broker) for applications in its domain.

A Desktop Agent can be connected to one or more App Directories and will use directories for application identity and discovery. Typically, a Desktop Agent will contain the proprietary logic of a given platform, handling functionality like explicit application interop workflows where security, consistency, and implementation requirements are proprietary.

## Hierarchy

**DesktopAgent**

## Index

### Methods

* [addContextListener](desktopagent.md#addcontextlistener)
* [addIntentListener](desktopagent.md#addintentlistener)
* [broadcast](desktopagent.md#broadcast)
* [findIntent](desktopagent.md#findintent)
* [findIntentsByContext](desktopagent.md#findintentsbycontext)
* [open](desktopagent.md#open)
* [raiseIntent](desktopagent.md#raiseintent)

---

## Methods

<a id="addcontextlistener"></a>

###  addContextListener

▸ **addContextListener**(handler: *`function`*): [Listener](listener.md)

*Defined in [interface.ts:176](/src/interface.ts#L176)*

Adds a listener for incoming context broadcast from the Desktop Agent.

**Parameters:**

| Name | Type |
| ------ | ------ |
| handler | `function` |

**Returns:** [Listener](listener.md)

___
<a id="addintentlistener"></a>

###  addIntentListener

▸ **addIntentListener**(intent: *`string`*, handler: *`function`*): [Listener](listener.md)

*Defined in [interface.ts:171](/src/interface.ts#L171)*

Adds a listener for incoming Intents from the Agent.

**Parameters:**

| Name | Type |
| ------ | ------ |
| intent | `string` |
| handler | `function` |

**Returns:** [Listener](listener.md)

___
<a id="broadcast"></a>

###  broadcast

▸ **broadcast**(context: *[Context](../#context)*): `void`

*Defined in [interface.ts:155](/src/interface.ts#L155)*

Publishes context to other apps on the desktop.

```javascript
 agent.broadcast(context);
```

**Parameters:**

| Name | Type |
| ------ | ------ |
| context | [Context](../#context) |

**Returns:** `void`

___
<a id="findintent"></a>

###  findIntent

▸ **findIntent**(intent: *`string`*, context?: *[Context](../#context)*): `Promise`<[AppIntent](appintent.md)>

*Defined in [interface.ts:112](/src/interface.ts#L112)*

Find out more information about a particular intent by passing its name, and optionally its context.

findIntent is effectively granting programmatic access to the Desktop Agent's resolver. A promise resolving to the intent, its metadata and metadata about the apps that registered it is returned. This can be used to raise the intent against a specific app.

If the resolution fails, the promise will return an `Error` with a string from the `ResolveError` enumeration.

```javascript
// I know 'StartChat' exists as a concept, and want to know more about it ...
const appIntent = await agent.findIntent("StartChat");

// returns a single AppIntent:
// {
//     intent: { name: "StartChat", displayName: "Chat" },
//     apps: [{ name: "Skype" }, { name: "Symphony" }, { name: "Slack" }]
// }

// raise the intent against a particular app
await agent.raiseIntent(appIntent.intent.name, context, appIntent.apps[0].name);
```

**Parameters:**

| Name | Type |
| ------ | ------ |
| intent | `string` |
| `Optional` context | [Context](../#context) |

**Returns:** `Promise`<[AppIntent](appintent.md)>

___
<a id="findintentsbycontext"></a>

###  findIntentsByContext

▸ **findIntentsByContext**(context: *[Context](../#context)*): `Promise`<`Array`<[AppIntent](appintent.md)>>

*Defined in [interface.ts:147](/src/interface.ts#L147)*

Find all the avalable intents for a particular context.

findIntents is effectively granting programmatic access to the Desktop Agent's resolver. A promise resolving to all the intents, their metadata and metadata about the apps that registered it is returned, based on the context types the intents have registered.

If the resolution fails, the promise will return an `Error` with a string from the `ResolveError` enumeration.

```javascript
// I have a context object, and I want to know what I can do with it, hence, I look for for intents...
const appIntents = await agent.findIntentsForContext(context);

// returns for example:
// [{
//     intent: { name: "StartCall", displayName: "Call" },
//     apps: [{ name: "Skype" }]
// },
// {
//     intent: { name: "StartChat", displayName: "Chat" },
//     apps: [{ name: "Skype" }, { name: "Symphony" }, { name: "Slack" }]
// }];

// select a particular intent to raise
const startChat = appIntents[1];

// target a particular app
const selectedApp = startChat.apps[0];

// raise the intent, passing the given context, targeting the app
await agent.raiseIntent(startChat.intent.name, context, selectedApp.name);
```

**Parameters:**

| Name | Type |
| ------ | ------ |
| context | [Context](../#context) |

**Returns:** `Promise`<`Array`<[AppIntent](appintent.md)>>

___
<a id="open"></a>

###  open

▸ **open**(name: *`string`*, context?: *[Context](../#context)*): `Promise`<`void`>

*Defined in [interface.ts:87](/src/interface.ts#L87)*

Launches/links to an app by name.

If a Context object is passed in, this object will be provided to the opened application via a contextListener. The Context argument is functionally equivalent to opening the target app with no context and broadcasting the context directly to it.

If opening errors, it returns an `Error` with a string from the `OpenError` enumeration.

```javascript
    //no context
    agent.open('myApp');
    //with context
    agent.open('myApp', context);
```

**Parameters:**

| Name | Type |
| ------ | ------ |
| name | `string` |
| `Optional` context | [Context](../#context) |

**Returns:** `Promise`<`void`>

___
<a id="raiseintent"></a>

###  raiseIntent

▸ **raiseIntent**(intent: *`string`*, context: *[Context](../#context)*, target?: *`string`*): `Promise`<[IntentResolution](intentresolution.md)>

*Defined in [interface.ts:166](/src/interface.ts#L166)*

Raises an intent to the desktop agent to resolve.

```javascript
//raise an intent to start a chat with a given contact
const intentR = await agent.findIntents("StartChat", context);
//use the IntentResolution object to target the same chat app with a new context
agent.raiseIntent("StartChat", newContext, intentR.source);
```

**Parameters:**

| Name | Type |
| ------ | ------ |
| intent | `string` |
| context | [Context](../#context) |
| `Optional` target | `string` |

**Returns:** `Promise`<[IntentResolution](intentresolution.md)>

___

