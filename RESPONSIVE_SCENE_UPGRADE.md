# Responsive Scene Upgrade Guide

This guide provides step-by-step instructions for making your Pips Clip scenes responsive across different Android device screen sizes by implementing dynamic scaling calculations.

## Overview

The responsive system works by:
1. Detecting the current screen dimensions
2. Calculating a scale factor based on a design reference size
3. Dynamically resizing and repositioning scene widgets
4. Showing the scene as a persistent overlay

## Prerequisites

### Required Permissions
Ensure Tasker has the following permissions:
- **System Alert Window**: Required for overlay scenes
- **Accessibility Service**: Needed for advanced screen interactions
- **Device Administrator** (optional): For enhanced system integration

### Tasker Setup
1. Enable "Allow External Access" in Tasker preferences
2. Set "UI Timeout" to a higher value (60+ seconds) for complex scenes
3. Ensure "Run In Foreground" is enabled for reliable operation

## Step-by-Step Implementation

### Step 1: Create the Clipboard Scene Init Task

1. **Open Tasker** and navigate to the Tasks tab
2. **Create a new Task** named "Clipboard Scene Init"
3. **Add the following actions in sequence:**

#### Action 1: Get Screen Width
- **Action**: Variable Set
- **Name**: `%SW`
- **To**: `%SCREENW`
- **Description**: Store current screen width

#### Action 2: Get Screen Height
- **Action**: Variable Set
- **Name**: `%SH` 
- **To**: `%SCREENH`
- **Description**: Store current screen height

#### Action 3: Set Design Reference Width
- **Action**: Variable Set
- **Name**: `%DESIGNW`
- **To**: `1080`
- **Description**: Reference design width (adjust based on your original design)

#### Action 4: Calculate Scale Factor
- **Action**: Do Maths
- **Variable**: `%SCALE`
- **Expression**: `%SW / %DESIGNW`
- **Description**: Calculate scaling factor based on current vs design width

#### Action 5: Calculate Scaled Width
- **Action**: Do Maths
- **Variable**: `%W`
- **Expression**: `round(%DESIGNW * %SCALE)`
- **Description**: Calculate final overlay width

#### Action 6: Calculate Scaled Height
- **Action**: Do Maths
- **Variable**: `%H`
- **Expression**: `round(480 * %SCALE)`
- **Description**: Calculate final overlay height (adjust 480 to your design height)

#### Action 7: Calculate X Position
- **Action**: Do Maths
- **Variable**: `%X`
- **Expression**: `round((%SW - %W) / 2)`
- **Description**: Center horizontally on screen

#### Action 8: Calculate Y Position
- **Action**: Do Maths
- **Variable**: `%Y`
- **Expression**: `round((%SH - %H) / 4)`
- **Description**: Position in upper portion of screen

#### Action 9: Show Scene
- **Action**: Scene → Show Scene
- **Name**: `PipsClipScene`
- **Display As**: `Overlay`
- **Horizontal Position**: `%X`
- **Vertical Position**: `%Y`
- **Width**: `%W`
- **Height**: `%H`
- **Show Exit Button**: `On`
- **Timeout (Seconds)**: `0` (persistent)

### Step 2: Create Widget Scaling Actions

After showing the scene, add actions to resize individual widgets:

#### For Each Major Widget:
1. **Scene → Set Widget Size**
   - **Scene Name**: `PipsClipScene`
   - **Element**: `[Widget ID]`
   - **Width**: `round([Original Width] * %SCALE)`
   - **Height**: `round([Original Height] * %SCALE)`

2. **Scene → Set Widget Property**
   - **Scene Name**: `PipsClipScene`
   - **Element**: `[Widget ID]`
   - **Property**: Various (text size, margins, etc.)
   - **Value**: `round([Original Value] * %SCALE)`

### Step 3: Test the Implementation

#### Testing on Different Screen Sizes:
1. **Portrait Mode**: Run the task and verify proper scaling
2. **Landscape Mode**: Test rotation behavior
3. **Different Devices**: Test on tablets and phones with varying DPI
4. **Edge Cases**: Test on very small or very large screens

#### Debugging Tips:
- Use **Flash** actions to display variable values during testing
- Add **Say** actions to announce calculated dimensions
- Enable Tasker's run log to track variable changes
- Test with **Variable Query** to inspect values

### Step 4: Handle Screen Rotation

Add a profile to detect orientation changes:

1. **Create Profile**: Event → Display → Display Rotation
2. **Link Task**: Your "Clipboard Scene Init" task
3. **Test**: Rotate device and verify scene adapts

### Step 5: Optimize for Performance

#### Best Practices:
- **Cache calculations**: Store frequently used values
- **Minimize widget updates**: Only resize when necessary
- **Use efficient expressions**: Prefer integer math when possible
- **Clean up variables**: Clear temporary variables after use

#### Performance Considerations:
- Large scenes may take longer to resize
- Consider breaking complex scenes into smaller components
- Use scene templates for consistent layouts

## Advanced Configuration

### Custom Scaling Factors
Adjust the scaling calculation for different UI elements:

```
Text Size: %SCALE * 0.8    (slightly smaller scaling)
Buttons: %SCALE * 1.0      (full scaling)  
Icons: %SCALE * 0.9        (preserve detail)
```

### Multi-DPI Support
For devices with different DPI settings:

```
%DPIFACTOR = %DENSITYDPI / 160
%ADJUSTEDSCALE = %SCALE * (160 / %DENSITYDPI)
```

### Persistent Overlay Settings
To maintain the overlay across app switches:
- Set scene timeout to 0
- Enable "System Overlay" in scene properties
- Consider adding a small close button for user control

## Troubleshooting

### Common Issues:

1. **Scene doesn't appear**: Check overlay permissions
2. **Wrong scaling**: Verify %DESIGNW matches your original design
3. **Performance issues**: Reduce number of widget resize operations
4. **Rotation problems**: Ensure profile triggers on orientation change

### Variables to Monitor:
- `%SW`, `%SH`: Current screen dimensions
- `%SCALE`: Should be close to 1.0 for 1080p screens
- `%W`, `%H`: Final overlay dimensions
- `%X`, `%Y`: Positioning coordinates

## Example Values

For a 1080x1920 screen with 1080 design width:
- `%SW = 1080, %SH = 1920`
- `%SCALE = 1.0`
- `%W = 1080, %H = 480`
- `%X = 0, %Y = 360`

For a 720x1280 screen:
- `%SW = 720, %SH = 1280`  
- `%SCALE = 0.667`
- `%W = 720, %H = 320`
- `%X = 0, %Y = 240`

## Integration with Existing Pips Clip

To integrate with your existing Pips Clip setup:
1. Import the provided sample XML file
2. Modify your existing clipboard trigger profile to call "Clipboard Scene Init"
3. Update scene names to match your current scenes
4. Test thoroughly with your existing workflows

This responsive approach ensures your clipboard manager works seamlessly across all Android devices while maintaining the familiar interface and functionality.