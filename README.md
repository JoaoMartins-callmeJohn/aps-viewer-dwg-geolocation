# APS Viewer DWG Geolocation

A sample application demonstrating how to extract and visualize geolocation data from georeferenced DWG files using the Autodesk Platform Services (APS) Viewer and its Geolocation extension.

## Overview

This project showcases how to:
- Load georeferenced DWG files in the APS Viewer
- Use the Geolocation extension to access coordinate system information
- Convert viewer coordinates to real-world latitude/longitude coordinates
- Display geographic coordinates when clicking on the model

## Sample Design

The repository includes a sample georeferenced DWG file:
- **`geolocated-esri.dwg`** - A Civil 3D drawing with proper georeferencing applied

This sample file demonstrates a properly georeferenced design that can be used to test the geolocation functionality.

## Important: Proper Georeferencing Requirements

### Use 3D Views
**Critical**: The design must be saved with a **3D view** active for geolocation data to be properly extracted. 2D views do not contain the necessary transformation data for the Geolocation extension to function correctly.

## The Geolocation Extension

The `Autodesk.Geolocation` extension is the key component that enables geographic coordinate extraction:

### Features:
- **Automatic Detection**: Detects if the loaded model contains geolocation data
- **Coordinate Transformation**: Converts model space coordinates to WGS84 (longitude/latitude)
- **Elevation Support**: Maintains Z-coordinate as elevation above sea level
- **Multiple Coordinate Systems**: Supports various coordinate systems embedded in DWG files

### Key Methods:
```javascript
// Check if model has geolocation data
geolocationExtension.hasGeolocationData()

// Convert model coordinates to lon/lat
let lonlat = geolocationExtension.lmvToLonLat(modelPoint)
// Returns: { x: longitude, y: latitude, z: elevation }

// Convert lon/lat back to model coordinates
let modelPoint = geolocationExtension.lonLatToLmv(longitude, latitude, elevation)
```

### How It Works
1. The viewer loads with the Geolocation extension enabled
2. When you click on the model, it:
   - Captures the click position
   - Converts screen coordinates to model coordinates
   - Uses the Geolocation extension to transform to longitude/latitude
   - Displays the geographic coordinates in a popup

## Troubleshooting

### "No geolocation data found" Error
This error appears when:
- The model was not properly georeferenced
- The model was saved in a 2D view instead of 3D
- The coordinate system was not properly assigned
- The model derivative failed to extract geolocation metadata

### Solutions:
1. Re-save your DWG in a 3D view
2. Verify coordinate system assignment
3. Re-upload to APS and wait for processing
4. Check the manifest to ensure geolocation metadata exists

## Technical Notes

- The extension requires the model to be processed with APS Model Derivative API v2
- Coordinate transformation accuracy depends on the original georeferencing quality
- The extension works with various coordinate systems (State Plane, UTM, etc.)
- WGS84 is used as the output coordinate system for longitude/latitude

## License

This sample is licensed under the MIT License. See [LICENSE](LICENSE) for details.
