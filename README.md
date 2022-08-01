# How to use any resolution in DX12 Games.
This repository explains and guides on how to use any resolution with DX12 Games.

## What is the problem with DX12?
With DX12, Microsoft was able to basically make performance between FSB/Windowed and FSE the same.            
But additionally they fully removed the ability to use FSE from their API.         

FSE has its pros and cons but one of its most useful features was the ability to render a game at any supported display resolution for better performance. But since DX12 doesn't have the ability to this, modern games perform terribly on low end hardware.

Some games offer a ingame render resolution slider to change the resolution ingame 3D models are rendered.
In 90% this silder is enough to increase performance but the rest 10% can get better performance by rendering their entire screen at a lower display resolution.

## How can one reintroduce the ability to render a game at a low display resolution?

Lets first understand, what's different between FSE and FSB/Windowed Mode.

FSE: Essentially the game can fully bypass DWM and has full access to the GPU itself.
FSB: Mimicks FSE but the game doesn't bypass DWM and doesn't have full access to the GPU.

With FSE, the game can tell your GPU to "Hey! Set the resolution to 1280x720!" and according your GPU responds to those changes.
With FSB, the game cannot tell your GPU to use a resolution of 1280x720 since its still fundamentally rendering as a window and cannot bypass DWM.

Here is the thing, FSB games will take on the resolution of what DWM returns so fundamentally if one changes their desktop resolution, we can force a game to scale down and use that resolution.

Of course, doing this manually would be a hassle and automation is a must.

### How can this process be automated?

[Resolution Enforcer](https://github.com/aetopia/resolution-enforcer) is a simple tool which allows you to enforce specfic resolutions on specific applications. Works with UWP and Win32 apps with ease.

Let's see how this process can be automated.

1. First, we constantly look for the current active/foreground window.
2. Once, we find a XYZ window then we call for `ChangeDisplaySettings` from the `win32api` to enforce a resolution.
3. Incase the window, we found becomes inactive or the user switches windows then we revert back to the native resolution.
4. We run this process in a loop.

The Resolution Enforcer exactly does this.
