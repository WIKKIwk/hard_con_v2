# GScale Mobile App

`gscale-mobile-app` is the operator-facing Flutter client of the GScale
system. It is the field control surface for:

- ERP setup confirmation
- default warehouse selection
- item search
- batch start and stop
- live scale and printer monitoring
- archive inspection

## Platform Support

- Primary target: Android phone and tablet
- Development targets: Flutter desktop and web

The app does not talk to ERP directly. It talks to `gscale-platform/mobileapi`,
which then coordinates ERP setup, catalog lookup, batch lifecycle, and runtime
state.

## System Position

```text
Operator
   |
   v
gscale-mobile-app
   |
   v
gscale-platform/mobileapi
   |
   +---------------------> gscale-erp-read
   |
   +---------------------> scale worker / bridge state / printer flow
```

## What the App Knows

- whether ERP write access is configured
- whether the read service is available
- which warehouse mode is active
- the live scale value and stability state
- the live printer snapshot from the backend
- the current batch, if one is active

## Printer Behavior

- The app shows the live printer snapshot returned by `gscale-platform`.
- When no batch is active, the selected printer type auto-matches the connected
  printer if the backend reports a clear `zebra` or `godex` snapshot.
- This keeps the operator from manually re-selecting the backend printer on
  every app restart.

## Typical Workflow

1. connect to the correct backend server,
2. confirm ERP configuration,
3. choose default warehouse mode or manual mode,
4. select an item,
5. start a batch,
6. watch live scale and print progress,
7. inspect archive history when needed.

## Configuration

The app expects an API base URL via the normal Flutter launch configuration.
In the wider system that backend is usually started from the
`gscale-platform` repository.

## Companion Reading

1. [`gscale-platform`](https://github.com/accord-erp-automation/gscale-platform)
2. [`gscale-erp-read`](https://github.com/accord-erp-automation/gscale-erp-read)

Those repositories explain the execution engine and the read-model service that
this app depends on.
