# tiny11builder
*Scripts to build a trimmed-down Windows 11 image - now in **PowerShell**!*

> **Note:** This project now supports **Windows 11 25H2** with updates to remove new AI-powered bloatware including Recall, Click to Do, and additional telemetry settings.

## Introduction :
Tiny11 builder, now completely overhauled. <br> After more than a year (for which I am so sorry) of no updates, tiny11 builder is now a much more complete and flexible solution - one script fits all. Also, it is a steppingstone for an even more fleshed-out solution.

You can now use it on ANY Windows 11 release (not just a specific build), as well as ANY language or architecture.
This is made possible thanks to the much-improved scripting capabilities of PowerShell, compared to the older Batch release.

This is a script created to automate the build of a streamlined Windows 11 image, similar to tiny10.
The script has also been updated to use DISM's recovery compression, resulting in a much smaller final ISO size, and no utilities from external sources. The only other executable included is **oscdimg.exe**, which is provided in the Windows ADK and it is used to create bootable ISO images. 
Also included is an unattended answer file, which is used to bypass the Microsoft Account on OOBE and to deploy the image with the `/compact` flag.
It's open-source, **so feel free to add or remove anything you want!** Feedback is also much appreciated.

Also, for the very first time, **introducing tiny11 core builder**! A more powerful script, designed for a quick and dirty development testbed. Just the bare minimum, none of the fluff. 
This script generates a significantly reduced Windows 11 image. However, **it's not suitable for regular use due to its lack of serviceability - you can't add languages, updates, or features post-creation**. tiny11 Core is not a full Windows 11 substitute but a rapid testing or development tool, potentially useful for VM environments.

---

## ⚠️ Script versions:
- **tiny11maker.ps1** : The regular script, which removes a lot of bloat but keeps the system serviceable. You can add languages, updates, and features post-creation. This is the recommended script for regular use.
- ⚠️ **tiny11coremaker.ps1** : The core script, which removes even more bloat but also removes the ability to service the image. You cannot add languages, updates, or features post-creation. This is recommended for quick testing or development use.

## Instructions:
1. Download Windows 11 from the [Microsoft website](https://www.microsoft.com/software-download/windows11) or [Rufus](https://github.com/pbatard/rufus)
2. Mount the downloaded ISO image using Windows Explorer.
3. Open **PowerShell 5.1** as Administrator. 
5. Change the script execution policy :
```powershell
Set-ExecutionPolicy Bypass -Scope Process
```
> Using `-Scope Process` you keep your original policy intact as this change only lasts for the current PowerShell session. 

6. Start the script :
```powershell
C:/path/to/your/tiny11/script.ps1 -ISO <letter> -SCRATCH <letter>
``` 
> You can see of the script by running the `get-help` command.

6. Select the drive letter where the image is mounted (only the letter, no colon (:))
7. Select the SKU that you want the image to be based.
8. Sit back and relax :)
9. When the image is completed, you will see it in the folder where the script was extracted, with the name tiny11.iso

---

## What is removed (Windows 11 25H2 comprehensive):

> **New in 25H2:** Full debloat including Recall, Click to Do, Windows AI Hub, Copilot, additional telemetry controls, and Group Policy app removal support.
<table>
  <tbody>
    <tr>
      <th>Tiny11maker</th>
      <th>Tiny11coremaker</th>
    </tr>
    <tr>
      <td>
        <ul>
          <li>3D Builder</li>
          <li>Clipchamp</li>
          <li>Copilot / Windows Copilot</li>
          <li>Cortana</li>
          <li>Dolby Access</li>
          <li>Feedback Hub</li>
          <li>Gaming App (Xbox)</li>
          <li>Get Help</li>
          <li>Get Started</li>
          <li>LinkedIn</li>
          <li>Mail and Calendar</li>
          <li>Maps</li>
          <li>Media Player</li>
          <li>Microsoft News</li>
          <li>Microsoft Office Hub</li>
          <li>Microsoft Photos</li>
          <li>Microsoft Solitaire Collection</li>
          <li>Microsoft Sticky Notes</li>
          <li>Mixed Reality Portal</li>
          <li>Office OneNote</li>
          <li>Outlook for Windows</li>
          <li>Paint</li>
          <li>People</li>
          <li>Power Automate Desktop</li>
          <li>Quick Assist</li>
          <li>Screen Sketch</li>
          <li>Skype</li>
          <li>Sound Recorder</li>
          <li>Teams</li>
          <li>To Do</li>
          <li>Wallet</li>
          <li>Weather</li>
          <li>Windows AI Hub</li>
          <li>Windows Calculator</li>
          <li>Windows Camera</li>
          <li>Windows Dev Home</li>
          <li>Windows Feedback Hub</li>
          <li>Windows Notepad</li>
          <li>Windows Recovery</li>
          <li>Windows Screen Sketch</li>
          <li>Windows Terminal</li>
          <li>Windows Web Experience</li>
          <li>Your Phone</li>
          <li>Zune Music and Video</li>
          <li>Edge</li>
          <li>OneDrive</li>
        </ul>
        <strong>Registry Optimizations:</strong>
        <ul>
          <li>Bypass TPM/CPU/RAM/SecureBoot checks</li>
          <li>Disable Windows Defender (optional)</li>
          <li>Disable telemetry and diagnostic data</li>
          <li>Disable Cortana and Bing search</li>
          <li>Disable Recall (25H2)</li>
          <li>Disable Click to Do (25H2)</li>
          <li>Disable Copilot</li>
          <li>Disable sponsored apps</li>
          <li>Enable local account bypass</li>
          <li>Disable reserved storage</li>
          <li>Disable BitLocker</li>
          <li>Disable DiagTrack service</li>
          <li>Disable Delivery Optimization</li>
          <li>Disable Windows Error Reporting</li>
          <li>Disable advertising info</li>
          <li>Group Policy app removal (25H2)</li>
        </ul>
      </td>
      <td>
        <ul>
          <li>all from regular tiny +</li>
          <li>Windows Component Store (WinSxS)</li>
          <li>Windows Defender (optional disable)</li>
          <li>Windows Update (disabled for image stability)</li>
          <li>WinRE</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Keep in mind that **you cannot add back features in tiny11 core**! <br>
You will be asked during image creation if you want to enable .net 3.5 support!

---

## Known issues:
- Although Edge is removed, there are some remnants in the Settings, but the app in itself is deleted. 
- You might have to update Winget before being able to install any apps, using Microsoft Store.
- Outlook and Dev Home might reappear after some time. This is an ongoing battle, though the latest script update tries to prevent this more aggressively.
- If you are using this script on arm64, you might see a glimpse of an error while running the script. This is caused by the fact that the arm64 image doesn't have OneDriveSetup.exe included in the System32 folder.

---

## Features to be implemented:
- ~~disabling telemetry~~ (Implemented in the 04-29-24 release!)
- ~~more ad suppression~~ (Partially implemented in the 09-06-25 release!)
- ~~Windows 11 25H2 support~~ (Implemented in the 16-04-26 release!)
- improved language and arch detection
- more flexibility in what to keep and what to delete
- maybe a GUI???

And that's pretty much it for now!
## ❤️ Support the Project

If this project has helped you, please consider showing your support! A small donation helps me dedicate more time to projects like this.
Thank you!

**[Patreon](http://patreon.com/ntdev) | [PayPal](http://paypal.me/ntdev2) | [Ko-fi](http://ko-fi.com/ntdev)**
Thanks for trying it and let me know how you like it!
