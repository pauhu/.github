# Pauhu open tools

[Internet at Origin v0.1.0](https://github.com/pauhu/internet-at-origin/tree/v0.1.0) provides a source-fetching and citation client. It is available alongside Tools at Origin.

## Menu

| Item | What it serves | Ingredients |
| --- | --- | --- |
| [internet-at-origin v0.1.0](https://github.com/pauhu/internet-at-origin/tree/v0.1.0) | Publisher-source fetch and caller-supplied citations | Planned for a later revision |
| [tools-at-origin v0.1.3](https://github.com/pauhu/tools-at-origin/tree/v0.1.3) | Local calculations with digest-based receipts (not signed) | [Ingredients](https://github.com/pauhu/tools-at-origin/tree/v0.1.3/ingredients) |
| [quantum-safe-encryption v0.1.0](https://github.com/pauhu/quantum-safe-encryption/tree/v0.1.0) | Post-quantum key encapsulation (ML-KEM-768, FIPS 203) and signatures (ML-DSA-65, FIPS 204) through an open library | [Ingredients](https://github.com/pauhu/quantum-safe-encryption/tree/v0.1.0/ingredients) |
| [on-device-models](https://github.com/pauhu/on-device-models) | Model cards for ten open-weight models intended for local use on your own device (text, sight, hearing, voice, meaning, reading, objects, regions, recognising, depth), each pinned to an exact upstream revision under its own licence; weights are not re-hosted and local running is not yet verified for every model | [Model cards](https://github.com/pauhu/on-device-models#readme) |
| [mcp](https://github.com/pauhu/mcp) | Sourced answers for your agent over MCP, with the source shown on the answer | [Ingredients](https://github.com/pauhu/mcp#readme) |
| [device-firewall](https://github.com/pauhu/device-firewall) | Network and firewall configuration for HTTPS access to the Pauhu AI API | [Ingredients](https://github.com/pauhu/device-firewall#readme) |
| [pauhu-eu-connector](https://github.com/pauhu/pauhu-eu-connector) | Dataset entries and usage policies for the European Language Data Space connector | [Ingredients](https://github.com/pauhu/pauhu-eu-connector#readme) |
| [folders-at-origin](https://github.com/pauhu/folders-at-origin) | Signed plans over a folder you granted, bound to your key and device; the key stays in your OS secure store (library preview, no command-line tool yet) | [Ingredients](https://github.com/pauhu/folders-at-origin#readme) |
| systems-at-origin (coming) | Granted system records and their origins | Planned |
| measurements-at-origin (coming) | Instrument readings and calibration references | Planned |

Tools at Origin uses Python and pinned dependencies; a mean calculation measured on a 383,110-byte tiny-voca sample took 72.666 ms and 19.383 MiB peak memory, with 2,008,699 bytes read, 5,142 bytes written and zero counted network attempts per fixture run.

## For builders

Use the released open menu items locally without a key. Pauhu Ltd offers a free developer key for building, testing and demos: 50 requests per day, resetting daily at 00:00 UTC, up to 5 per minute, valid for 90 days. [Get a developer key and read the terms](https://pauhu.dev/developer-terms/).

You can build products for your customers. Production use of Pauhu-hosted services requires a licensed key for each end-customer device. These key rules do not apply to running the released open tools independently. Developer keys are not for production or end customers.

### Building inside an organisation

You can build for private companies, publicly owned companies and public bodies, including tools used by colleagues. The same developer-key and production-key terms apply.

**Disclosure**

- We provide the released open items and developer access within the stated limits.
- The free key does not include production or end-customer access. Rights to store or reuse a result depend on its source licence.
- Suspected misuse can lead to a temporary suspension; serious or repeated abuse can lead to permanent revocation. The terms explain warnings and review.

## Start building

These examples use Python 3.12, Git and a POSIX shell. Each starts in a separate directory.

### Internet at Origin

Measured time to first result: 4.33 seconds.

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

Measured time to first result: 3.50 seconds.

```sh
git clone --branch v0.1.3 https://github.com/pauhu/tools-at-origin.git
cd tools-at-origin
python3.12 -m venv .venv
.venv/bin/pip install --require-hashes --only-binary=:all: -r requirements.lock
.venv/bin/python tools_origin.py mean --unit m 1 2 6 | python3.12 -c 'import json,sys; print("mean:",json.load(sys.stdin)["result"])'
# Expected: mean: 3.0
```

The locked packages target x86-64 Linux with glibc 2.34 or newer.

### Quantum-safe encryption

Measured time to first result: 30.04 seconds.

This example also needs Git, CMake, a C compiler and build tools, Go 1.20 or newer, Perl, standard shell utilities and TLS certificates. It obtains the pinned source identified in the included dependency inventory.

```sh
git clone --branch v0.1.0 https://github.com/pauhu/quantum-safe-encryption.git
cd quantum-safe-encryption
src=$(python3.12 -c 'import json; print(json.load(open("ingredients/sbom.cdx.json"))["components"][0]["externalReferences"][0]["url"].split("/tree/")[0]+".git")')
git init -q ../pqc-source
git -C ../pqc-source fetch --depth=1 "$src" "$(cut -f3 dependencies.lock)"
git -C ../pqc-source checkout --detach FETCH_HEAD
CMAKE_BUILD_PARALLEL_LEVEL=4 ./build.sh ../pqc-source
python3.12 -c 'import pqc; p,s=pqc.keygen("sign"); m=b"example"; print("signature verified:",pqc.verify(p,m,pqc.sign(s,m)))'
# Expected: signature verified: True
```

Each time is a single wall-clock observation from a clean directory on Linux x86-64, including downloads and installation or build. Prerequisites were already installed; times vary.

## How each item is made

Each released item has a versioned tag and recorded tests and internal-detail checks. Tools at Origin and Quantum-safe encryption also have independent review records and published ingredients. Quantum-safe encryption has a recorded offline test. Internet at Origin's ingredients are not yet published.

Where available, ingredients describe dependencies and measured resources for named test fixtures. These are fixture-run measurements, not production costs.

Questions and ideas are welcome in [Discussions](https://github.com/orgs/pauhu/discussions). Security reports: see [SECURITY.md](https://github.com/pauhu/.github/blob/main/SECURITY.md).

## Pro-human AI

We signed the Pro-Human AI Declaration. Read [what we do today and what is still proposed](https://github.com/pauhu/.github/blob/main/DECLARATION.md).
