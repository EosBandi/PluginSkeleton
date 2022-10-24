# Mission Planner Plugin Skeleton

A skeleton class library to create a Mission Planner plugin.

## Table of contents
 1. [Table of contents](#table-of-contents)
 2. [How to add to Visual Studio](#how-to-add-to-visual-studio)
 3. [How to build](#how-to-build)
 4. [How to develop](#how-to-develop)
    1. [string Name, Version, Author](#string-name-version-author)
    2. [bool Init()](#bool-init)
    3. [bool Loaded()](#bool-loaded)
    4. [bool Loop()](#bool-loop)
    5. [bool Exit()](#bool-exit)
    6. [Access main thread](#access-main-thread)

## How to add to Visual Studio

1. Clone this repo to `<Mission Planner local repo folder>\Plugins\`
   - Example result URL`: `C:\Users\<user>\Documents\GitHub\MissionPlanner\Plugins\PluginSkeleton\`
2. Add `Skeleton_rename_me.csproj` to `Plugins` folder in Visual Studio
   1. Right click on `Plugins` folder in the `Solution Explorer`
   2. Add
   3. Existing Item...
   4. Find and select `Skeleton_rename_me.csproj`
3. Rename everything
   - `Project` file in the Plugins folder in the Solution Explorer
   - `.cs` file in the project
   - `Namespace` inside .cs file
   - `class` inside .cs file

    Right click on the project in the Solution Explorer, select `Properties`, go to the `Application` tab
   - Value under `Assembly name`
   - Value under `Default namespace`

## How to build

Build the entire solution
1. Click `Build` on the top menu strip
2. Build Solution

Or build just the plugin project alone
1. Right click the Project under `Plugins` in the `Solution Explorer`
2. Build
   
   Or push **Ctrl+B**

Result DLLs are by default saved to:
 - Debug build: `<Mission Planner local repo folder>\bin\Debug\net<.Net version number>\Skeleton_rename_me.dll`
 - Release build: `<Mission Planner local repo folder>\bin\Release\net<.Net version number>\Skeleton_rename_me.dll`

File names will be whatever is set in Project Properties as the `Assembly name`.

## How to develop

The skeleton contains a selection of overrides from `MissionPlanner/Plugin/Plugin.cs`

### string Name, Version, Author

These show up in the the Plugin Manager window (**Ctrl+P**)

### bool Init()

This function runs when the plugin DLL is loaded by the plugin loader during the startup process.

Here is where the variable `loopratehz` is set by default. It dictates how often (per second) the function `Loop` will run.

**The maximum value of `loopratehz` is 100. Setting a value higher will not make it run faster than 100Hz!**

Some setup logic may be placed here, but since the UI isn't set up yet, nothing can be accessed or added.

### bool Loaded()

This function runs when the plugin is activated by the plugin loader.

Most of the logic should be placed here, since the UI is set up and accessible at this point.

To access UI or data elements from the main thread (where the main application is running), check out the [Access main thread](#access-main-thread) section below.

### bool Loop()

Code here will run periodically, in the rate set by `loopratehz` (times per second)

**The maximum value of `loopratehz` is 100. Setting a value higher will not make it run faster than 100Hz!**

To access UI or data elements from the main thread (where the main application is running), check out the [Access main thread](#access-main-thread) section below.

### bool Exit()

This function is called when the plugin is unloaded.

**This migh not be when the main application is closing, closing filestreams and such might result in exceptions being thrown!**

### Access main thread

The plugin runs on a different thread than the main window (**MainV2**). Control elements can be accessed by dispatching your code to the thread where the main window is running:

```cs
MainV2.instance.BeginInvoke((MethodInvoker)(() =>
{
    Label timeLabel = Host.MainForm.FlightData.Controls.Find("label1", true).FirstOrDefault() as Label;
    timeLabel.Text = DateTime.Now.ToLongTimeString();
}));
```

In this example, `label1` is the `Name` property of Label "`timeLabel`", which is on the FlightData screen.

Access elements from the main thread like this:

 - `Host.MainForm`: Main form class (MainV2)
 - `Host.cs`: Current stats of the MAV (CurrentState)
 - `Host.comPort`: Mavlink functions (MAVLinkInterface)
 - `Host.config`: XML settings (Settings)
 - `Host.FDGMapControl`: Map control in flightdata (myGMAP)
 - `Host.FDMenuMap`: Flightdata map context menu (ContextMenuStrip)
 - `Host.FDMenuMapPosition`: Point where the context menu was drawn on FD map (PointLatLng)
 - `Host.FDMapType`: Map provider of the FD map (GMapProvider)
 - `Host.FDMenuHud`: HUD context menu (ContextMenuStrip)
 - `Host.FPGMapControl`: Map control in flightplanner (myGMAP)
 - `Host.FPMenuMap`: Flight planner map context menu (ContextMenuStrip)
 - `Host.FPMenuMapPosition`: Point where the context menu was drawn on FP map (PointLatLng)
 - `Host.FPDrawnPolygon`: Polygon drawn by the user on the FP page (GMapPolygon)