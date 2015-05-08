---
title: Constructive Solid Geometry
layout: blogpost
posted: 04-26-15
updated: 05-08-15
description: Making a heart from simple geometric boolean operations.
---

{% highlight c# %}
using System.Drawing;

// Think of a Region as a shape, and this code should make sense.
private Region GetHeart(Size size)
{
    GraphicsPath gp = new GraphicsPath();
    Rectangle ellipse = new Rectangle(-size.Width / 4, -size.Height / 2, size.Width / 2, size.Height);
    gp.AddEllipse(ellipse);
    Matrix rotation = new Matrix();

    // These are two ellipses, rotated in opposite directions to make 2 overlapping hearts.
    Region regionLeft = new Region(gp);
    rotation.Rotate(-45f);
    regionLeft.Transform(rotation);
    Region regionRight = new Region(gp);
    rotation.Rotate(90f);
    regionRight.Transform(rotation);

    // This is the middle of the heart, which is where the two ellipses overlap.
    Region regionMiddle = regionLeft.Clone();
    regionMiddle.Intersect(regionRight);

    // This is the lower half of the "double heart", so this needs to be cut off
    Region regionExcluded = new Region(new Rectangle(2 * ellipse.Left, 0, 2 * ellipse.Width, ellipse.Height / 2));
    // Combining the two "lobes" and cutting off the bottom half
    Region regionTop = regionLeft.Clone();
    regionTop.Xor(regionRight);
    regionTop.Exclude(regionExcluded);

    // Combining the two top "lobes" with the middle of the heart for the total heart.
    Region regionTotal = regionMiddle.Clone();
    regionTotal.Union(regionTop);

    // Just moving the heart so it's drawn in a reasonable spot, not centered in the top left corner.
    regionTotal.Translate(ellipse.Width, ellipse.Height / 2);

    return regionTotal;
}
{% endhighlight %}
