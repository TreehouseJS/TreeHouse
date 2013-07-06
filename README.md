Treehouse
=========
JavaScript sandboxes to help Web developers help themselves.

Our [paper](https://www.usenix.org/system/files/conference/atc12/atc12-final159.pdf). The [USENIX 2012 talk](https://www.usenix.org/conference/usenixfederatedconferencesweek/treehouse-javascript-sandboxes-help-web-developers-help).

# Treehouse is broken but fixable

**tl;dr: The current implementation of Treehouse is vulnerable to prototype poisoning by guest code.**

In the Q&A of the talk, James Mickens pointed out that the broker is vulnerable to prototype poisoning attacks by guest code. Guest code can replace `RegExp.test`, for example, with a malicious version that always returns true. This would defeat all regular expression policy rules.

We believe this vulnerability can be fixed. First, the broker will no longer perform trusted operations after guest code has executed. Instead, it will have two phases of operation: a trusted phase and an untrusted one.

The trusted phase is defined to be before guest code runs and the untrusted phase after guest code runs. In the trusted phase, the broker will replace all privileged API methods and properties by calling `Object.defineProperty` on them, replacing them with `null` or with a proxy that marshals requests to perform privileged operations to the monitor for approval and action. It then sets up an RPC client and server to communicate with the monitor via `postMessage`. Finally, it sets up the VDOM and runs guest code.

Under this arrangement, we believe the following claims are true:

1. guest code cannot alter the DOM of the parent page except as allowed by the monitor
2. guest code cannot invoke privileged browser APIs except as allowed by the monitor

We are working on this now.
