Treehouse
=========
JavaScript sandboxes to help Web developers help themselves.

Our [paper](https://www.usenix.org/system/files/conference/atc12/atc12-final159.pdf). The [USENIX 2012 talk](https://www.usenix.org/conference/usenixfederatedconferencesweek/treehouse-javascript-sandboxes-help-web-developers-help).

# Treehouse is broken but fixable

**tl;dr: The current implementation of Treehouse is vulnerable to prototype poisoning by guest code.**

In the Q&A of the talk, James Mickens pointed out that the broker is vulnerable to prototype poisoning attacks by guest code. Guest code can replace `RegExp.test`, for example, with a malicious version that always returns true. This would defeat any regular expression policy rules.
