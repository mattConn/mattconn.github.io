---
title: "Checkerboard Moves"
date: 2020-09-17T02:59:12-04:00
draft: true
---

I was wondering the other day, if you were on a checkerboard, what would be your smallest move?

![](/images/checkerboard_1.png)  

If you could move to any color, you would have 8 choices; the squares in your neighborhood.

![](/images/checkerboard_2.png)

This is not just your neighborhood, but also your max amount of possible single moves.

Now what if you could only move to black squares? You lose half of your possible moves.

![](/images/checkerboard_4.png)

# Moving Beyond the Neighborhood
This move restriction becomes interesting outside of the neighborhood. Previously, without color restriction, movement from one square to any other would result in a straight path.

![](/images/checkerboard_5.png)

If we can only move to black squares, this path looks different:

![](/images/checkerboard_6.png)

This forces us into a Taxicab geometry; we are on a plane where the shortest distance between two points is no longer a straight line (euclidean distance) but an "L".

# Shortest (Permitted) Moves
Under color restrictions, the amount of your shortest possible moves __within your neighborhood__ has been reduced by half. However, the amount of shortest possible moves to __permitted squares__ (black squares) remains the same, but half of these moves leave the neighborhood.

![](/images/checkerboard_7.png)

# Shortest Paths
Your shortest path is then also affected. Without color restriction, your shortest path is a straight line and you travel 1 square to reach the nearest other square.

![](/images/checkerboard_8.png)


With color restriction, you no longer have a single shortest path, but 2, and they are no longer straight lines, but "L"'s, and you must travel an extra square to reach your destination no matter which path you choose.

![](/images/checkerboard_9.png)

This can be represented with a bipartite star graph:

![](/images/checkerboard_10.png)
