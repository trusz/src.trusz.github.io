+++
title = "Monodraw Tip - Anchors"
date = 2020-06-03
tags = ["monodraw", "tips",]
categories = ["Tool"] 
imgs = []
cover = "/images/monodraw-tips-01-anchor.jpg"
coverAuthor ="Simon Abrams"
coverUrl ="https://unsplash.com/s/photos/ship-anchor?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText"
readingTime = true  # show reading time after article date
toc = true
comments = false
justify = false  # text-align: justify;
single = false  
license = ""
draft = false
+++

I use Monodraw to create my Diagrams. Its anchors feature is really helpful to make diagrams more flexible and easier to change.
There are a few use cases we can implement with them.

> [↗ Monodraw for macOS — Helftone](https://monodraw.helftone.com/)

> ###### Table of Contents
> 
> - [Anchors and Attachment Points](#anchors-and-attachment-points)
> - [Aligning Objects](#aligning-objects)
> - [Responsive Separators](#responsive-separators)
> - [Responsive Diagram Frame](#responsive-diagram-frame)
> - [UML Class Diagram](#uml-class-diagram)

## Anchors and Attachment Points

In Monodraw every object has one anchor point (left) and multiple attachment points (right).

<p align=center>
    <img src="/images/monodraw-tips-01/anchor.png" height=150 />
    <img src="/images/monodraw-tips-01/attachment_points.png" height=150 />
</p>

With the anchor you can anchor an object to another object's attachment point.
This makes the object "stick" to the other one.

![gif demonstrating monodraw's anchor feature](/images/monodraw-tips-01/monodraw-anchors-01.gif)

This enables us to create responsive components.
Let's see a few use cases

## Aligning Objects

We can align text with the text align option.


<img src="/images/monodraw-tips-01/text_align.png" width=200 />

However, there is no alignment option for objects.
But we have anchors! Set the object's anchor point and the parent's attachment point depending on where you want to align the object.
For example if you want to align the object on the top left side:
1. set the object's anchor to the top left  
    <img src="/images/monodraw-tips-01/setup_obj_top_left.png" height=140 />
2. turn on the parent's top left inner attachment point  
    <img src="/images/monodraw-tips-01/setup_top_left.png" height=140 />
3. drag the object's anchor point to the parent's attachment point.  
    ![gif demonstrating attaching object](/images/monodraw-tips-01/monodraw-anchors-02.gif)

Now you can resize or move the parent the object stays aligned.

![gif demonstrating resizing](/images/monodraw-tips-01/monodraw-anchors-03.gif)

Here are all the possible alignment positions configured:

![gif demonstrating all alignments](/images/monodraw-tips-01/monodraw-anchors-04.gif)


## Responsive Separators

Responsive separators are really useful when onw wants to isolate text within an object. We will se examples at the end.

So how to configure them? This will require a few tricks.
Let's say we want to separate the name of the box from the rest of it like this:

<p align=center>
    <img alt="responsive separator" src="/images/monodraw-tips-01/responsive-separator.png" width=250 />
</p>


1. Create a rect and turn off text and fill.
2. Turn on the top left inner and top right inner attachment points and turn off every other.  
   <img src="/images/monodraw-tips-01/sep_config_01.png" height=400 />
3. Create two 3x3 boxes. We will call them ports.
4. Configure one port to have a bottom-left inner attachment point and a top-left anchor.  
   <img src="/images/monodraw-tips-01/sep_port_left.png" width=200 />
5. Configure the other port to have bottom-right attachment point and a top-right anchor.  
   <img src="/images/monodraw-tips-01/sep_port_right.png" width=200 />
6. Attach the ports to the parent.  
   ![gif demonstrating attaching port to parent](/images/monodraw-tips-01/monodraw-anchors-05.gif)
7. Draw a line without arrows from the left port to the right port.  
   ![gif demonstrating drawing line between ports](/images/monodraw-tips-01/monodraw-anchors-06.gif)
8. Remove fill and border from ports.  
   ![gif demonstrating removing fill and border](/images/monodraw-tips-01/monodraw-anchors-07.gif)
9. Add a borderless text and align in the top center.  
   ![gif demonstrating adding text](/images/monodraw-tips-01/monodraw-anchors-08.gif)
10. Configure a new box to have:
    - text align: top left
    - no fill
    - no border 
    - anchor offset x=1 y=1
11. Attach the text to the left port.  
   ![gif demonstrating adding body text](/images/monodraw-tips-01/monodraw-anchors-09.gif)

Now you have a responsive text box with title and body:

![gif demonstrating a responsive text box with title and body](/images/monodraw-tips-01/monodraw-anchors-10.gif)


## Responsive Diagram Frame

I usually frame the diagrams to provide a few metadata about the project security level and company. With the object alignment and responsive separator I could create a nice resizable frame.

![gif demonstrating a responsive frame](/images/monodraw-tips-01/monodraw-anchors-resp-frame.gif)


## UML Class Diagram

Sometimes I still need an UML Class diagram. This is can be also nicely done. Here I use a responsive separator for the class name on the top and one in the middle to separate the class members and functions.

![gif demonstrating an UML Class Diagram](/images/monodraw-tips-01/monodraw-anchors-uml-class.gif)