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

## Start building

These examples use Python 3.12, Git and a POSIX shell. Each starts in a separate directory.

### Internet at Origin

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

This example also needs CMake and a C compiler. It obtains the pinned source identified in the included dependency inventory.

```sh
git clone --branch v0.1.0 https://github.com/pauhu/quantum-safe-encryption.git
cd quantum-safe-encryption
src=$(python3 -c 'import json; print(json.load(open("ingredients/sbom.cdx.json"))["components"][0]["externalReferences"][0]["url"].split("/tree/")[0]+".git")')
git init -q ../pqc-source
git -C ../pqc-source fetch --depth=1 "$src" "$(cut -f3 dependencies.lock)"
git -C ../pqc-source checkout --detach FETCH_HEAD
./build.sh ../pqc-source
python3 -c 'import pqc; p,s=pqc.keygen("sign"); m=b"example"; print("signature verified:",pqc.verify(p,m,pqc.sign(s,m)))'
# Expected: signature verified: True
```

Where available, ingredients describe dependencies and measured resources for named test fixtures. These are fixture-run measurements, not production costs.

Questions and ideas are welcome in [Discussions](https://github.com/orgs/pauhu/discussions). Security reports: see [SECURITY.md](https://github.com/pauhu/.github/blob/main/SECURITY.md).
