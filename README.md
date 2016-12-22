# Virtual Beams Asteroid

An implementation of Asteroid with Angular.

##Table of contents

* [Installation](#installation)

* [Notes](#notes)

* [Configuration](#configuration)

    * [vbaConfigProvider](#vbaConfigProvider)

* [API](#api)

    * [vbaService](#vbaservice)

    * [vbaUtils](#vbautils)

    * [events](#events)

---

##Installation

First, dowload the library with bower:

    bower install virtual-beams-asteroid

Then, add this in your html:

    <script src="bower_components/angular/angular.js"></script>
    <script src="bower_components/ddp.js/src/ddp.js"></script>
    <script src="bower_components/q/q.js"></script>
    <script src="bower_components/asteroid/dist/asteroid.browser.js"></script>
    <script src="bower_components/virtual-beams-asteroid/virtualbeams_asteroid.js"></script>

For documentation and installation of **Asteroid** see https://github.com/mondora/asteroid

Add `virtualbeamsAsteroid` to your app’s dependencies module.

```js
angular
  .module('yourApp', [
    'virtualbeamsAsteroid'
  ]);
```

---

##Configuration

###vbaConfigProvider

---

####logPrefix(prefix)

Set logPrefix.

**Arguments**

* `prefix` **String** *required*.

***Default***: `'vba->'`.

---

####host(host)

Set host.

**Arguments**

* `host` **String** *required*.

***Default***: `''`.

---

####ssl(isSSL)

Set ssl.

**Arguments**

* `isSSL` **Boolean** *required*.

***Default***: `false`.

---

####log(isLog)

Set log.

**Arguments**

* `isLog` **Boolean** *required*.

***Default***: `false`.

---

####logError(isLogError)

Set logError.

**Arguments**

* `isLogError` **Boolean** *required*.

***Default***: `false`.

---

####loginRequiredInCalls(required)

Set login required in calls a server-side methods.

**Arguments**

* `required` **Boolean** *required*.

***Default***: `false`.

---

####loginRequiredInSubscribes(required)

Set login required in subscribes.

**Arguments**

* `required` **Boolean** *required*.

***Default***: `false`.

---

####extraData(addExtraData)

Add `extraData` to subscribe and method call's if `addExtraData` is true or String.

If `addExtraData` is **true** then session token is added to `extraData`, but if `addExtraData` is **String** then add `localStorage[addExtraData]` to `extraData`.

**Arguments**

* `addExtraData` **Boolean | String** *required*.

***Default***: `false`.

---

####stopSubscriptionsOnLogout(stop)

When the event `virtualbeamsAsteroidLogout` is fired and `stop` is **true** then call `stopSubscriptions`.

**Arguments**

* `stop` **Boolean** *required*

***Default***: `false`

####Example

```js
angular.module('yourApp')
  .config(['vbaConfigProvider', function (vbaConfigProvider) {
    vbaConfigProvider.logPrefix('yourApp->');
    vbaConfigProvider.host('localhost:3000');
    vbaConfigProvider.ssl(true);
    vbaConfigProvider.log(true);
    vbaConfigProvider.logError(true);
    vbaConfigProvider.loginRequiredInCalls(false);
    vbaConfigProvider.loginRequiredInSubscribes(false);
    vbaConfigProvider.extraData('localStorageData');
  }]);
```

---

##API

###vbaService

---

####get()

Creates a new Asteroid instance if is undefined.

**Return**

An Asteroid instance.

---

####call(method, data, config)

Calls a server-side method with the specified arguments.

**Arguments**

* `method` **String** *required*.

* `data` **String | Object** *optional*.

    `data` must be Object if `config.extraData` or `vbaConfigProvider.extraData` are **true** or **String**.

* `config` **Object** *optional*.

    * `extraData` **Boolean | String** *optional*, ***default***: `false`.

        If `extraData` or `vbaConfigProvider.extraData` are **true** or **String** then add `extraData` to `data` argument.

    * `loginRequired` **Boolean** *optional*, ***default***: `false`.

        If `loginRequired` or `vbaConfigProvider.loginRequired` are **true** then will call the specific method only if have session, otherwise, will wait until it have session.

**Return**

An Angular promise.

---

####subscribe(config)

Subscribes to the specified subscription.

This function **only** *notify* to the promise when data collection changes.

**Arguments**

* `config` **Object** *required*.

    * `nameSubscribe` **String** *required*.

    * `nameCollection` **String** *required*.

    * `id` **String** *optional*, ***default***: `''`.

    * `params` **Object** *optional*, ***default***: `{}`.

    * `scope` **Object** *optional*, ***default***: `undefined`.

      When the scope is destroyed, Virtual Beams Asteroid stop the collection event handle.

    * `extraData` **Boolean | String**, *optional*, ***default***: `false`.

        If `extraData` or `vbaConfigProvider.extraData` are **true** or **String** then add `extraData` to `config.params` argument.

    * `loginRequired` **Boolean** *optional*, ***default***: `false`.

        If `loginRequired` or `vbaConfigProvider.loginRequired` are **true** then will call the specific method only if have session, otherwise, will wait until it have session.

    * `selector` **Object | Function** *optional*, ***default***: `{}`.

    * `filter` **Function** *optional*, ***default***: `undefined`.

    * `reverse` **Boolean** *optional*, ***default***: `false`

      If `reverse` is **true** then Virtual Beams Asteroid notify an array reversed.

**Return**

An Angular promise.

---

###stopSubscriptions

Stop all subscriptions.

**Return**

undefined.

---

###stopOnlySubscription

Stop only subscription.

**Arguments**

* `nameSubscription` **String** *required*.

**Return**

boolean.

---

###vbaUtils

---

####log(params...)

If `vbaConfigProvider.log` is true then call:

`console.log(vbaConfigProvider.logPrefix, params...)`.

---

####error(params...)

If `vbaConfigProvider.logError` is true then call:

`console.error(vbaConfigProvider.logPrefix, params...)`.

---

###Events

---

####virtualbeamsAsteroidConnected

This event fire when `Asteroid` connects with the server.

---

####virtualbeamsAsteroidReconnected

This event fire when `Asteroid` connects again with the server.

---

####virtualbeamsAsteroidDisconnected

This event fire when the server's connection is closed.

---

####virtualbeamsAsteroidLogin

This event fire when user login is successful.

**Arguments**

* `idUser` **String**

---

####virtualbeamsAsteroidLogout

This event fire when user logout is successful.

---

####virtualbeamsAsteroidLoginError

This event fire when user login has an error.

**Arguments**

* `error` **Object**
