# Root My Galaxy Payloads

This repository contains the device-specific native side of
[Root My Galaxy](https://github.com/BuSung-dev/Root-My-Galaxy):

- exact firmware profiles and offsets;
- the app-domain CVE-2026-43499 exploit source and compiled payload;
- the app bootstrap helper source;
- the verified KernelSU late-load build artifacts;
- the support feed consumed by the application.

It intentionally does not contain Android application source code.

## Supported payloads

| Payload | Compatible models | Kernel version | Status |
| --- | --- | --- | --- |
| `e3q-S928BXXU6ZZI4` | Galaxy S24 Ultra `SM-S928B` | `6.1.162` | Device-tested |
The Android 17 beta `S928BXXU6ZZI4` profile is currently withdrawn from the
support feed. Both the generic and legacy-shaped KernelSU Next module pairs
crashed the phone during late-load; their binaries are retained only under
`kernelsu/quarantine/` for forensic analysis. The app payload and target
constants remain in the repository, but Root My Galaxy will not automatically
select or download a ZZI4 payload until an exact kernel-compatible module can
be built and validated.

Schema version 3 keeps each exploit and KernelSU artifact once. Its flat
`models` and `kernelVersions` arrays define runtime compatibility. See
[`support/README.md`](support/README.md) for the matching rules.

The port is based on the exploit source published at
<https://github.com/NebuSec/CyberMeowfia/tree/main/IonStack/CVE-2026-43499/exploit>.

## Feed delivery

Root My Galaxy resolves the payload repository's current commit first and
fetches `support/targets-v3.json` and every artifact from that immutable
commit. Per-artifact SHA-256 fields and manifest signatures are not part of
schema version 3. `targets-v2.json` is retained for released 0.2.3 clients.

## Build

```sh
make TARGET=pa3q-S928BXXU6ZZI4 ANDROID_NDK_HOME=/path/to/android-ndk
```

Outputs:

```text
build/<profile>/cve-2026-43499
build/<profile>/cve-2026-43499-app.so
build/<profile>/cve-2026-43499-root
```

The release app payload is built with:

```sh
make TARGET=essi-S928BXXU6ZZI4 ANDROID_NDK_HOME=/path/to/android-ndk release
```

The complete firmware-to-profile procedure is recorded in
[`docs/PORTING.md`](docs/PORTING.md). Samsung-specific KernelSU changes and
versioned artifacts are documented in [`kernelsu/README.md`](kernelsu/README.md).
The exact S921B DZF2 analysis is recorded separately in
[`docs/SM-S921B-S921BXXSFDZF2.md`](docs/SM-S921B-S921BXXSFDZF2.md), and the
S928U/S928U1 DZF2 analysis is in
[`docs/SM-S928U1-S928U1UES6DZF2.md`](docs/SM-S928U1-S928U1UES6DZF2.md).

Use only on devices you own or are explicitly authorized to test.
