---
title: Sketch relations
linktitle: Sketch relations
toc: true
type: docs
date: "2020-02-17"
draft: false
menu:
  SolidWorks:
    parent: Introduction to Sketching
    weight: 4

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

## Status of the sketch

Sketches can be in one of five definition states at any time. The status of a sketch depends on geometric relations between geometry and the dimensions that define it. The three most common states are:

* **Under defined:** There is inadequate definition of the sketch, but the sketch can still be used to create features. This is acceptable as most of the time during the initial stages of design, there is not enough information to fully define the sketch. Under defined geometry is {{<hl>}}blue{{</hl>}}.
* **Fully defined:** The sketch has complete information. Fully defined geometry is {{<hl>}}black{{</hl>}}. A general rule is that when a part is released for manufacturing, the sketches within it should be fully defined.
* **Over defined:** The sketch has duplicate dimensions or conflicting relations and it should not be used until repaired. Extra dimensions and relations should be deleted. Over defined geometry is {{<hl>}}red{{</hl>}}.

## Check under defined sketch geometry

### Check sketch status

A sketch is {{<hl>}}Under defined{{</hl>}} because some of the geometry is blue. Note that endpoints of a line can be a different color and different state than the line itself. 

For example, a vertical line from the origin is black because it is:
* Vertical
* Attached to the origin

However, the uppermost endpoint is blue because the length of the line is under defined.

### Dragging
Under defined geometry can be dragged to new locations or its size changed. Fully defined geometry cannot. You can determine how the geometry is under defined by dragging the blue parts of the geometry. The lines will either move or increase/decrease in length. This will give you an indication that you need to define the length of the line or need to define the location of the line.

<img src="/courses/SolidWorks/1-Sketch-relations_files/SolidWorks-design-intent2.PNG" alt="Design intent" width="80%"/>

## Design intent
The design intent governs how the part is built and how it will change. In a sketch, design intent is controlled by a combination of two things:
* **Sketch relations:** Create geometric relationships such as parallel, collinear, perpendicular, or coincident between sketch elements.
* **Dimensions:** Dimensions are used to define the size and location of the sketch geometry. Linear, radial, diameter and angular dimensions can be added.

To fully define a sketch *and* capture the desired design intent requires understanding and applying a combination of relations and dimensions.

## Sketch relations
Sketch relations are used to force a behaviour on a sketch element thereby capturing design intent. Some relations are added automatically when you create the initial sketch and others can be added later manually.

You can check the existing relations by either:

* Symbols appear next to the geometry to indicate what relations are associate with that entity. In the picture below, the arc has three relations: two tangent and one equal.
* Select the skech entity and the Property Manager show the relations associated with that entity.

> You can delete a relation by either clicking the relation icon and pressing the {{<hl>}}Delete{{</hl>}} button. You can also right click on the relation icon and selecting the delete option.

## Examples of sketch relations
There are many different sketch relations. Which ones are valid depends on the combination of geometry that you select.
The following are sketch relations:
* Coincident
* Merge
* Parallel
* Perpendicular
* Collinear
* Horizontal
* Vertical
* Equal
* Midpoint

