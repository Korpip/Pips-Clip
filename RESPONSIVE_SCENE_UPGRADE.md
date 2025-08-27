# Responsive Scene Upgrade Guide

This document provides instructions for upgrading to the new responsive overlay system that includes the "Show Overlay Safely" task and improved scene management.

## New Features

### Show Overlay Safely Task

The new "Show Overlay Safely" task provides a centralized way to display overlay scenes while preventing conflicts and blackouts. This task:

- Hides all other overlay scenes before showing the requested one
- Calculates responsive positioning and sizing based on screen dimensions
- Waits for the scene to be closed before cleaning up
- Ensures proper scene management to prevent display issues

### Overlay Scene Names

The system includes the following exact overlay scene names:

- **Strip URL** - URL processing and stripping functionality
- **Clipboard List** - Main clipboard history display
- **Fav List** - Favorites list management
- **Emoji** - Emoji and emoticon picker
- **Word Check** - Word lookup and spell checking
- **Edit Box** - Text editing interface

## Installation Instructions

### Importing the XML

1. Download the `responsive_pips_clip_tasker.xml` file
2. Open Tasker on your Android device
3. Go to the main menu (three dots) → Data → Import
4. Select the `responsive_pips_clip_tasker.xml` file
5. Choose to import all tasks and scenes
6. Enable the imported profile if prompted

### Testing the Import

1. After import, check that the following tasks are available:
   - "Clipboard Scene Init"
   - "Show Overlay Safely"

2. Verify that all overlay scenes are present:
   - Strip URL
   - Clipboard List
   - Fav List
   - Emoji
   - Word Check
   - Edit Box

3. Test the "Show Overlay Safely" task:
   - Run the task with a scene name as parameter (e.g., %sceneName = "Clipboard List")
   - Verify the scene appears with proper positioning
   - Ensure other overlays are hidden while the scene is active

## Troubleshooting

### Black Screen or Blackouts

If you experience black screens or display blackouts:

1. **Check Scene Backgrounds**: Ensure all scene backgrounds are set to transparent
   - Open each scene in Tasker's scene editor
   - Set Background → Colour to transparent (alpha = 0)
   - Save and test again

2. **Verify Screen Dimensions**: The system calculates positioning based on screen size
   - Run "Clipboard Scene Init" task manually
   - Check that %SW and %SH variables contain your actual screen dimensions
   - If incorrect, restart Tasker and try again

3. **Scene Conflicts**: Multiple overlays showing simultaneously can cause issues
   - Use only the "Show Overlay Safely" task to display scenes
   - Avoid calling Scene → Show Scene directly for overlay scenes

### Scene Positioning Issues

If scenes appear in wrong positions or sizes:

1. **Recalculate Variables**: Run the "Clipboard Scene Init" task to refresh screen calculations
2. **Check Design Width**: The system assumes a design width of 1080px - adjust %DESIGNW if needed
3. **Verify Percentage Calculations**: Default is 80% width, 50% height - modify %W and %H calculations if desired

## Technical Details

### Transparent Backgrounds

All overlay scenes should have transparent backgrounds to prevent blackouts:

- Background color alpha should be set to 0 (fully transparent)
- This allows the underlying interface to remain visible
- Prevents the black screen issue common with solid backgrounds

### Scene→Wait Usage

The "Show Overlay Safely" task uses Scene→Wait to:

- Pause task execution until the scene is closed
- Ensure proper cleanup after scene interaction
- Prevent premature scene hiding

### Variable Calculations

The "Clipboard Scene Init" task calculates:

- `%SW` = Screen Width
- `%SH` = Screen Height  
- `%DESIGNW` = 1080 (design reference width)
- `%SCALE` = %SW / %DESIGNW (scaling factor)
- `%W` = (%SW * 80) / 100 (80% of screen width)
- `%H` = (%SH * 50) / 100 (50% of screen height)
- `%X` = (%SW - %W) / 2 (centered horizontally)
- `%Y` = 10 (10 pixels from top)

These variables ensure scenes scale appropriately across different device screen sizes.

## Support

If you continue to experience issues after following this guide:

1. Verify your Tasker version is compatible (5.8+ recommended)
2. Check that all scenes have proper transparent backgrounds
3. Ensure no conflicting tasks are calling overlay scenes directly
4. Try restarting Tasker and re-importing the XML if problems persist