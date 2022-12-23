"""-----------------------------------------------------------------------------
  Script Name: Clip Multiple Feature Classes
  Description: Clips one or more shapefiles
               from a folder and places the clipped
               feature classes into a geodatabase.
  Created By:  Insert name here.
  Date:        Insert date here.
-----------------------------------------------------------------------------"""

# Import ArcPy site-package and os modules
import arcpy 
import os

# Set the input workspace
arcpy.env.workspace = arcpy.GetParameterAsText(0)

# Set the clip featureclass
clipFeatures = arcpy.GetParameterAsText(1)

# Set the output workspace
outWorkspace = arcpy.GetParameterAsText(2)

# Set the XY tolerance
clusterTolerance = arcpy.GetParameterAsText(3)

try:
    # Get a list of the featureclasses in the input folder
    fcs = arcpy.ListFeatureClasses()

    for fc in fcs:   
        # Validate the new feature class name for the output workspace.
        featureClassName = arcpy.ValidateTableName(fc, outWorkspace)
        outFeatureClass = os.path.join(outWorkspace, featureClassName)
        
        # Clip each feature class in the list with the clip feature class.
        # Do not clip the clipFeatures, it may be in the same workspace.
        if fc != os.path.basename(clipFeatures):
            arcpy.Clip_analysis(fc, clipFeatures, outFeatureClass, 
                                clusterTolerance)

except Exception as err:
    arcpy.AddError(err)
    print err
