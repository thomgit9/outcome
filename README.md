[![Documented with Setinstone.io](https://img.shields.io/badge/⛰️Documented%20with-Setinstone.io-success?logo=book&logoColor=white)](https://calendly.com/set-in-stone-thomas-benoit/setinstone-demo)

# Outcome

<table width="100%">
<tr><th>master branch<th>develop branch
<tr><td align="center"><img src="https://github.com/ned14/outcome/workflows/Documentation/badge.svg?branch=master"><td align="center"><img src="https://github.com/ned14/outcome/workflows/Documentation/badge.svg?branch=develop">
<tr><td align="center"><img src="https://github.com/ned14/outcome/workflows/Installability/badge.svg?branch=master"><td align="center"><img src="https://github.com/ned14/outcome/workflows/Installability/badge.svg?branch=develop">
<tr><td align="center"><a href="https://github.com/ned14/outcome/actions?query=workflow%3A%22Unit+tests+Linux%22"><img src="https://github.com/ned14/outcome/workflows/Unit%20tests%20Linux/badge.svg?branch=master"></a><td align="center"><a href="https://github.com/ned14/outcome/actions?query=workflow%3A%22Unit+tests+Linux%22"><img src="https://github.com/ned14/outcome/workflows/Unit%20tests%20Linux/badge.svg?branch=develop"></a>
<tr><td align="center"><a href="https://github.com/ned14/outcome/actions?query=workflow%3A%22Unit+tests+Mac+OS%22"><img src="https://github.com/ned14/outcome/workflows/Unit%20tests%20Mac%20OS/badge.svg?branch=master"></a><td align="center"><a href="https://github.com/ned14/outcome/actions?query=workflow%3A%22Unit+tests+Mac+OS%22"><img src="https://github.com/ned14/outcome/workflows/Unit%20tests%20Mac%20OS/badge.svg?branch=develop"></a>
<tr><td align="center"><a href="https://github.com/ned14/outcome/actions?query=workflow%3A%22Unit+tests+Windows%22"><img src="https://github.com/ned14/outcome/workflows/Unit%20tests%20Windows/badge.svg?branch=master"></a><td align="center"><a href="https://github.com/ned14/outcome/actions?query=workflow%3A%22Unit+tests+Windows%22"><img src="https://github.com/ned14/outcome/workflows/Unit%20tests%20Windows/badge.svg?branch=develop"></a>
</table>

CTest dashboard: [https://my.cdash.org/index.php?project=Boost.Outcome](https://my.cdash.org/index.php?project=Boost.Outcome)

All tests passing source tarballs: [https://github.com/ned14/outcome/releases](https://github.com/ned14/outcome/releases)

Documentation: [https://ned14.github.io/outcome/](https://ned14.github.io/outcome/)

---

## Presentation

**Outcome** is a lightweight C++14 library designed to handle and report function failures efficiently. It provides types `outcome<T>` and `result<T>` as an alternative or complement to exception handling, enabling explicit and predictable control of error conditions.

Use cases include:
- Environments where C++ exceptions are not suitable due to performance or policy.
- Systems requiring static control flow analysis for safety and correctness.
- Software architectures that propagate errors across threads without exceptions.
- Codebases compiled with `-fno-exceptions` or legacy systems without exception safety design.

Repository: [https://github.com/thomgit9/outcome](https://github.com/thomgit9/outcome)  
Default branch: `develop`  
License: Apache 2.0 / Boost Software License 1.0 dual license  
Visibility: public  

Maintainers:  
- [ned14](https://github.com/ned14) (primary author and maintainer)

---

## Installation

Outcome can be used either as a regular multi-header library or as a **single header** implementation.

**Option 1 – Single header inclusion:**

You may include the generated single header file directly in your project:

```bash
# Linux
wget https://github.com/ned14/outcome/raw/master/single-header/outcome.hpp

# BSD
fetch https://github.com/ned14/outcome/raw/master/single-header/outcome.hpp

# Using curl
curl -O -J -L https://github.com/ned14/outcome/raw/master/single-header/outcome.hpp
```

If debugging using Microsoft Visual Studio, consider including the [debugger visualization file](https://github.com/ned14/outcome/raw/master/include/outcome/outcome.natvis) in your build for improved IDE integration.

**Option 2 – CMake integration:**

```bash
git clone https://github.com/thomgit9/outcome.git
cd outcome
cmake -B build
cmake --build build
```

CTest integration is available, reporting to [CDash](https://my.cdash.org/index.php?project=Boost.Outcome).

---

## Usage

Outcome provides robust result types for safer error handling.

Typical usage examples:

```cpp
#include "outcome.hpp"
using namespace OUTCOME_V2_NAMESPACE;

result<int> divide(int a, int b)
{
  if(b == 0) return error_code(std::errc::invalid_argument);
  return a / b;
}

int main()
{
  auto r = divide(10, 2);
  if(r.has_value())
    std::cout << "Result: " << r.value() << std::endl;
  else
    std::cerr << "Error: " << r.error().message() << std::endl;
}
```

Outcome integrates with modern C++ constructs, supports coroutines, and interoperates with Boost.System and Standard error handling.

---

## Main Classes and Functions

| Name | File | Description | Inputs | Outputs |
|---|---|---|---|---|
| `result<T>` | `include/outcome/result.hpp` | Represents a value or an error condition. | `T`, `ErrorCode` | Value `T` or `ErrorCode` |
| `outcome<T>` | `include/outcome/outcome.hpp` | Extends `result<T>` to include exception and error storage. | `T`, `ErrorCode`, `Exception` | Value `T` or error/exception information |
| `basic_result` | `include/outcome/basic_result.hpp` | Low-level generic implementation of `result<T>`. | Template type parameters | Typed result wrapper |
| `basic_outcome` | `include/outcome/basic_outcome.hpp` | Customizable core of `outcome<T>`. | Template type parameters | Structured result including error/exception states |
| `try<T>` | `include/outcome/try.hpp` | Provides macro helpers for concise error propagation. | Expression or callable returning `result<T>` | Returns unwrapped success or propagates failure |
| `status_bits()` | `abi-compliance/src/main.cpp` | Returns ABI-locked internal status bitfields for `outcome`. | None | Internal status mask |
| `value_storage_int()` | `abi-compliance/src/main.cpp` | ABI-locked storage accessor for trivial value types. | `value_storage_trivial<int, long>&` | Returns identical object |
| `value_storage_NonTrivialType()` | `abi-compliance/src/main.cpp` | ABI-locked storage accessor for non-trivial types. | `value_storage_nontrivial<NonTrivialType, int>&` | Returns identical object |

---

## Developer Verification

Commits and tags in this repository can be verified using the following PGP key:

```
-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: GnuPG v2

mDMEVvMacRYJKwYBBAHaRw8BAQdAp+Qn6djfxWQYtAEvDmv4feVmGALEQH/pYpBC
llaXNQe0WE5pYWxsIERvdWdsYXMgKHMgW3VuZGVyc2NvcmVdIHNvdXJjZWZvcmdl
IHthdH0gbmVkcHJvZCBbZG90XSBjb20pIDxzcGFtdHJhcEBuZWRwcm9kLmNvbT6I
eQQTFggAIQUCVvMacQIbAwULCQgHAgYVCAkKCwIEFgIDAQIeAQIXgAAKCRCELDV4
Zvkgx4vwAP9gxeQUsp7ARMFGxfbR0xPf6fRbH+miMUg2e7rYNuHtLQD9EUoR32We
V8SjvX4r/deKniWctvCi5JccgfUwXkVzFAk=
=puFk
-----END PGP PUBLIC KEY BLOCK-----
```

---

### Contributors

| Contributor | GitHub Profile | Contributions |
|---|---|---|
| Niall Douglas | [ned14](https://github.com/ned14) | 1538 |
| Andrzej Krzemienski | [akrzemi1](https://github.com/akrzemi1) | 45 |
| cstratopoulos | [cstratopoulos](https://