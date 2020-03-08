---
title: Creating a part in SolidWorks
linktitle: Create a part
toc: true
type: docs
date: "2020-02-16"
draft: false
menu:
  SolidWorks:
    parent: 1. Introduction to Sketching
    weight: 103

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

## Stages in the sketch process

Every sketch has several characteristics that contribute to its shape, size and orientation. You will always follow the same workflow when creating any part:

1. Create part.
2. Sketch geometry.
3. Add relations.
4. Extrude sketch.
5. Repeat...

## Create a part

When you first open up SolidWorks, a window will appear prompting you to create a new SolidWorks document. This can either be a part, assembly or drawing. If this window does not appear, you can create a new part by selecting: {{< hl >}}File, New{{< /hl >}} from the main menu.

![](/courses/SolidWorks/1-Creata-a-part_files/SolidWorks-New-file.PNG)

> Always remember to save your part files frequently. Try and save it as soon as you have created a new document.

## What is sketching in SolidWorks?
Sketching is the act of creating a 2D profile comprised of a wireframe geometry. Typical geometry include:
* Lines
* Arcs
* Circles
* Ellipses

## Creating a sketch
To create a sketch, you must choose a plane on which to sketch. The system provides three initial planes by default. They are:
* Front plane
* Top plane
* Right plane
> It is important to choose the correct plane to start your sketch on as this choice can influence how your drawing of the part is going to be layed out.

![](/courses/SolidWorks/1-Creata-a-part_files/SolidWorks-default-planes.PNG)

When creating a new sketch, {{<hl>}}Insert Sketch{{</hl>}} opens up the sketcher on the selected plane. If you have not selected a plane when you insert a sketch, SolidWorks will ask you to select the plane on which you want to sketch on.

## Active sketch environment
When you have selected a plane to sketch on, the selected plane will rotate so it is parallel to the screen. **This only happens for the first sketch of the part.** You will see two red arrows originating from the same point, one pointing upwards and the other points right. It represents the **sketch origin**.

## Confirmation corner
When any SolidWorks commands are active, a symbol or a set of symbols appears in the upper right corner of the graphics area. This is called the **confirmation corner.** When a sketch is opened, the confirmation corner displays two symbols. One looks like the sketch symbol and the other is a red X. These symbols provide a visual reminder that you are active in a sketch.
> Clicking the sketch symbol exists the sketch and *save any changes*. 

> Clicking the red X exits the sketch and discards any changes.

When other commands are active, the confirmation corner displays a green check mark and a red X. The check mark executes the current command and the X cancels the command.

## Sketch entities
SolidWorks offers a vaiety of sketch tools for creating profile geometry. The **line** tool will be used most of the time. Other sketch entities include:
* Circle
* Arcs
* Ellipse
* Partial ellipse
* Parabola
* Spline
* Arc slots
* Polygon
* Rectangle
* Point
* Centerline

## Basic sketching
There are two techniques to sketch a geometry:

**Click-Click**
Position the cursor where you want the line to start. Click the left mouse button. Move the cursor to where you want the ine to end. A preview of the sketch entity will follow the cursor. Cick the left mouse bitton a second  time. 

**Click-Drag**
Position the cursor where you want the line to start. Press and hold the left mouse button. Drag the cursor to where you want the sketch entity to end. A preview of the sketch entity will follow the the cursor. Release the left mouse button.

## Sketching a line
Sketch a line by selecting the line tool. Click on the origin and move your mouse horizontally to the right. A preview of the line will appear. Click again to end the line.

Do not be too concerned with making the line the exact length you want it to be at the start when you create the sketch. Your sketch only need to be the approximate size of your final sketch. The dimensioning tool will be used later to make the size exact.

![](/courses/SolidWorks/1-Creata-a-part_files/SolidWorks-vertical-line.PNG)

Next, starting at the end of the horizontal line, sketch a line at an angle.

![](/courses/SolidWorks/1-Creata-a-part_files/SolidWorks-angled-line.PNG)

### Inference lines
Dashed inference lines appear when drawing lines to help you 'line-up' with existing geometry. These lines include existing line vecotrs, normals, horizontals, veritcals, tangents and centers.

Note that some lines capture actual geometric relations, while other simply act as a guide or reference when sketching. 

A difference in the color of the inference lines will distinguish them. 

In the picture below, the line labeled "A" are olive-green and if the sketch line snaps to them, will capture either a tangent or perpendicuar relationship. 

The line labeled "B" is blue. It only provides a reference, in this case verical, to the other endpoint. If the sketch line is ended at this point, no vertical relation will be captured.

### Sketch feedback
The sketcher has many feedback features. The cursor will change to show what tipes of entity is being created. It will also indicate what selections on the existing geometry, such as end, coincident or midpoint, are available using a red dot when the cursor is on it.

The most three common feedback symbols are:
* **Endpoints:** Yellow concentric circles appear at the Endpoint when the cursor is over it.

* **Midpoint:** The midpoint appears as a square. It changes to red when the cursor is over the line.

* **Coincident:** The quadrant points of the circle appear with a concentric circle over the centerpoint.

### Closing the sketch
Close the sketch with a fina line connected to the starting point of the first line.

![](/courses/SolidWorks/1-Creata-a-part_files/SolidWorks-completed-sketch.PNG)

> When you want to turn off a sketching tool, you can either press the {{<hl>}}Esc{{</hl>}} key, click the line tool a second time, or right-click and the graphics area and choose {{<hl>}}Select{{</hl>}}.