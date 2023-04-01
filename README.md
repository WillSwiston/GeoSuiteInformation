# Geo Suite Information
The purpose of this document is to explain information about GeoSuite not present on my resume or within the site.

## Motivation
A few years ago, I came to the realization that I knew basically nothing about geography. When reading the news, I had heard of many of these places, but knew nothing about them nor where they actually were on a map. Once I started learning more about the subject, I fell in love with it, and started writing simple games. Most of the early iterations were, quite frankly, terrible, but eventually I came up with something that I am proud of -- GeoSuite.
## Technical Information

### Styling
Styling has always been a tough point for me, as I am not a designer. After working on a few personal projects, I have come up with a workflow that works decently well, and makes use of the "cascading" nature of CSS.

Styling begins with App.css, which defines several CSS variables to be used across the project, as well as styles which affect every element. I also make use of a CSS reset I found online to remove the default stylings from most elements, so as to have more control.

From there, each page of the website has a main JavaScript and CSS pair, which define variables for that page. Some components are used across the website, which also have their corresponding JS and CSS pair.

### Distance Calculation

Calculating distance between countries is tough for really one reason: the number of points a GeoJSON file has. For this reason, it is pretty much a necessity to prune the number of vertices before doing the calculation. The approach for the calcultion is incredibly simple, just exhaustively doing the Haversine formula (to account for curvature of the earth) between each point in one country and another, and taking the minimum value. While I am sure there are other solutions to improve this, simply stopping when the distance is 0 is the furthest improvement here I made.

However, pruning the vertices turned out to be an incredibly interesting problem.

Consider three border points of a country, $(x_1, y_1), (x_2, y_2), (x_3, y_3). We can calculate the area of the triangle made by these points using the Shoelace Formula:

$$S = \frac{1}{2}|x_1y_2 - x_2y_1 + x_2y_3 - x_3y_2 + x_3y_1 - x_1y_3|$$

We can do this for every on a country, taking note of the center vertex as the key. We can then add these to a priority queue, remove the smallest half, and reduce the number of points by half without losing much specificity.

The main intuition behind this idea is that the vertices which have the largest area are more "volatile", and more likely to be the actual smallest (or indeed largest) distance.

While generally this will not impact distance by any meaningful amount, I introduced a threshold to where it counts countries as bordering each other, to prevent weirdness when pruning vertices.
## Next Steps
