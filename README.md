# Clover
A minimalist, barebones, header-only C++11 (and up) library for parsing semantic versions and evaluating version constraints.

## Features
- **Minimal** - it parses standard `major.minor.patch` version strings and checks them against operators like equal, greater/less than, caret (^), tilde (~).
- **Zero Dynamic Allocation** - due to its minimalist features and the use of string_view it doesn't need to allocate memory for tokens or strings.
- **Node-Style SemVer** - Supports ^ (compatible minor) and ~ (compatible patch) constraint evaluations.


# Quick start
```cpp
#include "Clover.hpp"
#include <cassert>

int main() {
    using namespace Clover;

    Version v("1.2.3");

    // string parsing evaluation
    assert(VersionConstraint("^1.2.0").satisfies(v));
    assert(!VersionConstraint(">=1.3.0").satisfies(v));
    assert(VersionConstraint("~1.2.0").satisfies(v));

    // direct component evaluation
    VersionConstraint strictReq(VersionOp::GreaterEq, 1, 3, 0);
    assert(!strictReq.satisfies(v));

    VersionConstraint boundaryReq(VersionOp::Less, Version("2.0.0"));
    assert(boundaryReq.satisfies(v));

    return 0;
}
```