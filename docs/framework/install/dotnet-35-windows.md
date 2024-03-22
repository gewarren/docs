---
title: Install .NET Framework 3.5 on Windows 11, 10, 8.1, 8
description: Learn how to install .NET Framework 3.5 on Windows 11, Windows 10, Windows 8.1, and Windows 8.
ms.date: 10/06/2021
---
# Install .NET Framework 3.5 on Windows 11, Windows 10, Windows 8.1, and Windows 8

You might need .NET Framework 3.5 to run an app on Windows 11, Windows 10, Windows 8.1, or Windows 8. This article shows you how to install it. You can also use these instructions for earlier Windows versions.

## Download from the web

You can download .NET Framework 3.5 SP1 from the [.NET Framework 3.5 SP1 download page](https://dotnet.microsoft.com/download/dotnet-framework/net35-sp1).

## Install on demand

You might see the following configuration dialog if you try to run an app that requires .NET Framework 3.5. Choose **Install this feature** to enable .NET Framework 3.5. This option requires an internet connection.

![Screenshot of the .NET Framework installation dialog.](./media/dotnet-35-windows/dotnet-framework-installation-dialog.png)

### Why am I getting this pop-up?

.NET Framework is created by Microsoft and provides an environment for running applications. There are different versions available. Many companies develop their apps to run using .NET Framework, and these apps target a specific version. If you see this pop-up, you're trying to run an application that requires .NET Framework version 3.5, but that version is not installed on your system.

## Enable in Control Panel

You can enable .NET Framework 3.5 through the Windows Control Panel. This option requires an internet connection.

1. Select the Windows key ![Screenshot of the Windows key logo.](./media/dotnet-35-windows/windows-keyboard-logo.png) on your keyboard, enter "Windows Features", and press <kbd>Enter</kbd>. The **Turn Windows features on or off** dialog box appears.

2. Select the **.NET Framework 3.5 (includes .NET 2.0 and 3.0)** check box, select **OK**, and reboot your computer if prompted.

   ![Screenshot showing installation of .NET with the control panel.](./media/dotnet-35-windows/dotnet-control-panel.png)

   You don't need to select the child items for **Windows Communication Foundation (WCF) HTTP Activation** and **Windows Communication Foundation (WCF) Non-HTTP Activation** unless you're a developer or server administrator who requires this functionality.

## Troubleshoot the installation

During installation, you might encounter error 0x800f0906, 0x800f0907, 0x800f081f, or 0x800F0922. To find out how to resolve these issues, see [.NET Framework 3.5 installation error: 0x800f0906, 0x800f0907, or 0x800f081f](https://support.microsoft.com/help/2734782/net-framework-3-5-installation-error-0x800f0906--0x800f081f--0x800f09).

If you still can't resolve your installation issue or you don't have an internet connection, you can try installing .NET Framework using your Windows installation media. For more information, see [Deploy .NET Framework 3.5 by using Deployment Image Servicing and Management (DISM)](/windows-hardware/manufacture/desktop/deploy-net-framework-35-by-using-deployment-image-servicing-and-management--dism). If you're using Windows 7, Windows 8.1, the latest release Windows 10, or Windows 11, but you don't have the installation media, create an up-to-date installation media here: [Create installation media for Windows](https://support.microsoft.com/help/15088/windows-create-installation-media). Additional information about Windows 11 and Windows 10 Features on Demand: [Features on Demand](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).

> [!WARNING]
> If you don't rely on Windows Update as the source for installing .NET Framework 3.5, make sure that you use sources strictly from the same corresponding Windows operating system version. Using sources from a different Windows operating system version will either install a mismatched version of .NET Framework 3.5 or cause the installation to fail, leaving the system in an unsupported and unserviceable state.
