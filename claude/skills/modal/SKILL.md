---
description: Expert guidance for building on Modal (modal.com). Use whenever code imports `modal`, the project deploys to Modal, or the task involves Modal Apps, Functions, Cls, Servers, Sandboxes, Images, Volumes, Secrets, GPUs, web endpoints, or the `modal` CLI.
---

# Modal expert

Kathir works at Modal. Treat Modal work as first-class: write idiomatic, current-API Modal code, and verify against source rather than memory. Modal ships features and deprecations every release, so training-data knowledge is often stale.

## Sources of truth (check in this order)

1. **Installed version**: `modal --version`, then `modal changelog --since <version-or-date>` to catch anything newer than you know. `modal <cmd> --help` for CLI details. Many commands accept `--json`. Prefix with `uv run` in a uv project.
2. **SDK source**: the `modal-labs/modal` monorepo (private; Kathir has access). Relevant paths:
   - `client/py/modal/` — Python SDK. Public API is in `__init__.py`'s `__all__`; implementations live in underscore modules (`_functions.py`, `_image.py`, `_partial_function.py`, `app.py`, `sandbox.py`, `volume.py`, ...). Signatures and docstrings here are authoritative.
   - `client/py/CHANGELOG.md` — release notes, including breaking changes and deprecations.
   - `client/js/`, `client/go/` — JS and Go SDKs (Sandboxes and calling deployed Functions only).
   - `examples/` — canonical examples, grouped by topic (`06_gpu_and_ml`, `07_web`, `13_sandboxes`, ...).
   - `frontend/src/lib/docs/guide/*.svx` — source of the user guide.
   - `client/py/modal/skills/modal/SKILL.md` — Modal's official agent skill.

   Getting it: use an existing local clone if there is one (check `~/modal`, `~/code/modal`, or ask). In a Claude Code cloud session, attach it with the `add_repo` tool (`modal-labs/modal`) and clone as instructed. Otherwise `git clone --depth 1 --filter=blob:none --sparse https://github.com/modal-labs/modal && git -C modal sparse-checkout set client/py examples frontend/src/lib/docs/guide` (the full repo is large). Never modify or push to it unless asked.
   If access fails, use the public mirrors: `modal-labs/modal-client` (the `client/` dir) and `modal-labs/modal-examples` (the `examples/` dir).
3. **Docs**: https://modal.com/llms.txt indexes every page; append `.md` to any docs URL for plain text. Don't pull https://modal.com/llms-full.txt into context; grep it.

When unsure whether a parameter exists or what it defaults to, grep the SDK source before writing it.

## Core model

- `app = modal.App("name")`. Decorate with `@app.function(...)`, `@app.cls(...)`, `@app.server(...)`, `@app.local_entrypoint()`. `modal.Stub` is gone.
- CLI: `modal run app.py[::fn]` (ephemeral), `modal serve app.py` (hot-reloading ephemeral, for web), `modal deploy app.py` (persistent; `--strategy rolling|recreate`), `modal shell`, `modal app logs <app> [--follow] [--since 1h] [--search ...]`, `modal container logs <id>`, `modal app list|stop|rollover|rollback`. Module paths need `-m` (`modal deploy -m pkg.app`).
- There is no local mode. Everything needs a token (`modal token info` to debug) and network. `.local()` runs the body in-process; `modal.is_local()` distinguishes contexts.
- Deployed objects are referenced lazily: `modal.Function.from_name("app", "fn")`, `modal.Cls.from_name("app", "Cls")`, `modal.Volume.from_name(..., create_if_missing=True)`, etc. `.lookup()` was removed everywhere except `modal.App.lookup(name, create_if_missing=True)`. `Function.from_name(..., version=N)` pins to a deployment version.
- Environments: `--env` / `MODAL_ENVIRONMENT`. Profiles: `--profile` / `.modal.toml`.

## Functions

- Invocation: `.remote()`, `.spawn()` → `FunctionCall` (`.get(timeout=)`, `FunctionCall.from_id`), `.map()`, `.starmap()`, `.for_each()`, `.spawn_map()`. Exceptions from `.map()` are raised as-is (`return_exceptions=True` to collect them).
- Async code: use `.aio` variants (`await f.remote.aio()`, `async for x in f.map.aio(...)`). Blocking calls in async contexts emit warnings by default; fix them rather than silencing.
- Key `@app.function` params: `image`, `gpu`, `cpu`, `memory`, `ephemeral_disk`, `timeout` (default 300s, max 24h), `startup_timeout`, `retries=modal.Retries(max_retries=, backoff_coefficient=, initial_delay=)`, `secrets`, `env` (non-sensitive vars), `volumes`, `schedule`, `region`, `routing_region`, `cloud`, `nonpreemptible` (CPU only), `enable_memory_snapshot`, `block_network`, `restrict_modal_access`, `single_use_containers`, `include_source`, `name`.
- Autoscaler: `min_containers`, `max_containers`, `buffer_containers`, `scaledown_window`. The old names (`keep_warm`, `concurrency_limit`, `container_idle_timeout`, `allow_concurrent_inputs`, `max_inputs=1`) are removed. Tune live with `Function.update_autoscaler()` (reset on redeploy).
- Input concurrency: `@modal.concurrent(max_inputs=, target_inputs=)`. Dynamic batching: `@modal.batched(max_batch_size=, wait_ms=)`. Runtime overrides: `.with_options()`, `.with_concurrency()`, `.with_batching()`.
- Schedules (deployed apps only): `schedule=modal.Cron("0 9 * * *", timezone="America/New_York")` or `modal.Period(hours=1)`.
- GPUs: `gpu="H100"`, `"A100-80GB:2"`, fallback list `["H100", "A100-40GB:2"]`. Types: T4, L4, A10, L40S, A100, A100-40GB, A100-80GB, RTX-PRO-6000, H100, H200, B200, B300. `H100!` forbids auto-upgrade to H200; `B200+` allows B300 at B200 price. Multi-node: `@modal.clustered(size=N)`.

## Classes (stateful containers)

- `@app.cls(...)` + `@modal.enter()` (load models once per container), `@modal.exit()`, `@modal.method()`. Parametrize with `x: str = modal.parameter()`.
- Always instantiate before calling: `Model().predict.remote(x)`; deployed: `modal.Cls.from_name("app", "Model")(x="a").predict.remote(...)`.
- Memory snapshots: `enable_memory_snapshot=True` with `@modal.enter(snap=True)` for CPU-side init; GPU snapshots are experimental (`experimental_options={"enable_gpu_snapshot": True}`).

## Images

- `modal.Image.debian_slim(python_version="3.12").uv_pip_install(...)` (prefer over `pip_install`), `.apt_install`, `.run_commands` (accepts `volumes=` / `secrets=`), `.env({...})`, `.workdir`, `.run_function`, `.uv_sync()`, `.pipe()` for reusable recipes. Existing images: `.from_registry("nvidia/cuda:12.8.0-devel-ubuntu24.04", add_python="3.12")`, `.from_dockerfile`, `.from_scratch()`.
- Local code/data: `.add_local_python_source("pkg")`, `.add_local_dir(local, remote)`, `.add_local_file`. These must be the last layers (they're mounted at startup) unless `copy=True`. `Mount` and `context_mount=` are gone.
- Heavy imports that aren't installed locally go inside the function body or under `with image.imports():`.
- Named images: `image.publish("name:tag")` / `modal.Image.from_name("name:tag")` (never triggers a build). `image.build(app)` builds eagerly.
- Order layers from least to most frequently changing; each changed layer rebuilds everything after it. Pin versions.

## Storage and state

- `modal.Volume`: mount via `volumes={"/data": vol}`. Writes become visible elsewhere only after `vol.commit()` (auto on container exit, plus background commits); readers call `vol.reload()`. `vol.with_mount_options(read_only=True, sub_path=...)`. Manage with `modal volume ...` or `Volume.objects.create/delete`. Cache model weights here rather than in the Image when they're large.
- `modal.CloudBucketMount` for S3/GCS/R2. `modal.Dict` / `modal.Queue` for small shared state and job queues. `modal.Secret.from_name / from_dict / from_dotenv`; secrets arrive as env vars.

## Web and servers

- Web functions: `@modal.fastapi_endpoint(method="POST", requires_proxy_auth=...)` (`@modal.web_endpoint` now errors), `@modal.asgi_app()`, `@modal.wsgi_app()`, `@modal.web_server(port)`. Use `modal serve` while iterating.
- `@app.server(port=8000, target_concurrency=..., unauthenticated=...)` on a class whose `@modal.enter()` starts a process binding `0.0.0.0`: the low-latency HTTP primitive. Differences from Functions: auth required by default (proxy tokens), no request queueing (503 while scaled to zero), no retries or Modal-side timeouts, autoscaling only when `target_concurrency` is set, sticky sessions via `Modal-Session-ID`, no `@modal.method()`.
- `modal endpoint` CLI deploys managed LLM inference endpoints. `modal curl` hits authenticated URLs.

## Sandboxes

- `app = modal.App.lookup("sbx", create_if_missing=True)`; `sb = modal.Sandbox.create("cmd", ..., app=app, image=..., timeout=..., gpu=..., encrypted_ports=[...], readiness_probe=modal.Probe.with_tcp(8080), outbound_domain_allowlist=[...], block_network=..., pty=...)`.
- `p = sb.exec("bash", "-c", "..."); p.wait(); p.stdout.read()`. Files: `sb.filesystem.read_text/write_text/read_bytes/write_bytes/copy_from_local/copy_to_local/list_files/stat/make_directory/remove/watch` (`sb.open`, `sb.ls`, `sb.mkdir`, `sb.rm`, `sb.watch` are deprecated).
- Lifecycle: `sb.wait_until_ready()`, `sb.terminate(wait=True)`, `sb.detach()` when done, `modal.Sandbox.from_id/from_name`, `sb.tunnels()`, `sb.create_connect_token()`. Snapshots: `snapshot_filesystem()` (new root image), `snapshot_directory()` + `mount_image()` (both default to a 30-day `ttl`; pass `ttl=None` to keep forever).
- `MODAL_SANDBOX_V2=1` opts into the higher-throughput backend (default in 1.6).

## Defaults to reach for

- Pin `modal` in the project's deps and match the local CLI to it.
- Load models in `@modal.enter()`, store weights on a Volume, and use `min_containers`/`buffer_containers` only where cold starts matter (they cost money while idle).
- Use `.map()`/`.spawn_map()` for fan-out instead of client-side thread pools.
- For an LLM server, start from the vLLM/SGLang examples under `examples/06_gpu_and_ml/llm-serving/`, or `modal endpoint` if a managed endpoint fits.
- Debug with `modal app logs`, `modal container logs`, `modal shell app.py::fn` (fresh container from the Image) or `modal shell <container-id>` (attach to a running one), and `obj.get_dashboard_url()`.
