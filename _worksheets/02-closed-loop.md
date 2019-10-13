---
layout: worksheet
title: Closed-Loop Systems
permalink: /worksheets/closed-loop/
---

In a closed-loop system, the results of data processing feedback into the external world, establishing a relationship where the output of the system depends on the sensory input. Many behavioural experiments in neuroscience require some kind of closed-loop interaction between the subject and the experimental setup. The exercises below will show you how to use the online data processing capabilities of Bonsai to create many different kinds of closed-loop systems.

### **Exercise 1:** Triggering a digital line based on region of interest activity

![Triggering a digital line on ROI activity]({{ site.baseurl }}/assets/images/closed-loop-roi.svg)

* Insert a `CameraCapture` source.
* Insert a `Crop` transform.
* Run the workflow and use the `RegionOfInterest` property to specify the desired area.

**Hint:** You can use the visual editor for an easier calibration. While the workflow is running, right-click on the `Crop` transform and select `Show Default Editor` from the context menu or click in the small button with ellipsis that appears when you select the `RegionOfInterest` property.
{: .notice--info}

* Insert a `Grayscale` and a `Threshold (Vision)` transform (or your preferred segmentation pipeline).
* Insert a `Sum (Dsp)` transform, and select the `Val0` field from the output.
* Insert a `GreaterThan` transform and configure the `Value` property to an appropriate threshold. Remember you can use the visualizers to see what values are coming through the `Sum` and what the result of the `GreaterThan` operator is.
* Insert the Arduino `DigitalOutput` sink.
* Set the `Pin` property of the `DigitalOutput` operator to 13.
* Configure the `PortName` property.
* Run the workflow and verify that entering the region of interest triggers the Arduino LED.
* **Optional:** Measure the latency of this closed-loop system.
* **Optional:** Replace the `Crop` transform by a `CropPolygon` to allow for non-rectangular regions.

**Note:** The `CropPolygon` operator uses the `Regions` property to define multiple, possibly non-rectangular regions. The visual editor is similar to `Crop`, where you draw a rectangular box. However, in `CropPolygon` you can move the corners of the box by right-clicking *inside* the box and dragging the cursor to the new position. You can add new points by double-clicking with the left mouse button, and delete points by double-clicking with the right mouse button. You can delete regions by pressing the `Del` key and cycle through selected regions by pressing the `Tab` key.
{: .notice--info}

### **Exercise 2:** Modulating stimulus intensity based on distance to a point

![Playing a dynamic sound]({{ site.baseurl }}/assets/images/closed-loop-generator.svg)

* Insert a `FunctionGenerator` source.
* Set the `Amplitude` property to 500, and the `Frequency` property to `200`.
* Insert an `AudioPlayback` sink.
* Externalize the `Amplitude` property of the `FunctionGenerator` using the right-click context menu.

If you run the workflow, you should hear a pure tone coming through the speakers. The `FunctionGenerator` periodically emits buffered waveforms with values ranging between 0 and `Amplitude`, the shape of which changes the properties of the tone. For example, by changing the value of `Amplitude` you can make the sound loud or soft. The next step is to modulate the `Amplitude` property dynamically based on the distance of the object to a target.

![Modulating stimulus intensity based on distance to a point]({{ site.baseurl }}/assets/images/closed-loop-modulate.svg)

* Create a video tracking workflow using `ConvertColor`, `HsvThreshold`, and the `Centroid` operator to directly compute the centre of mass of a colored object.
* Insert a `Subtract` transform and configure the `Value` property to be some target coordinate in the image.

The result of the `Subtract` operator will be a vector pointing from the target to the centroid of the largest object. The desired distance from the centroid to the target would be the length of that vector.

* Insert an `ExpressionTransform` operator. This node allows you to write small mathematical and logical expressions to transform input values.
* Right-click on the `ExpressionTransform` operator and select `Show Default Editor`. Set the expression to `Math.Sqrt(X*X + Y*Y)`.

**Note:** Inside the `Expression` editor you can access any field of the input by name. In this case `X` and `Y` represent the corresponding fields of the `Point2f` data type. You can check which fields are available by right-clicking the previous node. You can use all the normal arithmetical and logical operators as well as the mathematical functions available in the [`Math`](https://msdn.microsoft.com/en-us/library/system.math(v=vs.110).aspx) type. The default expression `it` means "input" and represents the input value itself.
{: .notice--info}

* Connect the `ExpressionTransform` operator to the externalized `Amplitude` property.
* Run the workflow and verify that stimulus intensity is modulated by the distance of the object to the target point.
* **Optional:** Modulate the `Frequency` property instead of `Amplitude`.
* **Optional:** Use the `Rescale` operator to adjust the gain of the modulation by configuring the `Min`, `Max`, `RangeMax` and `RangeMin` properties. Set the `RescaleType` property to `Clamp` to restrict the output values to an allowed range.

**Note:** You can specify inverse relationships using `Rescale` if you set the *maximum* input value to the `Min` property, and the *minimum* input value to the `Max` property. In this case, a small distance will generate a large output, and a large distance will produce a small output.
{: .notice--info}

### **Exercise 3:** Triggering a digital line based on distance between objects

![Triggering a digital line based on distance between objects]({{ site.baseurl }}/assets/images/closed-loop-trigger.svg)

* Create an object tracking workflow using `FindContours` and `BinaryRegionAnalysis`.
* Insert a `SortBinaryRegions` transform. This operator will sort the list of objects by area, in order of largest to smallest.

To calculate the distance between the two largest objects in every frame you will need to take into account some special cases. Specifically, there is the possibility that no object is detected, or that the two objects may be touching each other and will be detected as a single object. You can develop a new operator in order to perform this specific calculation.

* Insert a `PythonTransform` operator. Change the `Script` property to the following code:

```python
from math import sqrt

@returns(float)
def process(value):

  # no objects were detected
  if value.Count == 0:
    return float.NaN

  # only one object was detected, assume objects are touching
  elif value.Count == 1:
    return 0

  # two or more objects were detected, compute distance
  else:
    # d: displacement between two largest objects
    d = value[0].Centroid - value[1].Centroid
    return sqrt(d.X * d.X + d.Y * d.Y)
```

* Insert a `LessThan` transform and configure the `Value` property to an appropriate threshold.
* Connect the boolean output to Arduino pin 13 using a `DigitalOutput` sink.
* Run the workflow and verify that the Arduino LED is triggered when the two objects are close together.

### **Exercise 4:** Centring the video on a tracked object

![Shifting the video using warp affine]({{ site.baseurl }}/assets/images/closed-loop-warpaffine.svg)

* Insert a `CameraCapture` source.
* Insert a `WarpAffine` transform. This node applies affine transformations on the input defined by the `Transform` matrix.
* Externalize the `Transform` property of the `WarpAffine` operator using the right-click context menu.
* Create an `AffineTransform` source and connect it to the externalized property.
* Run the workflow and change the values of the `Translation` property while visualizing the output of `WarpAffine`. Notice that the transformation induces a translation in the input image controlled by the values in the property.

![Centring the video on a tracked object]({{ site.baseurl }}/assets/images/closed-loop-stabilization.svg)

* In a new branch, create a video tracking workflow using `ConvertColor`, `HsvThreshold`, and the `Centroid` operator to directly compute the centre of mass of a colored object.
* Insert a `Negate` transform. This will make the X and Y coordinates of the centroid negative.

We now want to map our negative centroid to the `Translation` property of `AffineTransform`, so that we dynamically translate each frame using the negative position of the object. You can do this by using property mapping operators, which are described in more detail at [http://bonsai-rx.org/docs/property-mapping](http://bonsai-rx.org/docs/property-mapping).

* Insert an `InputMapping` operator.
* Connect the `InputMapping` to the `AffineTransform` operator.
* Open the `PropertyMappings` editor and add a new mapping to the `Translation` property.
* Run the workflow. Verify that the object is always placed at position (0,0). What is the problem?

**Note:** Generally for image coordinates, (0,0) is at the top-left corner, and the centre will be at coordinates (width/2, height/2), usually (320,240) for images with 640 x 480 resolution.
{: .notice--info}

* Insert an `Add` transform. This will add a fixed offset to the point. Configure the `Value` property with an offset that will place the object at the image centre, e.g. (320,240).
* Run the workflow, and verify that the output of `WarpAffine` is now a video which is always centred on the tracked object.
* **Optional**: Insert a `Crop` transform after `WarpAffine` to select a bounded region around the object.
* **Optional**: Modify the object tracking workflow to use `FindContours` and `BinaryRegionAnalysis`.