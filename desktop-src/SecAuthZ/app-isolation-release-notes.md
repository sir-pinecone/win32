---
title: Latest release notes for Win32 app isolation
description: Provides information about the latest releases for Win32 app isolation.
ms.topic: article
ms.date: 10/31/2024
keywords: windows, win32, windows app development, app isolation, security, sandbox
ms.localizationpriority: medium
---

# Latest release notes for Win32 app isolation

This page contains the summary of newly supported features in Windows OS releases for Win32 app isolation. Release notes for the tools can be found in the [releases](https://github.com/microsoft/win32-app-isolation/releases) section of the Win32 app isolation GitHub repo.

## Windows build 26100.2161 (2024-10-24)

1. Packages run isolated in supported OSes and fall back to FullTrust in non-supported ones
1. VS support to package apps with isolation

## Windows Insiders build (Canary Channel) 25905 (2023-07-12)

1. File consent prompt reduction for:
   - Legacy directory browsing
   - Translating between shell display names and item identifiers
   - Isolated application launch through the context menu
1. Drag and drop into applications
1. Application multi-instancing with ShellExecute

## Windows Insiders build (Canary Channel) 25357 (2023-05-04)

1. Implicit brokering for the open file dialog and other APIs
1. Printing
1. System tray icons
1. Shell notifications
1. File system privacy settings to reset file access permissions
1. Integration with Windows Privacy App Permissions pages

## Related topics

- [Win32 app isolation overview](app-isolation-overview.md)
- [Win32 app isolation GitHub repo](https://github.com/microsoft/win32-app-isolation/)
