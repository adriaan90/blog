---
title: Dimensions
linktitle: Sketch dimensions
toc: true
type: docs
date: "2020-02-20"
draft: false
menu:
  SolidWorks:
    parent: 1. Introduction to Sketching
    weight: 105

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5
---

## Smart dimensions
The *Smart Dimension* tool determines the proper type of dimensions based on the geometry chosen. For example, if you pick an arc the system will create a radial dimension. If you pick a circle, you will get a diameter dimension, while selecting two parallel lines will create a linear dimension between them. In cases where the *Smart Dimension* tool is not quite smart enough, you have the option of selecting endpoints and moving the dimensions to different measurement positions.

## Where to find it
The *Smart Dimension* tool can be selected either by:
* Selecting *Smart Dimension* tool from the *Tools* option
* Right-click on the canvas and select *Smart Dimension* from the shortcut menu
* On the Dimensions/Relations toolbar, pick *Smart Dimension* tool

## Creating and previewing a dimension
As you select a line in the sketch geometry with the dimension tool, the system creates a preview of the dimension. The preview enables you to ensure that you have captured the correct dimension as well as enable you to see all the different options there are.

> Clicking the right mouse button after you have selected the geometry with the dimension tool, locks the orientation of the dimension preview.

You can place the dimension anywhere on the canvas by clicking the left mouse button. Once you have placed the dimension, a *Modify* tool will appear, displaying the current value of the line you selected. You can enter a new value for the line in the box provided. There is a thumbwheel that will also incrementally increase/decrease the value in the *Modify* toolbox.

>You can change the value of the dimension later as well, by just double-clicking on the dimension which will result in the *Modify* tool window appearing.

## Angular dimensions
Angular dimensions can be created using the same dimension tool used for linear, radial and diameter dimensions. Select either two lines that are both non-collinear and non-parallel, or select three non-collinear endpoints.
Depending on where you place the angular dimension, you can get the interior or exterior angle, the acute angle, or the oblique angle.

![](/courses/SolidWorks/1-Dimensions_files/SolidWorks-fully-defined-sketch.PNG)
