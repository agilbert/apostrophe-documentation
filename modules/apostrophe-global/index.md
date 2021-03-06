---
title: "apostrophe-global (module)"
layout: reference
module: true
namespaces:
  server: true
  browser: true
children:
  - server-apostrophe-global-cursor
  - browser-apostrophe-global
  - browser-apostrophe-global-chooser
  - browser-apostrophe-global-relationship-editor
  - browser-apostrophe-global-editor-modal
  - browser-apostrophe-global-manager-modal
---
## Inherits from: [apostrophe-pieces](../apostrophe-pieces/index.html)
### `apos.global`
Provides req.data.global, an Apostrophe doc
for sitewide content such as a footer displayed on all pages. You
can also create site-wide preferences by adding schema fields. Just
configure this module with the `addFields` option as you normally would
for any widget or pieces module.

## options

`deferWidgetLoading`: a performance option. if true, any widget loads that can be deferred
will be until the end of the process of loading the global doc, reducing the number of queries
for simple cases.

Note that the `defer` option must also be set to `true` for all widget types
you wish to defer loads for.

To avoid causing problems for routes that depend on the middleware, loads are
only deferred until the end of loading the global doc and anything it
joins with; they are not merged with deferred loads for the actual page.
This option defaults to `false` because in many cases performance is
not improved, as the global doc often contains no deferrable widgets,
or loads them efficiently already.

`addFields`: if the schema contains fields, the "Global Content" admin bar button will
launch the editor modal for those, otherwise it will shortcut directly to the versions dialog box
which is still relevant on almost all sites because of the use of global header
and footer areas, etc.

This module provides middleware so that `req.data.global` is always available,
even in requests that are not for Apostrophe pages. In a command line task, you can use
the provided `findGlobal` method.

## properties

`_id`: the MongoDB ID of the global doc. Available after `modulesReady`.


## Methods
### findGlobal(*req*, *callback*)
Fetch the `global` doc object. On success, the callback is invoked
with `(null, global)`.
### modulesReady(*callback*)

### initGlobal(*callback*)
Initialize the `global` doc, if necessary. Invoked late in the
startup process by `modulesReady`.
### enableMiddleware()
Add the `addGlobalToData` middleware.
### addGlobalToData(*req*, *res*, *next*)
Fetch the global doc and add it to `req.data` as `req.data.global`, if it
is not already present. If it is already present, skip the
extra query.

If called with three arguments, acts as middleware.

If called with two arguments, the first is `req` and the second is
invoked as `callback`.

If called with one argument, that argument is `req` and a promise
is returned.
### busyTryAgainSoon(*req*)

### whileBusy(*fn*, *options*)
Run the given function while the entire site is marked as busy.

This is a promise-based method. `fn` may return a promise, which will
be awaited. This method will return a promise, which must be awaited.

While the site is busy new requests are delayed as much as possible,
then GET requests receive a simple "busy" page that retries
after an interval, etc. To address the issue of requests already
in progress, this method marks the site busy, then waits for
`options.whileBusyDelay` seconds before invoking `fn`.
That option defaults to 60 (one minute). Explicitly tracking
all requests in flight would have too much performance impact
on normal operation.

This method should be used very rarely, for instance for a procedure
that deploys an entirely new set of content to the site. Use of
this method for anything more routine would have a crippling
performance impact.

**Use with workflow**: if `options.locale` argument is present, only
the given locale name is marked busy. If `req` has any other
`req.locale` it proceeds normally. This option works only with
'apostrophe-workflow' (the global docs must have `workflowLocale`
properties).
### getCreateSingletonOptions(*req*)

### addToAdminBar()
There is only one useful object of this type, so having access to the admin
bar button is not helpful unless you can edit that one, rather than
merely creating a new one (for which there is no UI). Thus we need
to set the permission requirement to admin-apostrophe-global.
This is called for you.
### getEditControls(*req*)

