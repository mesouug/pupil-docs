+++
date = "2017-01-19T13:08:13+07:00"
title = "pupil player"
weight = 7
+++

<div class="header-border-top"></div>
<div class="content-container">
  <div class="header-link">
    <a href="#pupil-player">
      <h2 id="pupil-player">Pupil Player</h2>
    </a>
  </div>
</div>

<p align="center">
  <img class="padTop--2" src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/player/player-marker-demo.png" width="85%">
</p>

<div class="content-container">
  <div class="header-link">
    <a href="#player-about">
      <h3 id="player-about">About</h3>
    </a>
  </div>
</div>
<div class="header-border-bottom"></div>

Pupil Player is the second tool you will use after Pupil Capture. It is a media and data visualizer at its core. You will use it to look at Pupil Capture recordings. Visualize your data and export it.

Features like <a href="#marker-tracking">surface tracking</a> found in Pupil Capture are also available in Pupil Player.

<div class="content-container">
  <div class="header-link">
    <a href="#start-player">
      <h3 id="start-player">Starting Pupil Player</h3>
    </a>
  </div>
</div>
<div class="header-border-bottom"></div>

Drag the recording directory (the triple digit one) directly onto the app icon **or** launch the application and drag + drop the recording directory into Pupil Player window.

<p align="center">
    <img src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/basic-workflow/pupil-capture/icon/pupil-player-pointer.png" width="20%">
</p>

> Running from source?

```bash
cd "path_to_pupil_dir/pupil_src/player"
python main.py "path/to/recording_directory"
```

<div class="content-container">
  <div class="header-link">
    <a href="#player-workflow">
      <h4 id="player-workflow">Workflow</h4>
    </a>
  </div>
</div>

Pupil Player is similar to a video player. You can playback recordings and can load plugins to build visualizations.

Here is an example workflow:

  + Start Pupil Player
  + Opening a Plugin - From the `Settings` GUI menu load the `Vis Circle` plugin.
  + Playback - press the play button or `space` bar on your keyboard to view the video playback with visualization overlay, or drag the playhead in the seek bar to scrub through the dataset.
  + Set trim marks - you can drag the small circles on the ends of the seek bar. This will set the start and end frame for the exporter.
  + Export Video & Raw Data - Load the `Video Export Launcher` plugin and the `Raw Data Exporter` plugin. Press `e` on your keyboard or the `e` button in the left hand side of the window to start the export.
  + Check out exported data in the `exports` directory within your recording directory

<aside class="notice">
Note - Pupil Player will <strongn>ever</strongn> remove or overwrite any of your raw data gathered during capture. All exports are isolated within a sub-directory named <code>exports</code>.
</aside>

<div class="content-container">
  <div class="header-link">
    <a href="#plugin-overview">
      <h3 id="plugin-overview">Plugin Overview</h3>
    </a>
  </div>
</div>
<div class="header-border-bottom"></div>

Pupil Player uses the same Plugin framework found in Pupil Capture to add functionality.

We implement all visualizations, marker tracking, and the exporter using this structure. Very little work (often no work) needs to be done to make a Capture Plugin work for the Pupil Player and vice versa.

There are two general types of plugins:

  + **Unique** - You can only launch one instance of this plugin.
  + **Not unique** - You can launch multiple instances of this type of plugin. For example, you can load one `Vis Circle` plugin to render the gaze position with a translucent green circle, and another `Vis Circle` plugin to render the gaze circle with a green stroke of 3 pixels thickness. You can think of these types of plugins as *additive*.

In the following sections we provide a summary of plugins currently available and in Pupil Player.

<div class="content-container">
  <div class="header-link">
    <a href="#vis-plugin-utils">
      <h3 id="vis-plugin-utils">Visualization Plugins and Utilities</h3>
    </a>
  </div>
</div>
<div class="header-border-bottom"></div>

For the sake of clarity, we will call plugins with the `Vis` prefix **visualization** plugins. These plugins are simple plugins, are mostly additive ( or *not unique*), and directly operate on the gaze positions to produce visualizations. Other plugins like `Offline Marker Detector` also produces visualizations, but will be discussed elsewhere due to the extent of its features.    

<div class="content-container">
  <div class="header-link">
    <a href="#vis-circle">
      <h4 id="vis-circle">Vis Circle</h4>
    </a>
  </div>
</div>

Visualize the gaze positions with a circle for each gaze position. This plugin is **not unique**, therefore you can add multiple instances of the plugin to build your visualization.

You can set the following parameters:

  + `radius` - the radius of the circle around the gaze point.
  + `stroke width` - the thickness or width of the stoke in pixels.  
  + `fill` - toggle on for a circle with solid fill. Toggle off for a circle with only stroke.
  + `color` - define the `red`, `green`, `blue` values for color. `Alpha` defines the opacity of the stroke and fill.  

<p align="center">
  <img src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/player/pupil-player-vizcircle.png" width="85%">
</p>

Here we show an example of how you could use **2** instances of the `Vis Circle` Plugin. The first instance renders the gaze position as a filled yellow circle. The second instance renders the same gaze position as an orange stroke circle.

<div class="content-container">
  <div class="header-link">
    <a href="#vis-cross">
      <h4 id="vis-cross">Vis Cross</h4>
    </a>
  </div>
</div>

Visualize the gaze positions with a cross for each gaze position. This plugin is **not unique**, therefore you can add multiple instances of the plugin to build your visualization. You can set the following parameters:

  + `inner offset length` - the distance in pixels to offset the interior cross endpoints from the gaze position. A value of `0` will make the crosshairs intersect the gaze position.  
  + `outer length` - The length of the cross lines in pixels from the gaze position. Note - equal values of `inner offset length` and `outer length` will result in a cross with no length, and therefore not rendered.
  + `stroke width` - the thickness or width of the stoke in pixels.  
  + `color` - define the `red`, `green`, `blue` values for color.

<p align="center">
  <img src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/player/pupil-player-vizcross.png" width="85%">
</p>

Here we show an example of how you could use **2** instances of the `Vis Cross` Plugin. The first instance renders the gaze position as a red cross with that extends to the boundaries of the screen. The second instance renders the gaze position as a green cross, with a heavier stroke weight.


<div class="content-container">
  <div class="header-link">
    <a href="#scan-path">
      <h4 id="scan-path">Scan Path</h4>
    </a>
  </div>
</div>

This plugin enables past gaze positions to stay visible for the duration of time specified by the user. This plugin is **unique**, therefore you can only load one instance of this plugin.

On its own, `Scan Path` does not render anything to the screen. It is designed to be used with other plugins. In some cases, it is even required to be enabled in order for other plugins to properly function. When used with `Vis` plugins (like `Vis Circle`, `Vis Cross`, `Vis Polyline`, or `Vis Light Points`) `Scan Path` will enable you to see both the current gaze positions and the past gaze positions for the specified duration of time.     

<p align="center">
  <img src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/player/pupil-player-scanpath.png" width="85%">
</p>

Here we show an example of `Scan Path` set with `0.4` seconds duration used with `Vis Circle`. Each green circle is a gaze position within the last `0.4` seconds of the recording.

<div class="content-container">
  <div class="header-link">
    <a href="#vis-polyline">
      <h4 id="vis-polyline">Vis Polyline</h4>
    </a>
  </div>
</div>

Visualize the gaze positions with a polyline for each gaze position. This plugin is **not unique**, therefore you can add multiple instances of the plugin to build your visualization. You can set the following parameters:

  + `line thickness` - the thickness or width of the polyline stroke in pixels.  
  + `color` - define the `red`, `green`, `blue` values for color.

<p align="center">
  <img src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/player/pupil-player-vizpoly.png" width="85%">
</p>

An example showing `Vis Polyline` used with `Vis Circle` and `Scan Path`. The polyline enables one to visualize the sequence of the gaze positions over the duration specified by `Scan Path`.

<div class="content-container">
  <div class="header-link">
    <a href="#vis-lightpoints">
      <h4 id="vis-lightpoints">Vis Light Points</h4>
    </a>
  </div>
</div>

Visualize the gaze positions as a point of light for each gaze position. The `falloff` of the light from the gaze position is specified by the user. This plugin is **not unique**, therefore you can add multiple instances of the plugin to build your visualization. You can set the following parameters:

  + `falloff` - The distance (in pixels) at which the light begins to fall off (fade to black). A very low number will result in a very dark visualization with tiny white light points. A very large number will result in a visualization of the world view with little or no emphasis of the gaze positions.  

<p align="center">
  <img src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/player/pupil-player-vizlightpoints.png" width="85%">
</p>

Here is an example demonstrating `Vis Light Points` with a falloff of 73.

<div class="content-container">
  <div class="header-link">
    <a href="#gaze-correction">
      <h4 id="gaze-correction">Manual Gaze Correction</h4>
    </a>
  </div>
</div>

This plugin allows one to manually offset the gaze position. The offset values are between `-1` and `1`. This plugin is **unique**, therefore you can only load one instance of this plugin. You can set the following parameters:

  + `x_offset` - the amount to offset the gaze position horizontally
  + `y_offset` - the amount to offset the gaze position vertically

<div class="content-container">
  <div class="header-link">
    <a href="#eye-overlay">
      <h4 id="eye-overlay">Eye Video Overlay</h4>
    </a>
  </div>
</div>

This plugin can be used to overlay the eye video on top of the world video. Note that the eye video is not recorded by default in Pupil Capture, so if you want to use this plugin, make sure to check `record eye video` in Pupil Capture. This plugin is **unique**, therefore you can only load one instance of this plugin.

 You can set the following parameters:

  + `opacity` - the opacity of the overlay eye video image. `1.0` is opaque and `0.0` is transparent.
  + `video scale` - use the slider to increase or decrease the size of the eye videos.
  + `move overlay` - toggle `on` and then click and drag eye video to move around in the player window. Toggle `off` when done moving the video frames.
  + `show` - show or hide eye video overlays.
  + `horiz. and vert. flip` - flip eye videos vertically or horizontally

<p align="center">
  <img src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/player/pupil-player-eyeoverlay.png" width="85%">
</p>

Here is an example of the `Eye Video Overlay` with binocular eye videos.

<div class="content-container">
  <div class="header-link">
    <a href="#export">
      <h3 id="export">Export</h3>
    </a>
  </div>
</div>
<div class="header-border-bottom"></div>

You can export data and videos by pressing `e` on your keyboard or the `e` hot key button in the Pupil Player window.

All open plugins that have export capability will export when you press `e`. All exports are separated from your raw data and contained in the `exports` sub-directory. The exports directory lives within your recording directory.

<div class="content-container">
  <div class="header-link">
    <a href="#export-dir">
      <h4 id="export-dir">Exports directory</h4>
    </a>
  </div>
</div>

All exports are saved within the `exports` sub-directory within your recording directory.
A new directory will be created within the `exports` directory named with the `start` frame and `end` frame that is specified by the trim marks.

<p align="center">
  <img src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/basic-workflow/pupil-player/recording-directory/recording_folder_exports_v07.png" width="85%">
</p>

<div class="content-container">
  <div class="header-link">
    <a href="#export-launcher">
      <h4 id="export-launcher">Video Export Launcher</h4>
    </a>
  </div>
</div>

To export a video, load the `Export Video` plugin. You can select the frame range to export by setting trim marks in the seek bar or directly in the plugin GUI.

You can specify the name of the export in the GUI. Click press the `e` button or click `e` on your keyboard to start the export.

The exporter will run in the background and you can see the progress bar of the export in the GUI. While exporting you can continue working with Pupil Player and even launch new exports.

<p align="center">
  <img src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/player/pupil-player-export.png" width="85%">  
</p>

<div class="content-container">
  <div class="header-link">
    <a href="#raw-data-exporter">
      <h4 id="raw-data-exporter">Raw Data Exporter</h4>
    </a>
  </div>
</div>

To export `.csv` files of your data, load the `Raw Data Exporter` plugin. You can select the frame range to export by setting trim marks in the seek bar or directly in the plugin GUI.

Click press the `e` button or click `e` on your keyboard to start the export.

<p align="center">
  <img src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/player/player-rawexport.png" width="85%">
</p>

<div class="content-container">
  <div class="header-link">
    <a href="#offline-surface-tracker">
      <h4 id="offline-surface-tracker">Offline Surface Tracker</h4>
    </a>
  </div>
</div>

This plugin is an offline version of the [Surface Tracking](#marker-tracking) plugin for Pupil Capture. You can use this plugin to detect markers in the recording, define surfaces, edit surfaces, and create and export visualizations of gaze data within the defined surfaces.

<p align="center">
  <img src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/player/player-marker-demo.png" width="85%">
</p>

Here is an example workflow for using the `Offline Surface Detector` plugin to generate heatmap visualizations and export surface data reports:

  + Load `Offline Surface Detector` plugin - if you already have surfaces defined, the load may take a few seconds because the plugin will look through the entire video and cache the detected surfaces.
  + Add surface - if you do not have any defined surfaces, you can click on the `Add surface` button when the markers you want to user are visible or just click the circular `A` button in the left hand side of the screen.
  + Surface name and size - In the `Marker Detector` GUI window, define the surface name and real world size. *Note* - defining size is important as it will affect how heatmaps are rendered.
  + Set trim marks - optional, but if you want to export data for a specific range, then you should set the trim marks.
  + Recalculate gaze distributions - click the `(Re)calculate gaze distributions` button after specifying surface sizes. You should now see heatmaps in the Player window (if gaze positions were within your defined surfaces).
  + Export gaze and surface data - click `e` and all surface metrics reports will be exported and saved for your trim section within your `export` folder.

<div class="content-container">
  <div class="header-link">
    <a href="#fixation-dispersion">
      <h4 id="fixation-dispersion">Fixation Detector - Dispersion Duration</h4>
    </a>
  </div>
</div>

This plugin detects fixation based on a dispersion threshold in terms of degrees of visual angle. This plugin is **unique**, therefore you can only load one nstance of this plugin.

<p align="center">
  <img src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/player/player-2d-fixation.png" width="85%">
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/wiki/pupil-labs/pupil/media/player/player-3d-fixation.png" width="85%">
</p>

Toggle `Show fixations` to show a visualization of fixations. The blue number is the number of the fixation (0 being the first fixation). You can export fixation reports for your current trim section by pressing `e` on your keyboard or the `e` hot key button in the left hand side of the window.

<div class="content-container">
  <div class="header-link">
    <a href="#batch-exporter">
      <h4 id="batch-exporter">Batch Exporter</h4>
    </a>
  </div>
</div>

You can use this plugin to apply visualizations to an entire directory (folder) of recordings in one batch. You need to specify the following:

  + `Recording source directory` - a directory (folder) that contains one or more Pupil recording folder.
  + `Recording destination directory` - an existing directory (folder) where you want to save the visualizations.

<div class="content-container">
  <div class="header-link">
    <a href="#develop-plugin">
      <h3 id="develop-plugin">Developing your own Plugin</h3>
    </a>
  </div>
</div>
<div class="header-border-bottom"></div>

To develop your own plugin see the <a href="#plugin-guide">developer guide</a>.


