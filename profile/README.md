# Pauhu open tools

[Internet at Origin v0.1.0](https://github.com/pauhu/internet-at-origin/tree/v0.1.0) provides a source-fetching and citation client. It is available alongside Tools at Origin.

## Menu

| Item | What it serves | Ingredients |
| --- | --- | --- |
| [internet-at-origin v0.1.0](https://github.com/pauhu/internet-at-origin/tree/v0.1.0) | Publisher-source fetch and caller-supplied citations | Planned for a later revision |
| [tools-at-origin v0.1.3](https://github.com/pauhu/tools-at-origin/tree/v0.1.3) | Local calculations with digest-based receipts (not signed) | [Ingredients](https://github.com/pauhu/tools-at-origin/tree/v0.1.3/ingredients) |
| [quantum-safe-encryption v0.1.0](https://github.com/pauhu/quantum-safe-encryption/tree/v0.1.0) | Post-quantum key encapsulation (ML-KEM-768, FIPS 203) and signatures (ML-DSA-65, FIPS 204) through an open library | [Ingredients](https://github.com/pauhu/quantum-safe-encryption/tree/v0.1.0/ingredients) |
| folders-at-origin (coming) | Selected files and their origins | Planned |
| systems-at-origin (coming) | Granted system records and their origins | Planned |
| measurements-at-origin (coming) | Instrument readings and calibration references | Planned |

Tools at Origin uses Python and pinned dependencies; a mean calculation measured on a 383,110-byte tiny-voca sample took 72.666 ms and 19.383 MiB peak memory, with 2,008,699 bytes read, 5,142 bytes written and zero counted network attempts per fixture run.

## For builders

Use the released open menu items locally without a key. Pauhu Ltd offers a free developer key for building, testing and demos: 50 requests per day, up to 5 per minute, valid for 90 days. [Get a developer key and read the terms](https://pauhu.dev/developer-terms/).

Build and ship your own product for your customers. You set your own price for your product; Pauhu does not set or limit it. For production use of the service, each end-customer device needs a licensed key. Licensed keys and their price are on [pauhu.ai](https://pauhu.ai/); sales are opening soon. Developer keys are not for production or end customers.

### Building inside an organisation

If you build for a company, non-profit or public-sector organisation, your users may be colleagues or citizens rather than paying customers. Your organisation can hold the licensed keys for its use of pauhu.ai, with one key per end-user device, or choose annual seat licences on [pauhu.eu](https://pauhu.eu/) for its data needs. These cover different services. Your organisation decides how to recover its costs internally. Sales are opening soon.

**Disclosure**

- We provide the released open items and developer access within the stated limits.
- The free key does not include production or end-customer access. Rights to store or reuse a result depend on its source licence.
- Suspected misuse can lead to a temporary suspension; serious or repeated abuse can lead to permanent revocation. The terms explain warnings and review.

## Start building

These examples use Python 3.12, Git and a POSIX shell. Each starts in a separate directory.

### Internet at Origin

Measured time to first result: 3.98 seconds.

```sh
git clone --branch v0.1.0 https://github.com/pauhu/internet-at-origin.git
cd internet-at-origin
python3.12 -m venv .venv
.venv/bin/pip install .
printf '%s\n' '{"sources":[{"title":"Pauhu","url":"https://pauhu.eu/","retrieved_at":"2026-11-19"}]}' | .venv/bin/pauhu-origin cite -
# Expected: 1. Pauhu. https://pauhu.eu/ (retrieved 2026-11-19).
```

The citation example formats caller-supplied information; it does not fetch or verify the source.

### Tools at Origin

Measured time to first result: 3.11 seconds.

```sh
git clone --branch v0.1.3 https://github.com/pauhu/tools-at-origin.git
cd tools-at-origin
python3.12 -m venv .venv
.venv/bin/pip install --require-hashes --only-binary=:all: -r requirements.lock
.venv/bin/python tools_origin.py mean --unit m 1 2 6 | python3 -c 'import json,sys; print("mean:",json.load(sys.stdin)["result"])'
# Expected: mean: 3.0
```

The locked packages target x86-64 Linux with glibc 2.34 or newer.

### Quantum-safe encryption

Measured time to first result: 26.91 seconds.

This example also needs CMake and a C compiler. It obtains the pinned source identified in the included dependency inventory.

```sh
git clone --branch v0.1.0 https://github.com/pauhu/quantum-safe-encryption.git
cd quantum-safe-encryption
src=$(python3 -c 'import json; print(json.load(open("ingredients/sbom.cdx.json"))["components"][0]["externalReferences"][0]["url"].split("/tree/")[0]+".git")')
git init -q ../pqc-source
git -C ../pqc-source fetch --depth=1 "$src" "$(cut -f3 dependencies.lock)"
git -C ../pqc-source checkout --detach FETCH_HEAD
CMAKE_BUILD_PARALLEL_LEVEL=4 ./build.sh ../pqc-source
python3 -c 'import pqc; p,s=pqc.keygen("sign"); m=b"example"; print("signature verified:",pqc.verify(p,m,pqc.sign(s,m)))'
# Expected: signature verified: True
```

Times are observations from clean directories on one Linux x86-64 run with the prerequisites already installed; downloads and build times vary.

## How each item is made

Releases use pinned inputs where needed, pass tests and checks for exposed keys and internal details, receive independent review, and are published at a named tag. Tools at Origin and quantum-safe-encryption also check offline operation and publish measured ingredients. Internet at Origin is a source-fetching client; its ingredients are planned for a later revision.

Where available, ingredients describe dependencies and measured resources for named test fixtures. These are fixture-run measurements, not production costs.

Questions and ideas are welcome in [Discussions](https://github.com/orgs/pauhu/discussions). Security reports: see [SECURITY.md](https://github.com/pauhu/.github/blob/main/SECURITY.md).
