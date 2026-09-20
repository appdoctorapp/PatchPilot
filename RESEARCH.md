# Research and implementation decisions — 2026-09-20

## Environment inspected

Existing repository: an unrelated App Doctor .NET/WPF product. It was preserved; PatchPilot is a separate subtree. Windows 11 x64 (10.0.26200), .NET SDK 10.0.400, runtime 10.0.11 and WPF reference packs are installed. No Unreal project or Unreal tools were found in this workspace. PATH, common Epic/Steamworks/SteamCMD directories, Epic Launcher installation records and Epic engine registry locations did not reveal UE/UnrealPak/SteamCMD. This is a scoped discovery, not a claim about every disk on the computer. No Steam account or branch was used.

## Primary references

1. [Valve: Uploading to Steam / content structure](https://partner.steamgames.com/doc/sdk/uploading?l=english). Roughly 1 MB content chunks, stable asset ordering, limited pack sizes and locality matter. A changed pack can require its full replacement on disk. The Unreal-specific note suggests `-patchpaddingalign=1048576 -blocksize=1048576`. Compression can trade installed size against delta locality.
2. [Epic: UE-272843](https://issues.unrealengine.com/issue/UE-272843). The issue lists UE 5.1–5.4 and describes UAT's duplicated padding parameter overriding a user-supplied value. Direct UnrealPak avoids that command-line plumbing issue. We therefore do not promise that adding an INI/UAT option changes effective alignment.
3. [Epic: Project packaging settings](https://dev.epicgames.com/documentation/unreal-engine/API/Developer/DeveloperToolSettings/UProjectPackagingSettings). Documents compression, Pak/IoStore and chunk settings. The public page currently defaults to a newer engine version; 5.4-specific API pages were inaccessible in the research tool. No claim of 5.4 source-level verification is made. Adapter qualification with the customer's licensed engine is required.
4. [Epic: Pak version enumeration](https://dev.epicgames.com/documentation/unreal-engine/API/Runtime/PakFile/FPakInfo_2). Distinguishes v11 from the newer UTF-8 directory version. This adapter accepts only a standard v11 footer and delegates extraction/testing/building to the user's matching tool.
5. [CAVS upstream](https://github.com/terragov/cavs), [LICENSE](https://github.com/terragov/cavs/blob/main/LICENSE), [NOTICE](https://github.com/terragov/cavs/blob/main/NOTICE). README and exact LICENSE/NOTICE contents were inspected via GitHub on 2026-09-20. Apache License 2.0; copyright 2026 Orelvis Lago Vasallo / BitLakeLab. Upstream describes content-addressed delivery, persistent cache reuse and byte-identical reconstruction; Unity/Unreal clients are described as untested reference integrations.

## Reuse versus implement

- Reuse **UnrealPak as an external tool**, not engine source or redistributed binaries. It owns Pak format correctness and compression. UE/Epic licenses remain the developer's responsibility.
- Reuse .NET/WPF standard libraries (MIT notices shipped with portable runtime). No additional third-party package dependency.
- **Do not embed CAVS in this preview.** It addresses its own delivery/cache path; it cannot establish actual SteamPipe savings. Its license permits reuse subject to Apache terms, but a future integration must pin a version, preserve notices, mark modifications, and audit transitive dependency licenses. No CAVS code or binary was copied.
- Implement the comparison model, package adapter guardrails, snapshot isolation, candidate generation, validation orchestration, failure/recovery, reports, English UI and CLI directly.
- Preserve existing Pak membership instead of regrouping assets. Grouping needs dependency-aware chunk assignments and boot/loading tests. No new engine or runtime plugin is required.

## Minimum hypothesis experiment

Hypothesis: stable asset order and supported padding/compression changes can improve *actual* update cost without unacceptable installed-size or load-time regressions.

Hold game content, executable, cook results, machine, storage and Steam depot rules constant. First rebuild unchanged content twice to detect packaging nondeterminism. Then edit a small payload, add a payload, and delete one. Compare current packaging, stable order/uncompressed, and stable order/Zlib/1 MiB padding on both previous and new releases. Verify extracted bytes and mounts. Record first migration, same-policy update and structure-only migration independently.

The local harness runs this design on an explicitly synthetic format. It validates orchestration and illustrates mechanisms; it **does not test the real-game hypothesis**. A fake `.pak` or fabricated Steam number is never substituted.

Real qualification requires matching UE 5.4 full builds and private Steam access. Archive Steam build/depot IDs and manifest pairs; use identical clients, network conditions and download-cache conditions. Measure download bytes, disk counters and installation wall time. For game validation, launch every candidate and time an agreed representative scene over repeated cold/warm-cache runs; record machine/storage, scene and repetitions. Retain failed and worse candidates. Live branch publication is outside this tool and requires separate authorization.
