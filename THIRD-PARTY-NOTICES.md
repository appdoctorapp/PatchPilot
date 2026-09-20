# Dependency and license inventory

PatchPilot-owned code is proprietary evaluation software. No additional NuGet library dependency is used.

| Component | License / use |
|---|---|
| Microsoft .NET runtime | MIT and runtime third-party notices; exact restored runtime notices included under `licenses/` in the portable ZIP |
| Microsoft WPF / Windows Desktop runtime | MIT and included third-party notices |
| Windows APIs | Part of the user's Windows installation; Windows is not redistributed |
| UnrealPak | Optional external tool from the user's licensed UE 5.4 installation; not redistributed |
| Steamworks / SteamCMD | Not bundled, invoked or connected in this version |
| CAVS | Reviewed (Apache-2.0, LICENSE and NOTICE verified); not a dependency, no code/binary redistributed |

Sources: https://github.com/dotnet/runtime/blob/main/LICENSE.TXT ; https://github.com/dotnet/wpf/blob/main/LICENSE.TXT ; https://github.com/terragov/cavs/blob/main/LICENSE ; https://github.com/terragov/cavs/blob/main/NOTICE .

Steam and Unreal Engine marks belong to their respective owners. PatchPilot is not affiliated with Valve or Epic Games. No external assets, fonts or scripts are downloaded at runtime.
