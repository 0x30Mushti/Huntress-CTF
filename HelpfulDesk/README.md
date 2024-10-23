# HelpfulDesk

## Description 

```
HelpfulDesk is the go-to solution for small and medium businesses who need remote monitoring and management. Last night, HelpfulDesk released a security bulletin urging everyone to patch to the latest patch level. They were scarce on the details, but I bet that can't be good...
```

## How to solve the CTF:

Start the container and visit the website.

You'll be greeted by the HomePage, featuring a login page.

Navigate to the Security Bulletins tab.

I downloaded the Update 1.2 (Critical Severity) zip-archive.

## Thought process. 

The downloaded file contains numerous files, maps, etc. To save time, I focused on what could be related to extracting a flag, filtering out the rest.

I zeroed in on the file named HelpfulDesk.dll.

Given its name, it seems like the primary DLL file and could serve as a point of entry for getting the flag. If it didn’t yield anything, I would proceed with other files.

## Analyzing the dll.

To analyze the HelpfulDesk.dll file, I used the file command, which provided the following result:

```
file Helpfuldesk.dll 

HelpfulDesk.dll: PE32 executable (console) Intel 80386 Mono/.Net assembly, for MS Windows, 3 sections
```

For .NET binaries, I prefer using [dnspy](https://github.com/dnSpy/dnSpy) because it's user-friendly and has an intuitive interface.

I opened the DLL with dnSpy and manually inspected various classes. Eventually, I came across the class named:

``SetupController``

Here is the code:

```
using System;
using System.Collections.Generic;
using System.IO;
using System.Runtime.CompilerServices;
using System.Text.Json;
using HelpfulDesk.Models;
using HelpfulDesk.Services;
using Microsoft.AspNetCore.Mvc;

namespace HelpfulDesk.Controllers
{
	// Token: 0x0200001F RID: 31
	[NullableContext(1)]
	[Nullable(0)]
	public class SetupController : Controller
	{
		// Token: 0x060000F9 RID: 249 RVA: 0x000041AC File Offset: 0x000023AC
		public IActionResult SetupWizard()
		{
			if (File.Exists(this._credsFilePath))
			{
				string requestPath = base.HttpContext.Request.Path.Value.TrimEnd('/');
				if (requestPath.Equals("/Setup/SetupWizard", StringComparison.OrdinalIgnoreCase))
				{
					return this.View("Error", new ErrorViewModel
					{
						RequestId = "Server already set up.",
						ExceptionMessage = "Server already set up.",
						StatusCode = 403
					});
				}
			}
			return this.View();
		}

		// Token: 0x060000FA RID: 250 RVA: 0x0000422C File Offset: 0x0000242C
		[HttpPost]
		public IActionResult SetupWizard(string username, string password)
		{
			string filePath = Path.Combine(Directory.GetCurrentDirectory(), "credentials.json");
			List<AuthenticationService.UserCredentials> credentials = new List<AuthenticationService.UserCredentials>
			{
				new AuthenticationService.UserCredentials
				{
					Username = username,
					Password = password,
					IsAdmin = true
				}
			};
			string json = JsonSerializer.Serialize<List<AuthenticationService.UserCredentials>>(credentials, null);
			File.WriteAllText(filePath, json);
			return this.RedirectToAction("SetupComplete");
		}

		// Token: 0x060000FB RID: 251 RVA: 0x00004289 File Offset: 0x00002489
		public IActionResult SetupComplete()
		{
			return this.View();
		}

		// Token: 0x0400009F RID: 159
		private readonly string _credsFilePath = "credentials.json";
	}
}
```

## Exploiting the Code

The code defines a SetupController class in an ASP.NET Core web application, which manages the initial setup process of the server. It includes three key methods:

SetupWizard() (GET): Checks if the credentials file (credentials.json) already exists. If the file exists and the request path matches /Setup/SetupWizard, it returns an error view, signaling that the server has already been set up. Otherwise, it shows the setup form.

SetupWizard() (POST): This is triggered when the setup form is submitted. It accepts the username and password, generates a list of user credentials (marking the user as an admin), serializes the data into JSON format, and saves it to credentials.json. The user is then redirected to the "SetupComplete" page.

The setup can only be run once, as the existence of the credentials file blocks further attempts.

## Exploit Plan
By analyzing the code, I identified the setup URL:
http://challenge.ctf.games:30842/Setup/SetupWizard

However, this results in a 403 error. Upon reviewing the code further, I discovered this snippet:

```
if (requestPath.Equals("/Setup/SetupWizard", StringComparison.OrdinalIgnoreCase))
{
    return this.View("Error", new ErrorViewModel
    {
        RequestId = "Server already set up.",
        ExceptionMessage = "Server already set up.",
        StatusCode = 403
    });
}
```
The 403 error occurs because the code checks for an exact path match (/Setup/SetupWizard). Adding a trailing slash (/Setup/SetupWizard/) bypasses this check due to a path normalization issue.

Let's try this URL instead:
```http://challenge.ctf.games:30842/Setup/SetupWizard/```

Success! We now see:


So lets head to ```http://challenge.ctf.games:30842/Setup/SetupWizard/```

Success! We now see:
```
Welcome to the HelpfulDesk Setup Wizard!
Please set the Administrator's username and password below to whatever you want:
```

## Creating a account and extracting the flag:

I created an administrator account and returned to the HomePage.

After logging in with the new credentials, I accessed the filesystem for the host ```HOST-WIN-DX130S2.```

Within the filesystem, I found the file:
```C:\Users\administrator\Desktop\flag.txt```

I downloaded and opened the file, and the flag was inside flag.txt.


