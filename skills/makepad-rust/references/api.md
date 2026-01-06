# Makepad-Rust - Api

**Pages:** 90

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/log

**Contents:**
- #log
- #Declaration
- #Parameters
- #Description
- #See Also

return the natural logarithm of the parameter

log returns the natural logarithm of x. i.e., the value 𝑦 which satisfies $$ x = e^{y} $$ Results are undefined if x ≤ 0.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/sign

**Contents:**
- #sign
- #Declaration
- #Parameters
- #Description
- #See Also

extract the sign of the parameter

sign returns -1.0 if 𝑥<0.0, 0.0 if 𝑥=0.0 and 1.0 if 𝑥>0.0.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/pow

**Contents:**
- #pow
- #Declaration
- #Parameters
- #Description
- #See Also

return the value of the first parameter raised to the power of the second

pow returns the value of x raised to the y power. i.e., $$ x^{y} $$ Results are undefined if x < 0 or if x == 0 and y ≤ 0.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/fill

**Contents:**
- #fill
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The fill function fills the current shape in the Sdf2d drawing context with an RGBA color. It converts the provided color to pre-multiplied alpha format before blending it with the existing content. This function also resets the internal shape and clipping parameters, preparing the context for subsequent drawing operations.

**Examples:**

Example 1 (php):
```php
1fn fill(inout self, color: vec4) -> vec4
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4    
5    // Draw a rectangle with rounded corners.
6    sdf.box(10.0, 10.0, 100.0, 100.0, 5.0);
7    
8    // Apply a solid red fill to the rectangle.
9    sdf.fill(#f00);
10    
11    // Rotate the drawing by 90 degrees around the center of the rectangle.
12    sdf.rotate(PI * 0.5, 60.0, 60.0);
13    
14    // Return the final color result.
15    return sdf.result;
16}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/primitives/rect

**Contents:**
- #rect
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The rect function draws a rectangle within the Sdf2d drawing context. The rectangle is defined by its position and dimensions, with the origin at the lower-left corner.

**Examples:**

Example 1 (unknown):
```unknown
1fn rect(inout self, x: float, y: float, w: float, h: float)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a rectangle at position (10.0, 10.0) with width 80.0 and height 60.0.
6    sdf.rect(10.0, 10.0, 80.0, 60.0);
7
8    // Fill the rectangle with a solid red color.
9    sdf.fill(#f00);
10
11    // Rotate the drawing by 45 degrees (PI * 0.25 radians) around the center of the rectangle.
12    sdf.rotate(PI * 0.25, 50.0, 40.0);
13
14    // Return the final color result.
15    return sdf.result;
16}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/max

**Contents:**
- #max
- #Declaration
- #Parameters
- #Description
- #See Also

return the greater of two values

max returns the maximum of the two parameters. It returns y if y is greater than x, otherwise it returns x.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/asin

**Contents:**
- #asin
- #Declaration
- #Parameters
- #Description
- #See Also

return the arcsine of the parameter

atan returns the angle whose trigonometric sine is 𝑥. The range of values returned by asin is $$ \left[ -\frac{\pi}{2}, \frac{\pi}{2} \right] $$ The result is undefined if |𝑥|>1.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/fill_keep

**Contents:**
- #fill_keep
- #Parameters
- #Returns
- #Example
  - #Explanation

The fill_keep function blends an RGBA color with the current result color of the Sdf2d drawing context, converting the color to pre-multiplied alpha format before blending. It preserves the existing shapes and clipping settings, allowing you to apply additional fills without resetting the drawing state.

**Examples:**

Example 1 (php):
```php
1fn fill_keep(inout self, color: vec4) -> vec4
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a rectangle with rounded corners.
6    sdf.box(10.0, 10.0, 80.0, 80.0, 5.0);
7
8    // Apply a semi-transparent red fill, converting to pre-multiplied alpha.
9    sdf.fill_keep(vec4(1.0, 0.0, 0.0, 0.5));
10
11    // Draw another shape without resetting the state.
12    // For example, draw a circle inside the rectangle.
13    sdf.circle(50.0, 50.0, 30.0);
14
15    // Apply a semi-transparent green fill, blending with the previous fills.
16    sdf.fill_keep(vec4(0.0, 1.0, 0.0, 0.5));
17
18    // Return the final color result.
19    return sdf.result;
20}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/line_to

**Contents:**
- #line_to
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The line_to function draws a line segment from the current position (self.last_pos) to a specified point (x, y). This updates the path in the Sdf2d drawing context, allowing you to create complex shapes by connecting multiple points.

Create Drawing Context: Initialize the Sdf2d context using the current position (self.pos) and size (self.rect_size) of the viewport.

Start Path: Set the starting point of the path at (30.0, 30.0) using sdf.move_to.

Draw Lines: Use sdf.line_to to draw lines connecting to various points, constructing the outline of a star shape.

Close Path: Use sdf.close_path to draw a line back to the starting point, ensuring that the shape is closed.

Apply Fill: Fill the closed path with red color using sdf.fill(#f00).

Return Result: Return sdf.result, which contains the final rendered color after all drawing operations.

Path Construction: The combination of move_to, line_to, and close_path allows you to create complex vector shapes by defining their outlines point by point.

State Management: Each call to line_to updates the current position (self.last_pos). The close_path function uses this position to connect back to the starting point (self.start_pos).

Drawing Order: Ensure that you define your entire path and close it before applying fills or strokes to render the shape correctly.

Transformations: You can apply transformations like translate, rotate, or scale to the Sdf2d context to manipulate the shape as needed.

**Examples:**

Example 1 (unknown):
```unknown
1fn line_to(inout self, x: float, y: float)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Start a new path at point (30.0, 30.0).
6    sdf.move_to(30.0, 30.0);
7
8    // Draw lines to create a star shape.
9    sdf.line_to(50.0, 70.0);   // Line to point (50.0, 70.0).
10    sdf.line_to(70.0, 30.0);   // Line to point (70.0, 30.0).
11    sdf.line_to(10.0, 50.0);   // Line to point (10.0, 50.0).
12    sdf.line_to(90.0, 50.0);   // Line to point (90.0, 50.0).
13
14    // Close the path to complete the shape.
15    sdf.close_path();
16
17    // Fill the shape with a solid red color.
18    sdf.fill(#f00);
19
20    // Return the final color result.
21    return sdf.result;
22}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/sample2d

**Contents:**
- #2D-samples a render target texture. [???]
- #Declaration
- #Parameters

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/fields/last_pos (vec2)

**Contents:**
- #last_pos

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/min

**Contents:**
- #min
- #Declaration
- #Parameters
- #Description
- #See Also

return the lesser of two values

min returns the minimum of the two parameters. It returns y if y is less than x, otherwise it returns x.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/equal

**Contents:**
- #equal
- #Declaration
- #Parameters
- #Description
- #See Also

perform a component-wise equal-to comparison of two vectors

equal returns a boolean vector in which each element i is computed as x[i] == y[i].

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/refract

**Contents:**
- #refract
- #Declaration
- #Parameters
- #Description
- #See Also

calculate the refraction direction for an incident vector

For a given incident vector I, surface normal N and ratio of indices of refraction, eta, refract returns the refraction vector, R. R is calculated as:

The input parameters I and N should be normalized in order to achieve the desired result.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/inversesqrt

**Contents:**
- #inversesqrt
- #Declaration
- #Parameters
- #Description
- #See Also

return the inverse of the square root of the parameter

inversesqrt returns the inverse of the square root of x. i.e., the value $$ \frac{1}{\sqrt{x}} $$ Results are undefined if 𝑥≤0.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/exp

**Contents:**
- #exp
- #Declaration
- #Parameters
- #Description
- #See Also

return the natural exponentiation of the parameter

exp returns the natural exponentiation of x. i.e., $$ e^{x} $$

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/exp2

**Contents:**
- #exp2
- #Declaration
- #Parameters
- #Description
- #See Also

return 2 raised to the power of the parameter

exp2 returns 2 raised to the power of x. i.e., $$ 2^{x} $$

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/cross

**Contents:**
- #cross
- #Declaration
- #Parameters
- #Description
- #See Also

calculate the cross product of two vectors

cross returns the cross product of two vectors, x and y. i.e., $$ \begin{pmatrix} x[1] \cdot y[2] - y[1] \cdot x[2] \ x[2] \cdot y[0] - y[2] \cdot x[0] \ x[0] \cdot y[1] - y[0] \cdot x[1] \end{pmatrix} $$

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/fields/old_shape (float)

**Contents:**
- #old_shape

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/lessThanEqual

**Contents:**
- #lessThanEqual
- #Declaration
- #Parameters
- #Description
- #See Also

perform a component-wise less-than-or-equal comparison of two vectors

lessThanEqual returns a boolean vector in which each element i is computed as x[i] ≤ y[i].

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/step

**Contents:**
- #step
- #Declaration
- #Parameters
- #Description
- #See Also

generate a step function by comparing two values

step generates a step function by comparing x to edge. For element i of the return value, 0.0 is returned if x[i] < edge[i], and 1.0 is returned otherwise.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/bool/intersect

**Contents:**
- #intersect
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The intersect function modifies the current shape in the Sdf2d drawing context by intersecting it with the previously stored shape. The result is a new shape that represents the area where both shapes overlap. This operation is useful when you want to create complex shapes by combining simpler ones based on their overlapping regions.

Create Drawing Context: We initialize an Sdf2d drawing context scaled to the size of the current rectangle (self.rect_size).

First Shape: We draw a circle centered at (40.0, 50.0) with a radius of 30.0.

Store Shape: By calling sdf.union(), we save the current shape into self.old_shape. This prepares it for the intersection operation.

Second Shape: We draw another circle centered at (60.0, 50.0) with the same radius.

Apply Intersection: Using sdf.intersect(), we modify the shape field to represent the intersection between the current shape (second circle) and the stored shape (first circle). The result is the overlapping area of the two circles.

Fill Shape: We fill the intersected area with red color using sdf.fill(#f00).

Return Result: The function returns sdf.result, which contains the final rendered image showing the intersected shape.

Shape Composition: The intersect function is useful when you need to create shapes based on the common area of multiple shapes, allowing for more complex designs.

Order of Operations: Ensure that you store the initial shape using sdf.union() before drawing the second shape and applying sdf.intersect().

Transformation: You can apply transformations like translate, rotate, or scale to manipulate the shapes before intersecting them.

Combining Shapes: The intersect function is part of a set of Boolean operations that allows you to combine shapes in various ways. Other related functions include sdf.union() and sdf.subtract().

Further Effects: After intersecting shapes, you can apply effects like sdf.glow() or sdf.stroke() to enhance the visual appearance.

**Examples:**

Example 1 (unknown):
```unknown
1fn intersect(inout self)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw the first circle at position (40.0, 50.0) with a radius of 30.0.
6    sdf.circle(40.0, 50.0, 30.0);
7
8    // Store the current shape for intersection.
9    sdf.union(); // Store the first shape.
10
11    // Draw the second circle at position (60.0, 50.0) with a radius of 30.0.
12    sdf.circle(60.0, 50.0, 30.0);
13
14    // Perform intersection between the current shape and the stored shape.
15    sdf.intersect();
16
17    // Fill the intersected area with a solid red color.
18    sdf.fill(#f00);
19
20    // Return the final color result.
21    return sdf.result;
22}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/tan

**Contents:**
- #tan
- #Declaration
- #Parameters
- #Description
- #See Also

return the tangent of the parameter

tan returns the trigonometric tangent of angle.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/primitives/hline

**Contents:**
- #hline
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The hline function draws a horizontal line within the Sdf2d drawing context at a specified y-coordinate and with a specified half-thickness. This function is useful for adding horizontal lines or dividers in your graphics or UI elements.

**Examples:**

Example 1 (unknown):
```unknown
1fn hline(inout self, y: float, h: float)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a horizontal line at y = 50.0 with half-thickness 2.0.
6    sdf.hline(50.0, 2.0);
7
8    // Fill the horizontal line with a solid red color.
9    sdf.fill(#f00);
10
11    // Rotate the drawing by 90 degrees (PI * 0.5 radians) around point (50.0, 50.0).
12    sdf.rotate(PI * 0.5, 50.0, 50.0);
13
14    // Return the final color result.
15    return sdf.result;
16}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/greaterThan

**Contents:**
- #greaterThan
- #Declaration
- #Parameters
- #Description
- #See Also

perform a component-wise greater-than comparison of two vectors

greaterThan returns a boolean vector in which each element i is computed as x[i] > y[i].

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/faceforward

**Contents:**
- #faceforward
- #Declaration
- #Parameters
- #Description
- #See Also

return a vector pointing in the same direction as another.

faceforward orients a vector to point away from a surface as defined by its normal. If [[dot]](Nref, I) < 0 faceforward returns N, otherwise it returns -N.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/primitives/circle

**Contents:**
- #circle
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The circle function draws a circle within the Sdf2d drawing context. The circle is defined by its center coordinates and radius.

**Examples:**

Example 1 (unknown):
```unknown
1fn circle(inout self, x: float, y: float, r: float)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a circle centered at (50.0, 50.0) with a radius of 40.0.
6    sdf.circle(50.0, 50.0, 40.0);
7
8    // Fill the circle with a solid red color.
9    sdf.fill(#f00);
10
11    // Rotate the drawing by 45 degrees around the center of the circle.
12    sdf.rotate(PI * 0.25, 50.0, 50.0);
13
14    // Return the final color result.
15    return sdf.result;
16}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/primitives/box_all

**Contents:**
- #box_all
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The box_all function draws a rectangle within the Sdf2d drawing context, allowing each corner to have an individual radius. This enables the creation of rectangles with asymmetrical rounded corners.

**Examples:**

Example 1 (unknown):
```unknown
1fn box_all(
2    inout self,
3    x: float,
4    y: float,
5    w: float,
6    h: float,
7    r_left_top: float,
8    r_right_top: float,
9    r_right_bottom: float,
10    r_left_bottom: float
11)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a rectangle at position (10.0, 10.0) with width 80.0 and height 80.0.
6    // Each corner has a different radius.
7    sdf.box_all(
8        10.0, // x position
9        10.0, // y position
10        80.0, // width
11        80.0, // height
12        0.0,  // top-left corner radius (sharp corner)
13        10.0, // top-right corner radius
14        20.0, // bottom-right corner radius
15        5.0   // bottom-left corner radius
16    );
17
18    // Fill the shape with a solid red color.
19    sdf.fill(#f00);
20
21    // Return the final color result.
22    return sdf.result;
23}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/fields/start_pos (vec2)

**Contents:**
- #start_pos

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/stroke_keep

**Contents:**
- #stroke_keep
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The stroke_keep function applies a stroke to the current shape within the Sdf2d drawing context, blending a specified color along the edge of the shape based on the stroke width. This function preserves the existing shape and clipping settings, allowing you to add strokes without resetting the drawing state.

**Examples:**

Example 1 (php):
```php
1fn stroke_keep(inout self, color: vec4, width: float) -> vec4
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a rectangle with rounded corners.
6    sdf.box(10.0, 10.0, 80.0, 80.0, 5.0);
7
8    // Apply a stroke with a width of 2.5 units and red color.
9    sdf.stroke_keep(#f00, 2.5);
10
11    // Optionally, fill the shape with another color without resetting the state.
12    // sdf.fill_keep(#0f0);
13
14    // Return the final color result.
15    return sdf.result;
16}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/inverse

**Contents:**
- #inverse
- #Declaration
- #Parameters
- #Description
- #See Also

calculate the inverse of a matrix

inverse returns the inverse of the matrix m. The values in the returned matrix are undefined if m is singular or poorly-conditioned (nearly singular).

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/primitives/box_y

**Contents:**
- #box_y
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The box_y function draws a rectangle within the Sdf2d drawing context, allowing you to specify different corner radii for the top and bottom sides. This enables the creation of rectangles with asymmetric rounded corners along the vertical axis.

**Examples:**

Example 1 (unknown):
```unknown
1fn box_y(inout self, x: float, y: float, w: float, h: float, r_top: float, r_bottom: float)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a rectangle at position (10.0, 10.0) with width 80.0 and height 100.0.
6    // Top corners have a radius of 15.0, bottom corners have a radius of 5.0.
7    sdf.box_y(
8        10.0,   // x position
9        10.0,   // y position
10        80.0,   // width
11        100.0,  // height
12        15.0,   // radius of top corners
13        5.0     // radius of bottom corners
14    );
15
16    // Apply a solid red fill to the box.
17    sdf.fill(#f00);
18
19    // Return the final color result.
20    return sdf.result;
21}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/fields/result (vec4)

**Contents:**
- #result

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/fill_premul

**Contents:**
- #fill_premul
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes
  - #Important Considerations

The fill_premul function fills the current shape in the Sdf2d drawing context with a pre-multiplied RGBA color. It blends the provided color with the existing content and resets the internal shape and clipping parameters, preparing the context for subsequent drawing operations.

**Examples:**

Example 1 (php):
```php
1fn fill_premul(inout self, color: vec4) -> vec4
```

Example 2 (julia):
```julia
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a rectangle with rounded corners.
6    sdf.box(10.0, 10.0, 80.0, 80.0, 5.0);
7
8    // Apply a semi-transparent red fill using pre-multiplied alpha.
9    // The RGB components are multiplied by the alpha (0.5).
10    sdf.fill_premul(vec4(1.0 * 0.5, 0.0 * 0.5, 0.0 * 0.5, 0.5));
11
12    // Rotate the drawing by 45 degrees around the center of the rectangle.
13    sdf.rotate(PI * 0.25, 50.0, 50.0);
14
15    // Return the final color result.
16    return sdf.result;
17}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/radians

**Contents:**
- #radians
- #Declaration
- #Parameters
- #Description
- #See Also

convert a quantity in degrees to radians

radians converts a quantity, specified in degrees into radians. That is, the return value is $$ \frac{\pi \cdot \textit{degrees}}{180} $$

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/fields/aa (float)

**Contents:**
- #aa

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/sqrt

**Contents:**
- #sqrt
- #Declaration
- #Parameters
- #Description
- #See Also

return the square root of the parameter

sqrt returns the square root of x. i.e., the value $$ \sqrt{x} $$ Results are undefined if 𝑥<0.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/all

**Contents:**
- #all
- #Declaration
- #Parameters
- #Description
- #See Also

check whether all elements of a boolean vector are true

all returns true if all elements of x are true and false otherwise. It is functionally equivalent to:

**Examples:**

Example 1 (unknown):
```unknown
1bool all(bvec x)       // bvec can be bvec2, bvec3 or bvec4
2    {
3        bool result = true;
4        int i;
5        for (i = 0; i < x.length(); ++i)
6        {
7            result &= x[i];
8        }
9        return result;
10    }
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/lessThan

**Contents:**
- #lessThan
- #Declaration
- #Parameters
- #Description
- #See Also

perform a component-wise less-than comparison of two vectors

lessThan returns a boolean vector in which each element i is computed as x[i] < y[i].

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/dFdx

**Contents:**
- #dFdx, dFdy
- #Declaration
- #Parameters
- #Description

return the partial derivative of an argument with respect to x or y

Available only in the fragment shader, dFdx and dFdy return the partial derivative of expression p in x and y, respectively. Deviatives are calculated using local differencing. Expressions that imply higher order derivatives such as dFdx(dFdx(n)) have undefined results, as do mixed-order derivatives such as dFdx(dFdy(n)). It is assumed that the expression p is continuous and therefore, expressions evaluated via non-uniform control flow may be undefined.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/smoothstep

**Contents:**
- #smoothstep
- #Declaration
- #Parameters
- #Description
- #See Also

perform Hermite interpolation between two values

smoothstep performs smooth Hermite interpolation between 0 and 1 when edge0 < x < edge1. This is useful in cases where a threshold function with a smooth transition is desired. smoothstep is equivalent to:

smoothstep returns 0.0 if x ≤ edge0 and 1.0 if x ≥ edge1. Results are undefined if edge0 ≥ edge1.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/fract

**Contents:**
- #fract
- #Declaration
- #Parameters
- #Description
- #See Also

compute the fractional part of the argument

fract returns the fractional part of x. This is calculated as x - floor(x).

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/bool/subtract

**Contents:**
- #subtract
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The subtract function modifies the current shape in the Sdf2d drawing context by subtracting the previously stored shape from it. The result is a new shape representing the area of the current shape minus the overlapping area with the stored shape. This operation is useful when you want to cut out a portion of a shape using another shape.

Create Drawing Context: Initialize the Sdf2d context using the current position and size of the viewport.

Draw Base Shape: Use sdf.circle(50.0, 50.0, 40.0) to draw a circle centered at (50.0, 50.0) with a radius of 40.0.

Store Shape: Call sdf.union() to store the current shape (the circle) in self.old_shape.

Draw Subtracting Shape: Draw a rectangle overlapping the circle using sdf.rect(40.0, 40.0, 20.0, 20.0).

Subtract Shapes: Use sdf.subtract() to subtract the stored shape (the circle) from the current shape (the rectangle), resulting in a shape representing the rectangle minus the overlapping area with the circle.

Fill Shape: Fill the resulting shape with red color using sdf.fill(#f00).

Return Result: Return sdf.result, which contains the final rendered image showing the subtracted shape.

Shape Composition: The subtract function allows you to create shapes by cutting out parts of one shape using another. This is useful for creating holes, notches, or complex silhouettes.

Order of Operations: Ensure that you store the initial shape using sdf.union() before drawing the subtracting shape and applying sdf.subtract().

Transformations: You can apply transformations like translate, rotate, or scale to manipulate the shapes before subtracting them.

Combining Shapes: The subtract function is part of a set of Boolean operations that allows you to combine shapes in various ways. Other related functions include sdf.union() and sdf.intersect().

Further Effects: After subtracting shapes, you can apply effects like sdf.glow() or sdf.stroke() to enhance the visual appearance.

**Examples:**

Example 1 (unknown):
```unknown
1fn subtract(inout self)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw the base shape: a circle centered at (50.0, 50.0) with a radius of 40.0.
6    sdf.circle(50.0, 50.0, 40.0);
7
8    // Store the base shape for subtraction.
9    sdf.union(); // Store the current shape.
10
11    // Draw the subtracting shape: a rectangle overlapping the circle.
12    sdf.rect(40.0, 40.0, 20.0, 20.0);
13
14    // Subtract the stored shape (circle) from the current shape (rectangle).
15    sdf.subtract();
16
17    // Fill the resulting shape with a solid red color.
18    sdf.fill(#f00);
19
20    // Return the final color result.
21    return sdf.result;
22}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/degrees

**Contents:**
- #degrees
- #Declaration
- #Parameters
- #Description
- #See Also

convert a quantity in radians to degrees

degrees converts a quantity, specified in radians into degrees. That is, the return value is $$ \frac{180 \cdot \textit{radians}}{\pi} $$

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/reflect

**Contents:**
- #reflect
- #Declaration
- #Parameters
- #Description
- #See Also

calculate the reflection direction for an incident vector

For a given incident vector I and surface normal N reflect returns the reflection direction calculated as I - 2.0 * dot(N, I) * N. N should be normalized in order to achieve the desired result.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/transform/translate

**Contents:**
- #translate
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The translate function applies a translation transformation to the coordinate system of the Sdf2d drawing context. It shifts the origin of the coordinate system by the specified amounts along the x and y axes. This affects all subsequent drawing operations, allowing you to move shapes and paths within the context.

self (inout): A reference to the Sdf2d instance. The function modifies self.pos in place, updating the coordinate system of the drawing context.

x (float): The distance to translate along the x-axis. Positive values move the drawing context to the left, negative values move it to the right.

y (float): The distance to translate along the y-axis. Positive values move the drawing context downward, negative values move it upward.

Initialize Drawing Context: We create an Sdf2d instance by scaling self.pos by self.rect_size, which represents the size of the viewport.

Apply Translation: We call sdf.translate(30.0, 20.0) to shift the coordinate system. This moves the origin (0, 0) of the drawing context 30 units to the right and 20 units upward. Note that translation values subtract from self.pos.

Draw Shape: We draw a rectangle starting at position (10.0, 10.0) with a width and height of 80.0 units and corner radius of 5.0. Because of the translation, this rectangle appears shifted in the output.

Apply Fill: We fill the rectangle with red color using sdf.fill(#f00).

Return Result: We return sdf.result, which contains the final rendered color after the drawing operations.

Direction of Translation: The translate function subtracts the given values from the current position (self.pos). This means that positive values of x and y move the drawing context to the left and downward, respectively, relative to the original coordinate system.

Effect on Subsequent Drawing: All drawing operations after the translation are affected by the coordinate shift. This allows you to position multiple shapes relative to the translated coordinate system.

Cumulative Transformations: Multiple transformations (e.g., translate, rotate, scale) are cumulative and affect subsequent drawing operations in the order they are applied.

Return Value: The function returns the updated self.pos, which can be useful if you need to know the new position after translation for calculations.

**Examples:**

Example 1 (php):
```php
1fn translate(inout self, x: float, y: float) -> vec2
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Initialize the Sdf2d drawing context relative to the viewport size.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4    
5    // Apply translation of 30 units left and 20 units down.
6    sdf.translate(30.0, 20.0);
7    
8    // Draw a rectangle with rounded corners after translation.
9    sdf.box(10.0, 10.0, 80.0, 80.0, 5.0);
10    
11    // Fill the rectangle with red color.
12    sdf.fill(#f00);
13    
14    // Return the final color result.
15    return sdf.result;
16}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/index

**Contents:**
- #简介
- #Makepad 速查手册
- #Sdf2d API

Makepad 提供了一系列 API，让你可以与 Makepad 平台进行交互。本文档概述了可用的 API、其用法和示例。

《Makepad 速查手册》是面向希望开始使用 Makepad 的开发人员的综合资源。它涵盖了有效使用该平台的关键概念、功能和最佳实践。

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/primitives/arc2

**Contents:**
- #arc2
- #Parameters
- #Returns
- #Example
  - #Explanation
- #Notes

The arc2 function draws an arc segment of a circle within the Sdf2d drawing context. It defines a portion of a circle centered at (x, y) with radius r, starting from angle s and ending at angle e. The arc is shaped based on the current position within the drawing context.

**Examples:**

Example 1 (unknown):
```unknown
1fn arc2(inout self, x: float, y: float, r: float, s: float, e: float)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4    
5    // Draw an arc centered at (50.0, 50.0) with a radius of 40.0,
6    // starting from 0 radians (0 degrees) to PI radians (180 degrees).
7    sdf.arc2(50.0, 50.0, 40.0, 0.0, PI);
8    
9    // Apply a solid red fill to the arc.
10    sdf.fill(#f00);
11    
12    // Return the final color result.
13    return sdf.result;
14}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/greaterThanEqual

**Contents:**
- #greaterThanEqual
- #Declaration
- #Parameters
- #Description
- #See Also

perform a component-wise greater-than-or-equal comparison of two vectors

greaterThanEqual returns a boolean vector in which each element i is computed as x[i] ≥ y[i].

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/fields/scale_factor (float)

**Contents:**
- #scale_factor

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/log2

**Contents:**
- #log2
- #Declaration
- #Parameters
- #Description
- #See Also

return the base 2 logarithm of the parameter

log2 returns the base 2 logarithm of x. i.e., the value 𝑦 which satisfies $$ x = 2^{y} $$ Results are undefined if x ≤ 0.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/transpose

**Contents:**
- #transpose
- #Declaration
- #Parameters
- #Description
- #See Also

calculate the transpose of a matrix

transpose returns the transpose of the matrix m.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/fields/blur (float)

**Contents:**
- #blur

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/not

**Contents:**
- #not
- #Declaration
- #Parameters
- #Description
- #See Also

logically invert a boolean vector

not logically inverts the boolean vector x. It returns a new boolean vector for which each element i is computed as !x[i].

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/fields/shape (float)

**Contents:**
- #shape

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/ceil

**Contents:**
- #ceil
- #Declaration
- #Parameters
- #Description
- #See Also

find the nearest integer that is greater than or equal to the parameter

ceil returns a value equal to the nearest integer that is greater than or equal to x.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/fields/dist (float)

**Contents:**
- #dist

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/abs

**Contents:**
- #abs
- #Declaration
- #Parameters
- #Description
- #See Also

return the absolute value of the parameter

abs returns x if x ≥ 0, otherwise returns -x.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/effects/gloop

**Contents:**
- #gloop
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes
  - #Additional Information

The gloop function blends the current shape in the Sdf2d drawing context with the previously stored shape, creating a smooth, organic transition between them. This blending technique is often used to produce fluid, blobby effects similar to metaballs.

**Examples:**

Example 1 (unknown):
```unknown
1fn gloop(inout self, k: float)
```

Example 2 (julia):
```julia
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw the first circle at position (40.0, 50.0) with a radius of 30.0.
6    sdf.circle(40.0, 50.0, 30.0);
7
8    // Store the current shape as the starting point for blending.
9    sdf.union();
10
11    // Draw the second circle at position (70.0, 50.0) with a radius of 30.0.
12    sdf.circle(70.0, 50.0, 30.0);
13
14    // Blend the two shapes using the gloop function with smoothing factor 10.0.
15    sdf.gloop(10.0);
16
17    // Fill the blended shape with a solid red color.
18    sdf.fill(#f00);
19
20    // Return the final color result.
21    return sdf.result;
22}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/primitives/hexagon

**Contents:**
- #hexagon
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes
  - #Additional Information

The hexagon function draws a regular hexagon within the Sdf2d drawing context. The hexagon is defined by its center coordinates and radius.

**Examples:**

Example 1 (unknown):
```unknown
1fn hexagon(inout self, x: float, y: float, r: float)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a hexagon centered at (50.0, 50.0) with a radius of 40.0.
6    sdf.hexagon(50.0, 50.0, 40.0);
7
8    // Fill the hexagon with a solid red color.
9    sdf.fill(#f00);
10
11    // Rotate the drawing by 30 degrees around the center of the hexagon.
12    sdf.rotate(PI / 6.0, 50.0, 50.0);
13
14    // Return the final color result.
15    return sdf.result;
16}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/cos

**Contents:**
- #cos
- #Declaration
- #Parameters
- #Description
- #See Also

return the cosine of the parameter

cos returns the trigonometric cosine of angle.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/distance

**Contents:**
- #distance
- #Declaration
- #Parameters
- #Description
- #See Also

calculate the distance between two points

distance returns the distance between the two points p0 and p1. i.e., length(p0 - p1);

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/effects/glow_keep

**Contents:**
- #glow_keep
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The glow_keep function applies a glowing effect to the current shape in the Sdf2d drawing context by blending a specified color with the existing result based on the glow width. This function preserves the current shape and clipping settings while applying the glow effect, allowing you to layer effects without resetting the drawing state.

**Examples:**

Example 1 (php):
```php
1fn glow_keep(inout self, color: vec4, width: float) -> vec4
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4    
5    // Draw a circle centered at (50.0, 50.0) with a radius of 30.0.
6    sdf.circle(50.0, 50.0, 30.0);
7    
8    // Apply a red glow effect with a width of 10.0 units, preserving the shape state.
9    sdf.glow_keep(#f00, 10.0);
10    
11    // Optionally, fill the shape with another color without resetting the state.
12    sdf.fill_keep(#00f);
13    
14    // Return the final color result.
15    return sdf.result;
16}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/mix

**Contents:**
- #mix
- #Declaration
- #Parameters
- #Description
- #See Also

linearly interpolate between two values

mix performs a linear interpolation between x and y using a to weight between them. The return value is computed as follows: 𝑥⋅(1−𝑎)+𝑦⋅𝑎.

For the variants of mix where a is genBType, elements for which a[i] is false, the result for that element is taken from x, and where a[i] is true, it will be taken from y. Components of x and y that are not selected are allowed to be invalid floating point values and will have no effect on the results.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/sin

**Contents:**
- #sin
- #Declaration
- #Parameters
- #Description
- #See Also

return the sine of the parameter

sin returns the trigonometric sine of angle.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/clamp

**Contents:**
- #clamp
- #Declaration
- #Parameters
- #Description
- #See Also

constrain a value to lie between two further values

clamp returns the value of x constrained to the range minVal to maxVal. The returned value is computed as [[min]]([[max]](x, minVal), maxVal). The result is undefined if minVal ≥ maxVal.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/transform/scale

**Contents:**
- #scale
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes
  - #Important Considerations
  - #Alternative Example with Zoom In
  - #Summary

The scale function applies a scaling transformation to the coordinate system of the Sdf2d drawing context around a specified pivot point. This transformation scales all subsequent drawing operations, allowing you to zoom in or out relative to a specific point.

Create Drawing Context: We initialize an Sdf2d drawing context scaled to the size of the viewport (self.rect_size).

Apply Scaling: We apply a scaling transformation by a factor of 0.5 using sdf.scale(0.5, 50.0, 50.0). The scaling is performed around the pivot point (50.0, 50.0), effectively zooming out, as the scaling factor is less than 1.0.

Draw Shape: After applying the scaling, we draw a rectangle using sdf.box(20.0, 20.0, 60.0, 60.0, 5.0). Due to the scaling, the rectangle is drawn at half its original size relative to the pivot point.

Apply Fill: We fill the rectangle with red color #f00 by calling sdf.fill(#f00).

Return Result: We return sdf.result, which contains the final rendered color after all drawing operations.

Scaling Factor (f): The value of f determines the scaling. A value greater than 1.0 enlarges the drawing (zoom in), while a value between 0.0 and 1.0 reduces it (zoom out).

Pivot Point: The scaling occurs relative to the pivot point (x, y). Changing the pivot point allows you to zoom relative to different locations in your drawing.

Order of Operations: The scale function affects the coordinate system from the point it is called onward. Any drawing commands issued after the scaling will be scaled accordingly. To scale existing shapes, apply the scaling before the drawing commands.

Cumulative Transformations: Transformations such as scale, rotate, and translate are cumulative and affect subsequent drawing operations in the order they are applied.

Effect on Strokes and Line Widths: Scaling affects all aspects of subsequent drawings, including stroke widths and line sizes, which are scaled accordingly.

Coordinate Transformation: The scale function modifies the position self.pos and updates self.scale_factor using the formula:

This scales the distance from the current position to the pivot point by the scaling factor f.

Resetting Scale: If you need to reset the scale for further drawing operations, you may need to apply an inverse scaling or reinitialize the Sdf2d context.

To zoom in instead of zooming out, use a scaling factor greater than 1.0:

The scale function is particularly useful when you want to zoom in or out of your drawing relative to a specific point. By scaling the drawing context before issuing drawing commands, all subsequent shapes are transformed accordingly, simplifying the process of rendering scaled elements in your graphics.

**Examples:**

Example 1 (unknown):
```unknown
1fn scale(inout self, f: float, x: float, y: float)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context relative to the viewport size.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Apply scaling by a factor of 0.5 around point (50.0, 50.0).
6    sdf.scale(0.5, 50.0, 50.0);
7
8    // Draw a rectangle with rounded corners after scaling.
9    sdf.box(20.0, 20.0, 60.0, 60.0, 5.0);
10
11    // Fill the rectangle with red color.
12    sdf.fill(#f00);
13
14    // Return the final color result.
15    return sdf.result;
16}
```

Example 3 (python):
```python
1self.scale_factor *= f;
2self.pos = (self.pos - vec2(x, y)) * f + vec2(x, y);
```

Example 4 (php):
```php
1fn pixel(self) -> vec4 {
2    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
3
4    // Apply scaling by a factor of 2.0 around point (50.0, 50.0).
5    sdf.scale(2.0, 50.0, 50.0);
6
7    // Draw a rectangle with rounded corners after scaling.
8    sdf.box(10.0, 10.0, 80.0, 80.0, 5.0);
9
10    // Fill the rectangle with blue color.
11    sdf.fill(#00f);
12
13    return sdf.result;
14}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/fields/has_clip (float)

**Contents:**
- #has_clip

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/any

**Contents:**
- #any
- #Declaration
- #Parameters
- #Description
- #See Also

check whether any element of a boolean vector is true

any returns true if any element of x is true and false otherwise. It is functionally equivalent to:

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/stroke

**Contents:**
- #stroke
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The stroke function applies a stroke to the current shape within the Sdf2d drawing context, blending a specified color along the edge of the shape based on the stroke width. Unlike stroke_keep, the stroke function resets the internal shape and clipping state after performing the stroke operation, making it ready for a new shape definition.

**Examples:**

Example 1 (php):
```php
1fn stroke(inout self, color: vec4, width: float) -> vec4
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a rectangle with rounded corners.
6    sdf.box(10.0, 10.0, 80.0, 80.0, 5.0);
7
8    // Apply a stroke with a width of 2.5 units and red color.
9    sdf.stroke(#f00, 2.5);
10
11    // Since 'stroke' resets the shape state, we can define a new shape.
12    // For example, draw a circle.
13    sdf.circle(50.0, 50.0, 30.0);
14
15    // Apply a different stroke to the new shape.
16    sdf.stroke(#00f, 1.5);
17
18    // Return the final color result.
19    return sdf.result;
20}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/notEqual

**Contents:**
- #notEqual
- #Declaration
- #Parameters
- #Description
- #See Also

perform a component-wise not-equal-to comparison of two vectors.

notEqual returns a boolean vector in which each element i is computed as x[i] != y[i].

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/primitives/box_x

**Contents:**
- #box_x
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The box_x function draws a rectangle within the Sdf2d drawing context, allowing you to specify different corner radii for the left and right sides. This enables the creation of rectangles with asymmetric rounded corners along the horizontal axis.

**Examples:**

Example 1 (unknown):
```unknown
1fn box_x(inout self, x: float, y: float, w: float, h: float, r_left: float, r_right: float)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a rectangle at position (10.0, 10.0) with width 100.0 and height 80.0.
6    // Left corners have a radius of 5.0, right corners have a radius of 15.0.
7    sdf.box_x(
8        10.0,  // x position
9        10.0,  // y position
10        100.0, // width
11        80.0,  // height
12        5.0,   // radius of left corners
13        15.0   // radius of right corners
14    );
15
16    // Apply a solid red fill to the box.
17    sdf.fill(#f00);
18
19    // Return the final color result.
20    return sdf.result;
21}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/acos

**Contents:**
- #acos
- #Declaration
- #Parameters
- #Description
- #See Also

return the arccosine of the parameter

atan returns the angle whose trigonometric cosine is x. The range of values returned by acos is [0,π]. The result is undefined if |x|>1.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/effects/glow

**Contents:**
- #glow
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes
  - #Additional Information

The glow function applies a glowing effect to the current shape in the Sdf2d drawing context by blending a specified color around the edges of the shape based on the glow width. Unlike glow_keep, the glow function resets the internal shape and clipping state after applying the glow effect, preparing the context for new drawing operations.

Create Drawing Context: Initialize the Sdf2d context using the current position (self.pos) multiplied by self.rect_size, which represents the size of the viewport.

Draw First Shape: Use sdf.box to draw a rectangle starting at position (10.0, 10.0) with a width and height of 80.0 units and a corner radius of 5.0.

Apply Glow to First Shape: Use sdf.glow to apply a blue glow (#88f) with a width of 10.0 units to the current shape (the rectangle). After this, the internal shape and clipping state are reset.

Draw Second Shape: Define a new shape—a circle centered at (50.0, 50.0) with a radius of 30.0.

Apply Glow to Second Shape: Apply an orange glow (#f80) with a width of 5.0 units to the new shape.

Return Result: Return sdf.result, which contains the final rendered color after all drawing operations.

State Reset: Unlike glow_keep, the glow function resets the internal shape (shape, old_shape) and clipping (clip, has_clip) state after execution. This means you can start defining a new shape immediately after without manual resets.

Order of Operations: Ensure that you apply glow after defining the shape you wish to apply the effect to. Since glow resets the shape state, any shape you wish to affect must be defined before calling glow.

Glow Width Scaling: The width parameter is adjusted based on the scale_factor of the Sdf2d context, ensuring consistent glow size regardless of transformations.

Color Specification: Colors can be specified using hexadecimal notation (e.g., #88f for blue, #f80 for orange) or as vec4 values.

Multiple Glows: By using glow and defining new shapes between calls, you can apply multiple glow effects to different shapes sequentially.

Combining Effects: Since glow resets the shape state, you can combine it with other drawing operations to create complex visuals. Remember to define each shape before applying the glow to it.

Usage: Use glow when you want to apply a glow effect and prepare the context for new drawing operations without preserving the current shape and clipping settings.

Performance Consideration: Applying multiple glow effects can be computationally intensive. Optimize by minimizing the number of glow operations when possible.

Applications: The glow function is useful for highlighting elements, creating neon effects, or adding emphasis to shapes in your graphics or UI designs.

**Examples:**

Example 1 (php):
```php
1fn glow(inout self, color: vec4, width: float) -> vec4
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a rectangle with rounded corners.
6    sdf.box(10.0, 10.0, 80.0, 80.0, 5.0);
7
8    // Apply a blue glow effect with a width of 10.0 units.
9    sdf.glow(#88f, 10.0);
10
11    // Since 'glow' resets the shape state, we can define a new shape.
12    // Draw a circle inside the rectangle.
13    sdf.circle(50.0, 50.0, 30.0);
14
15    // Apply an orange glow effect to the new shape.
16    sdf.glow(#f80, 5.0);
17
18    // Return the final color result.
19    return sdf.result;
20}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/fields/pos (vec2)

**Contents:**
- #post

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/length

**Contents:**
- #length
- #Declaration
- #Parameters
- #Description
- #See Also

calculate the length of a vector

length returns the length of the vector. i.e., $$ \sqrt{x[0]^2 + x[1]^2 + \dots} $$

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/viewport

**Contents:**
- #viewport
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes
  - #Additional Information

The viewport function creates and returns a new instance of the Sdf2d drawing context, initialized with default values for rendering operations. This function sets up a new drawing context with predefined settings such as position, anti-aliasing, clipping, and other rendering-related parameters. It is commonly used to initialize the drawing context within the pixel shader function.

Create Drawing Context: We initialize an Sdf2d drawing context by scaling self.pos (the normalized position in the viewport) by self.rect_size (the size of the rectangle or viewport). This maps the position to pixel coordinates.

Draw Shape: We use sdf.box to draw a rectangle starting at position (1.0, 1.0) with a width and height of 100.0 units, and a corner radius of 3.0.

Apply Fill: We fill the rectangle with red color using sdf.fill(#f00).

Rotate Drawing: We rotate the entire drawing context by 90 degrees (PI * 0.5 radians) around the point (50.0, 50.0).

Return Result: We return sdf.result, which contains the final rendered color after all drawing operations.

Initialization: The viewport function initializes the Sdf2d context with default values:

Context Usage: After creating the Sdf2d context with viewport, you can perform various drawing operations like drawing shapes, applying transformations, and rendering effects.

Coordinate Mapping: Multiplying self.pos by self.rect_size maps the normalized device coordinates to the actual pixel coordinates of the viewport, which is necessary for accurate positioning of shapes.

Transformation Order: Remember that transformations like rotate, scale, and translate affect all subsequent drawing commands. Apply them in the correct order to achieve the desired effect.

Anti-Aliasing: The aa (anti-aliasing) factor is automatically initialized based on the input pos, helping to smooth edges and improve visual quality.

Custom Initialization: You can modify the initialization by directly setting or modifying the fields of the Sdf2d context after creating it with viewport, if needed.

Multiple Viewports: If you need to render multiple separate drawing contexts, you can create multiple Sdf2d instances using viewport with different positions or contexts.

Integration with Shader Code: The viewport function is typically used within the pixel shader function to initialize the drawing context for per-pixel operations.

**Examples:**

Example 1 (php):
```php
1fn viewport(pos: vec2) -> Self
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context relative to the viewport size.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a rectangle with rounded corners.
6    sdf.box(1.0, 1.0, 100.0, 100.0, 3.0);
7
8    // Fill the rectangle with red color.
9    sdf.fill(#f00);
10
11    // Rotate the drawing by 90 degrees (PI * 0.5 radians) around point (50.0, 50.0).
12    sdf.rotate(PI * 0.5, 50.0, 50.0);
13
14    // Return the final color result.
15    return sdf.result;
16}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/close_path

**Contents:**
- #close_path
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The close_path function completes the current path by drawing a line from the last point back to the starting point. This effectively closes the shape, ensuring that it is a continuous loop. It's commonly used when you need to create closed shapes like polygons.

**Examples:**

Example 1 (unknown):
```unknown
1fn close_path(inout self)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Start a new path at point (50.0, 10.0).
6    sdf.move_to(50.0, 10.0);
7
8    // Draw lines to create a triangle.
9    sdf.line_to(90.0, 80.0);   // Line to point (90.0, 80.0).
10    sdf.line_to(10.0, 80.0);   // Line to point (10.0, 80.0);
11    
12    // Close the path, connecting back to the starting point.
13    sdf.close_path();
14
15    // Fill the shape with a solid red color.
16    sdf.fill(#f00);
17
18    // Return the final color result.
19    return sdf.result;
20}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/fill_keep_premul

**Contents:**
- #fill_keep_premul
- #Parameters
- #Returns
- #Example

The fill_keep_premul function blends a pre-multiplied source color with the current result color of the SDF2D drawing context, preserving the existing content while applying the new fill.

This function is particularly useful when you want to add a new fill color to your drawing without overwriting the existing content, allowing for complex blending effects in your shaders.

**Examples:**

Example 1 (php):
```php
1fn fill_keep_premul(inout self, source: vec4) -> vec4
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a rectangle with rounded corners.
6    sdf.box(10.0, 10.0, 80.0, 80.0, 5.0);
7
8    // Apply a semi-transparent red fill while keeping existing content.
9    sdf.fill_keep_premul(vec4(1.0, 0.0, 0.0, 0.5));
10
11    // Rotate the drawing by 45 degrees around the center of the rectangle.
12    sdf.rotate(PI * 0.25, 50.0, 50.0);
13
14    // Return the final color result.
15    return sdf.result;
16}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/mod

**Contents:**
- #mod
- #Declaration
- #Parameters
- #Description
- #See Also

compute value of one parameter modulo another

mod returns the value of x modulo y. This is computed as x - y * floor(x/y).

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/dot

**Contents:**
- #dot
- #Declaration
- #Parameters
- #Description
- #See Also

calculate the dot product of two vectors

dot returns the dot product of two vectors, x and y. i.e., $$ x[0] \cdot y[0] + x[1] \cdot y[1] + \ldots $$

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/move_to

**Contents:**
- #move_to
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The move_to function sets the starting point of a new path in the Sdf2d drawing context. It updates the current position to the specified coordinates (x, y) without drawing a line from the previous position. This is commonly used when beginning a new shape or repositioning the drawing cursor without connecting it to the previous path.

self (inout): A reference to the Sdf2d instance. The function modifies the start_pos and last_pos fields of self to the new position (x, y).

x (float): The x-coordinate of the new starting point.

y (float): The y-coordinate of the new starting point.

Initialize Drawing Context: We create an Sdf2d instance for drawing, scaled to the current rectangle size.

Start New Path: Use sdf.move_to(50.0, 20.0) to set the starting point of the path at (50.0, 20.0).

Draw Lines: Draw lines to two other points using sdf.line_to, forming the sides of a triangle.

Close Path: Call sdf.close_path() to draw a line back to the starting point, completing the triangle.

Fill Shape: Fill the shape with red color using sdf.fill(#f00).

Result: Return sdf.result, which contains the final rendered image.

Starting Point: The move_to function moves the drawing cursor without creating a line, essentially lifting the pen up and moving to a new location.

Path Construction: Combining move_to, line_to, and close_path allows you to define complex shapes by specifying their vertices.

State Update: After calling move_to, both start_pos and last_pos are updated to the new coordinates, preparing for subsequent drawing commands.

**Examples:**

Example 1 (unknown):
```unknown
1fn move_to(inout self, x: float, y: float)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Initialize the Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Move to the starting point of the path at (50.0, 20.0).
6    sdf.move_to(50.0, 20.0);
7
8    // Draw lines to create a triangle.
9    sdf.line_to(80.0, 70.0);   // Line to (80.0, 70.0).
10    sdf.line_to(20.0, 70.0);   // Line to (20.0, 70.0).
11    sdf.close_path();          // Close the path back to (50.0, 20.0).
12
13    // Fill the triangle with a solid red color.
14    sdf.fill(#f00);
15
16    // Return the final color result.
17    return sdf.result;
18}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/floor

**Contents:**
- #floor
- #Declaration
- #Parameters
- #Description

find the nearest integer less than or equal to the parameter

floor returns a value equal to the nearest integer that is less than or equal to x.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/draw/primitives/box

**Contents:**
- #box
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The box function draws a rectangle with rounded corners within the Sdf2d drawing context. The rectangle is defined by its position, dimensions, and a uniform corner radius applied to all corners.

**Examples:**

Example 1 (unknown):
```unknown
1fn box(inout self, x: float, y: float, w: float, h: float, r: float)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw a rectangle at position (10.0, 10.0) with width 100.0, height 100.0,
6    // and corner radius of 5.0.
7    sdf.box(10.0, 10.0, 100.0, 100.0, 5.0);
8
9    // Fill the shape with a solid red color.
10    sdf.fill(#f00);
11
12    // Return the final color result.
13    return sdf.result;
14}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/clear

**Contents:**
- #clear
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes

The clear function resets the current drawing result in the Sdf2d context by blending a specified color over the existing content, taking into account the alpha component for transparency. This is typically used to clear the drawing area before commencing new drawing operations.

self (inout): A reference to the Sdf2d instance. The function modifies the result field of self in place.

color (vec4): An RGBA color used to clear the drawing context.

Create Drawing Context: Initialize the Sdf2d context using the current position scaled by self.rect_size.

Clear the Context: Call sdf.clear(#181818) to clear the drawing context with a dark gray color (#181818). This blends the specified color over any existing content, taking the alpha component into account.

Draw Shape: Draw a rectangle starting at position (10.0, 10.0), with width and height adjusted to fit within the context (self.rect_size.x - 20.0, self.rect_size.y - 20.0), and with rounded corners of radius 5.0.

Apply Fill: Fill the rectangle with red color using sdf.fill(#f00).

Return Result: Return sdf.result, which contains the final rendered color.

Blending Behavior: The clear function blends the provided color with the existing content using the alpha component. If you want to completely overwrite the existing content, ensure that the alpha component is set to 1.0.

Usage: Use sdf.clear() at the beginning of your pixel function to reset the drawing context before starting new drawing operations.

Color Specification: Colors can be specified using hexadecimal notation (e.g., #181818 for dark gray) or as vec4 values.

Order of Operations: The clear function should be called before any drawing operations to ensure the context is reset.

**Examples:**

Example 1 (unknown):
```unknown
1fn clear(inout self, color: vec4)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Clear the drawing context with a dark gray color.
6    sdf.clear(#181818);
7
8    // Draw a rectangle with rounded corners.
9    sdf.box(10.0, 10.0, self.rect_size.x - 20.0, self.rect_size.y - 20.0, 5.0);
10
11    // Fill the rectangle with red color.
12    sdf.fill(#f00);
13
14    // Return the final color result.
15    return sdf.result;
16}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/transform/rotate

**Contents:**
- #rotate
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes
  - #Alternative Example without Rotation
  - #Summary

The rotate function applies a rotation transformation to the coordinate system of the Sdf2d drawing context around a specified pivot point. This transformation affects all subsequent drawing operations, allowing you to rotate shapes and paths within the context.

Create Drawing Context: We initialize an Sdf2d drawing context scaled to the size of the rectangle (self.rect_size), which represents the viewport area for drawing.

Apply Rotation: We apply a rotation of 90 degrees clockwise by calling sdf.rotate(PI * 0.5, 50.0, 50.0). The rotation is performed around the pivot point (50.0, 50.0), which is the center of the rectangle we will draw.

Draw Shape: After applying the rotation, we draw a rectangle using sdf.box(1.0, 1.0, 100.0, 100.0, 3.0). The rectangle starts at position (1.0, 1.0), has a width and height of 100.0 units, and corners rounded with a radius of 3.0.

Apply Fill: We fill the rectangle with red color #f00 by calling sdf.fill(#f00).

Return Result: We return sdf.result, which contains the final rendered color after all drawing operations.

Rotation Direction: The rotation angle a is in radians. Due to the implementation using cos(-a) and sin(-a), positive values of a result in a clockwise rotation, which may differ from common mathematical conventions.

Pivot Point: The rotation occurs around the specified pivot point (x, y). Changing the pivot point adjusts the center of rotation, allowing for rotations around different points in your drawing.

Order of Operations: The rotate function affects the coordinate system from the point it is called onward. Any drawing commands issued after the rotation will be rotated accordingly. To rotate existing shapes, apply the rotation before the drawing commands.

Transformations Stack: Transformations such as rotate, translate, and scale are cumulative and affect subsequent drawing operations in the order they are applied.

If you apply the rotation after drawing the shape, the rotation won't affect the shape:

In this case, the rectangle will not appear rotated because the rotation was applied after the drawing commands.

The rotate function is particularly useful when you want to rotate shapes or patterns without manually calculating the rotated coordinates for each point. By rotating the drawing context before issuing drawing commands, all subsequent shapes are transformed accordingly, simplifying the process of rendering rotated elements in your graphics.

**Examples:**

Example 1 (unknown):
```unknown
1fn rotate(inout self, a: float, x: float, y: float)
```

Example 2 (php):
```php
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context relative to the viewport size.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Apply rotation by 90 degrees (PI * 0.5 radians) clockwise around point (50.0, 50.0).
6    sdf.rotate(PI * 0.5, 50.0, 50.0);
7
8    // Draw a rectangle with rounded corners after rotation.
9    sdf.box(1.0, 1.0, 100.0, 100.0, 3.0);
10
11    // Fill the rectangle with red color.
12    sdf.fill(#f00);
13
14    // Return the final color result.
15    return sdf.result;
16}
```

Example 3 (php):
```php
1fn pixel(self) -> vec4 {
2    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
3
4    // Draw a rectangle without rotation.
5    sdf.box(1.0, 1.0, 100.0, 100.0, 3.0);
6
7    // Fill the rectangle with red color.
8    sdf.fill(#f00);
9
10    // Apply rotation (this won't affect the already drawn rectangle).
11    sdf.rotate(PI * 0.5, 50.0, 50.0);
12
13    return sdf.result;
14}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/methods/bool/union

**Contents:**
- #union
- #Parameters
- #Returns
- #Example
  - #Explanation
  - #Notes
  - #Important Considerations
  - #Alternative Example with More Shapes

The union function combines the current shape in the Sdf2d drawing context with the previously stored shape, resulting in a new shape that encompasses both the current and the stored shapes. This operation is useful for creating complex shapes by combining simpler ones, effectively merging them into a single shape.

Create Drawing Context: We initialize an Sdf2d drawing context scaled to the size of the current rectangle (self.rect_size).

First Shape: Draw a circle centered at (40.0, 50.0) with a radius of 30.0.

Store Shape: Use sdf.union() to store the current shape in self.old_shape, preparing to combine it with another shape.

Second Shape: Draw another circle centered at (70.0, 50.0) with the same radius.

Combine Shapes: Call sdf.union() again to combine the stored shape (self.old_shape) with the current shape (self.shape). The result is a new shape that encompasses both circles.

Apply Fill: Fill the combined shape with red color using sdf.fill(#f00).

Return Result: Return sdf.result, which contains the final rendered image showing the union of the two circles.

Shape Composition: The union function allows you to combine shapes by merging their areas, effectively uniting them into a single shape.

Order of Operations: When combining multiple shapes, you should call sdf.union() after drawing each shape to accumulate them. Each call to sdf.union() updates the internal state to include the new shape.

Transformation: You can apply transformations like translate, rotate, or scale to manipulate the shapes before combining them.

Additional Shapes: To combine more shapes, continue the pattern:

Further Effects: After combining shapes, you can apply effects like sdf.glow() or sdf.stroke() to enhance the visual appearance.

Preserving State: Since sdf.union() modifies the internal state but does not reset it, you can continue to add shapes or apply effects as needed.

Combining Multiple Shapes: Ensure you call sdf.union() after each new shape you want to include in the union.

Understanding Internal State: The union function updates self.old_shape and self.shape to represent the minimum of the current and stored shapes' signed distance fields, effectively merging them.

Performance: Be mindful of the number of shapes combined, as complex unions may impact rendering performance.

In this alternative example:

**Examples:**

Example 1 (unknown):
```unknown
1fn union(inout self)
```

Example 2 (julia):
```julia
1fn pixel(self) -> vec4 {
2    // Create an Sdf2d drawing context for the current viewport.
3    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
4
5    // Draw the first circle at position (40.0, 50.0) with a radius of 30.0.
6    sdf.circle(40.0, 50.0, 30.0);
7
8    // Store the current shape for the union operation.
9    sdf.union();
10
11    // Draw the second circle at position (70.0, 50.0) with a radius of 30.0.
12    sdf.circle(70.0, 50.0, 30.0);
13
14    // Combine the stored shape with the current shape using union.
15    sdf.union();
16
17    // Fill the combined shape with a solid red color.
18    sdf.fill(#f00);
19
20    // Return the final color result.
21    return sdf.result;
22}
```

Example 3 (php):
```php
1fn pixel(self) -> vec4 {
2    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
3
4    // Draw and combine multiple circles.
5    sdf.circle(40.0, 50.0, 30.0);
6    sdf.union(); // Store first circle.
7
8    sdf.circle(70.0, 50.0, 30.0);
9    sdf.union(); // Combine second circle.
10
11    sdf.circle(55.0, 80.0, 30.0);
12    sdf.union(); // Combine third circle.
13
14    // Fill the combined shape.
15    sdf.fill(#f00);
16
17    return sdf.result;
18}
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/normalize

**Contents:**
- #normalize
- #Declaration
- #Parameters
- #Description
- #See Also

calculate the normalize product of two vectors

normalize returns a vector with the same direction as its parameter, v, but with length 1.

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/atan

**Contents:**
- #atan
- #Declaration
- #Parameters
- #Description
- #See Also

return the arc-tangent of the parameters

atan returns the angle whose trigonometric arctangent is $$ \frac{y}{x} $$ or 𝑦_𝑜𝑣𝑒𝑟_𝑥, depending on which overload is invoked. In the first overload, the signs of 𝑦 and 𝑥 are used to determine the quadrant that the angle lies in. The values returned by atan in this case are in the range [−𝜋,𝜋]. Results are undefined if 𝑥 is zero. For the second overload, atan returns the angle whose tangent is 𝑦_𝑜𝑣𝑒𝑟_𝑥. Values returned in this case are in the range $$ \left[ -\frac{\pi}{2}, \frac{\pi}{2} \right] $$

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Sdf2d/builtin/matrixCompMult

**Contents:**
- #matrixCompMult
- #Declaration
- #Parameters
- #Description
- #See Also

perform a component-wise multiplication of two matrices

matrixCompMult performs a component-wise multiplication of two matrices, yielding a result matrix where each component, result[i][j] is computed as the scalar product of x[i][j] and y[i][j].

---
