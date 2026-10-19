Release: LumenS-v1.01
Build: optimized Release, debug symbols omitted. Package folder version v1.01; assembly metadata remains 1.0.0.0.
Protection status: Dotfuscator Community name renaming applied; WPF XAML transformation enabled. The obfuscation map is not included in this distribution.

LumenS-v1.01 — single-player camera keyframe capture
By Shxdow.v1

This package includes the .NET 9 runtime it needs, so a separate .NET installation should not be required. Double-click the LumenS shortcut in this folder; the application files are grouped in LumenS App. A launch screen appears before the workspace. On that screen, click the console-key box and press the key used to open ARK's console (Tab by default). Esc continues with the saved key, or Tab on first launch.

Quick start:
1. Load Steam ARK: Survival Evolved single-player. LumenS detects the running game automatically.
2. In Keybinds, choose Initialize camera tracking. This setup uses the console to move the spectator camera to a temporary calibration point at (1423.14, 0, 0), then finds the camera pointer.
3. Move the spectator camera to each desired shot and press the configured keyframe hotkey while ARK is focused. Change that hotkey in Keybinds. Camera values are read from ARK memory.
4. In Keyframes, press Play to interpolate position, yaw, pitch, and FOV at up to 60 updates per second by writing grouped camera values to ARK memory. Pause holds the current camera pose. Click or drag on the timeline, or click a keyframe diamond, to move the ARK camera to that path point. The first playback or scrub asks for confirmation before enabling camera memory writes. Choose Don't remind me again on future launches to save that preference; leaving it unchecked shows the warning again next time LumenS starts.
5. In Render, choose an output folder and click Render for an immediate render, or Add to queue to save this setup for later. In Queue, start queued jobs to render them one after another. Completed timelines stay in the queue. Click New timeline to save the current path and begin an empty one; double-click a saved timeline to reopen it. The queue is saved between launches under %LOCALAPPDATA%\LumenS.

Camera memory writes and rendering are intended only for single-player with BattlEye disabled. This does not establish or guarantee VAC/anti-cheat safety. Roll is applied to ARK while playing, scrubbing, and rendering. LumenS resets ARK roll to 0° when the app closes, when camera memory is still available. Frames are captured with Windows Graphics Capture targeted at ARK's window handle; LumenS does not copy the desktop. Keep ARK open in windowed or borderless mode and do not minimize it. Other windows can cover ARK without being captured. Overlays rendered inside ARK itself may still appear.

Render frames exclude the mouse cursor. LumenS applies MXGlider's client-area crop (10 px left, 30 px top, 10 px bottom) before resizing each frame. This removes the top-left FPS label and thin edge artifacts shown in the capture; other overlays positioned outside that crop can still appear. If Windows Graphics Capture temporarily stops supplying frames, LumenS retries and falls back to MXGlider's window-targeted PrintWindow capture; it does not capture the desktop.

Rendering uses a dropdown for 100–1,000 frame intervals per adjacent keyframe pair, in steps of 100 (default 500). Six keyframes at the default produce 2,501 frames, about 41.7 seconds at 60 fps. Rendered keyframe intervals are evenly timed; changing the output frame rate changes the clip duration.

Pathing has two modes. Normal follows each segment's Smooth or Linear setting. Smooth uses editable spline curves; Linear moves at a constant rate. Aggressive (MXGlider) uses the same uniform Catmull–Rom formula and per-keyframe sampling approach found in MXGlider for position, yaw, pitch, and FOV. Like MXGlider, LumenS now tracks yaw continuously while ARK is focused and stores turns across the 0°/360° boundary, so a captured full turn is not lost when its ending angle matches its starting angle. MXGlider has no roll channel; in Aggressive mode, each whole 360° turn entered on a keyframe is treated as a roll event eased across the neighboring keyframes, while any remaining angle is treated as the keyframe's pose. The event starts and finishes at rest, and a following 0° holds rather than starting another turn. During Aggressive camera writes, LumenS temporarily widens ARK's camera roll limits to ±720° and restores the original limits when the memory session closes. Aggressive pathing uses smooth interpolation for the entire path; the per-segment Smooth/Linear switch is disabled in that mode. Exported paths include the selected mode, and older keyframe-array JSON files load as Normal.

Roll curves ease in and out at the path ends. Normal mode uses monotone roll tangents to reduce overshoot. Aggressive full turns start and finish at rest within the neighboring-keyframe span; spline handles can shape the curve without extending the turn beyond those keys. Normal paths that exceed ARK's default roll limits temporarily widen the limits and restore them when the camera memory session closes.

In Normal mode, a whole 360° turn marked on a keyframe now also spans the previous, marked, and next keyframes when available. Partial roll values continue to act as camera poses.

Queue entries show their saved pathing mode. Select an entry and press F2 to rename it; double-click to reopen it.

After adding a timeline to Queue, the confirmation reports the mode saved with that snapshot.

The Camera Preview panel shows a reduced-size live view of ARK's window at a low update rate. Preview capture pauses during rendering to leave the capture device available to the render.

Keep ARK open in windowed or borderless mode during rendering; it can be covered by other windows but must not be minimized. Frames are resized to the selected output dimensions; choosing a larger output size does not add detail beyond the game's captured resolution. The render frame-ready check waits up to 300 ms when the captured scene has not changed.

The console key is saved at %LOCALAPPDATA%\LumenS\console-key.json, the camera pointer at %LOCALAPPDATA%\LumenS\ark-camera-pointer.json, and the keyframe hotkey at %LOCALAPPDATA%\LumenS\capture-hotkey.json. The optional camera-write warning preference is saved at %LOCALAPPDATA%\LumenS\camera-memory-warning.json. Reinitialize tracking after an ARK update or if memory reads/writes fail. Camera paths can be exported or loaded as JSON.


Keyframe list entries now show [Roll] when a keyframe has a nonzero roll value. In Aggressive pathing, roll remains LumenS-specific and is interpolated separately from MXGlider's pathing.


Render queue: Add to queue stores a snapshot of the keyframes, pathing mode, output folder, frame rate, interval frame count, resolution, and render options. Start queue renders those saved jobs in order. The first job shows the normal render confirmation; later jobs in the same queue run automatically. Cancel render stops the active job and leaves later jobs queued.

Weather commands in the Commands tab:
Choose the map currently loaded in ARK, choose one of its supported weather events, and use Start weather or Stop weather. The available events are map-specific; stop is disabled for events that ARK does not list a stop command for. These controls send ARK's CE console commands and are intended for single-player with BattlEye disabled.

World save/INI tab:
Choose a destination from the official map list or a discovered custom map. Each official ARK: Survival Evolved map uses its map-specific save folder (The Center uses ShooterGame\\Saved\\TheCenterSavedArksLocal); The Island uses SavedArksLocal. Select a replacement .ark file and LumenS backs up the current save, then writes the selected file under the exact map filename. Save replacement does not require closing ARK, but wait until ARK has finished saving that world. The player profile remains as it was. The installed-mod list is a reference; enable and order mods in ARK's own Mods menu. LumenS does not edit ActiveMods or download Workshop mods. INI profile changes are the only operation here that requires ARK to be closed.
Saving an INI profile stores a copy of Engine\\Config\\BaseDeviceProfiles.ini. Applying a saved profile restores that file only; GameUserSettings.ini, Game.ini, and Engine.ini are left alone. Pretty INI replaces the Windows device-profile CVar entries with exactly these three lines: +CVars=ShowFlag.Materials=0, +CVars=FogDensity=0.0, and +CVars=FX.MaxCPUParticlesPerEmitter=1. The previous BaseDeviceProfiles.ini is backed up under LumenS Backups before changes. Profiles can be deleted from the profile selector.


