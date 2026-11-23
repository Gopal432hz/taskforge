# TaskForge

A local prototype for making checkable coding tasks from pinned Click history.

TaskForge mines real upstream feature changes, extracts public API documentation into a task contract, and emits a package with a reference patch plus feature and selected regression checks. Two fresh examples use the same generic extraction path. The version-callback example retains a manual requirements review. Upstream feature changes were selected by a human. No frontier-model benchmark or original invention of history mining is claimed.

## Run locally

Requirements: git, bash, and an existing Python interpreter with pytest. The evidence was recorded with Python3.10.12/pytest9.0.2 and, separately, with Python3.11.17/pytest8.4.2. No command installs dependencies or calls a model API.

From `replay/`:

    python run-demo.py --python /absolute/path/to/python-with-pytest

From `replay/generalization/`:

    python run-generalization.py --python /absolute/path/to/python-with-pytest

The commands clone only the included local input bundle when needed, create task packages and run local verifiers. They replace their own generated/runtime directories. The generalization command freshly mines two selected upstream changes and extracts their API docs without a per-task prose override.

## What the results show

| Task behavior | Base feature checks | Reference feature checks | Selected regressions, base / reference |
|---|---:|---:|---:|
| Custom version callback | 3 fail | 3 pass | 93 / 93 pass |
| Option help spec | 8 fail | 8 pass | 656 / 656 pass |
| Invocation context precedence | 24 fail | 24 pass | 311 / 311 pass, plus24skips each |

The context suite has24platform-dependent skips on both sides. These are test-case counts, not task counts. Two deliberately wrong implementations are also rejected: suppressing help specs for hidden options produces4failures, and discarding invocation context settings produces7failures. See `evidence/`.

The original replay command remaps absolute verifier paths locally. A separate declared-interpreter check runs the exact unchanged emitted `solve.sh` and `test.sh` at `/app`, `/tests`, `/solution` and `/logs` under local mount/PID/network isolation, using Python3.11.17/pytest8.4.2. All three tasks produce base reward0 and reference reward1 with selected regressions passing.

## Inspectable output

Each `sample-packages/<task>/` contains:

- `instruction.md`: requirements and pinned starting version.
- `task.toml`: provisional Harbor1.4 metadata.
- `environment/repo.tar.gz`: base snapshot with no future Git history.
- `environment/Dockerfile`: declared build instructions.
- `solution/reference.patch` and `solution/solve.sh`: upstream reference implementation.
- `tests/`: selected upstream feature tests and emitted verifier.

The reusable factory code is in `replay/factory/`. The generic API-doc extractor and two-example driver are in `replay/generalization/run-generalization.py`.

## Limits

The Debian-based emitted image has NOT been built. Passing in the declared interpreter on a local Ubuntu host is not a Docker build, Harbor-runner acceptance or organizer acceptance. The Harbor layout is a provisional choice, not a confirmed event interface. Selected regressions are not whole-repository coverage. No model evaluation, resistance to memorization, exhaustive requirements coverage or arbitrary-repository/language generalization is claimed.

The input `click-history.bundle` is for preparation only. Never mount it, `solution/`, or result evidence into an evaluated coding agent's environment. Public upstream changes may already be in model training data. History stripping reduces direct future-commit access, not memorization.

## Reuse and provenance

Click code, reference patches and tests are upstream work, not TaskForge-original implementation. Upstream source: https://github.com/pallets/click . Its BSD3-Clause copyright/license notices are retained in `CLICK-LICENSE.txt` and archived base snapshots. Do not imply upstream endorsement.

Reference changes:

- Version callback: base16fc00e2f4a2717a521084f193709a6058afc693; change9f9b149ea30287b4977b800a024ee68144b8c869.
- Help spec: basebe13b152cf5949a362926322c835fa76019cc6fe; change271effb3d96da5575eb527216ce3552bd7628fab.
- Context settings: baseac0e297ec2d7d78455a7d18ce306996e668c4a85; change90a2973f85fffc4d8a20effb2bcdff3f394c5e62.

Official brief/rubric: https://hack2skill.com/event/innodata
Harbor configuration reference: https://docs.harborframework.com/tasks/configuration
Harbor verifier reference: https://docs.harborframework.com/tasks/verifier

No credentials or private accounts are required to run the local demonstration.
