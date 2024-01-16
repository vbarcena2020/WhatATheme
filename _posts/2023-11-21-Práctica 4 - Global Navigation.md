---
title: Práctica 4 - Global Navigation
layout: post
post-image: "https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/RM_img.jpg?raw=true"
description: Programing a global navigation by myself.
tags:
- Práctica 4 - Global Navigation
- Gradient Path Planning
- Lidar
- Post
- Robótica Móvil
- Unibotics
- Python
- Docker
---

This post will show you how I progress in my task of subject "Robótica Móvil" to program a global navigation:

---

# **GLobal Navigation**
This task presents us with the situation of a labyrinth with a car in the center. We have to navigate to a target point selected by click on the map. Whit this target, the map and the position of the car we have to create a path and go to the target by the shortest path.

## Planning the Implementation
The method that I have decided to implement is a Gradient Path Planning.

With the map I create a gradient grid to set the cost of the values arround the car increasing the cost further away the point is from the car. When the grid is done I get the path with lowest cost and move throught it.

## Method Implementation
In this practice I will perform three subtasks, get the gradient grid, thicken the walls and make the path.

#### Gradient Grid process:
Firstly, I start in the target position previously got and set the cost in the lowest value. Then I look for the point arround it and if it isn't a wall and it doesn't have a value in the grid, I set the new cost int the grid and save the point in a list. Then I look for the next point in the list and the points arround it, and continue untill the point I am looking at is the same as the car position.

Also I increase the cost more in the points close in linear and more in the points close in diagonal.

![Grid Cost](https://cdn.discordapp.com/attachments/828395914145431612/1176475937592840233/grid_cost.png?ex=656f0199&is=655c8c99&hm=7b44705cbf795fd626664090db2ae36226f16868ccfcbd6a43b2286fe1d58bb6&)



#### Thicken Walls Process:
With the walls detected to not get to close to it when the car get the path I create this function to thicken the walls increasing the values of the cost of the near points in all directions in all the walls points previously saved. 

#### Make Path Process:  
With the gradient grid with the thicken walls I can get a path searching for the lowest cost on the points near the car and saving it and looking for more untill the point I am looking at is the target point. The saved list of points is the path I will follow.

## Used Libraries
The code libraries that I have used are numpy, math and queue: 
- The numpy library, is used to set the arrays. 
- I have used math to calculate all the values needed.
- Queue library, is used to create queues os points and walls.


## Code Functions
For this task I needed to create five functions. 

The first function, called "get_gradient" and "gradient_point", are the functions that are responsible to set the cost in the points around the point looking at.

- **Get Gradient function:**
 
        def gradient_point(point_x, point_y, point_cost):
            if map_[point_x][point_y] == FREE_MAP:
                if grid_map[point_x][point_y] == FREE_GRID:
                    grid_map[point_x][point_y] = point_cost
                    points.put([point_x, point_y])
            else:
                walls.put([point_x, point_y])   
                        
        def get_gradient(point):
            if point[0] < MAX_GRID - 1:
                grad_point(point[0] + 1, point[1], cost)
                
            if point[0] > MIN_GRID:
                grad_point(point[0] - 1, point[1], cost)
                
            if point[1] < MAX_GRID - 1:  
                grad_point(point[0], point[1] + 1, cost)

            if point[1] > MIN_GRID:   
                grad_point(point[0], point[1] - 1, cost)
                
            if point[0] > MIN_GRID and point[1] > MIN_GRID:   
                grad_point(point[0] - 1, point[1] - 1, cost + DIAGONAL_COST_INCREMENT)
                
            if point[0] > MIN_GRID and point[1] < MAX_GRID - 1:  
                grad_point(point[0] - 1, point[1] + 1, cost + DIAGONAL_COST_INCREMENT)

            if point[0] < MAX_GRID - 1 and point[1] < MAX_GRID - 1:
                grad_point(point[0] + 1, point[1] + 1, cost + DIAGONAL_COST_INCREMENT)

            if point[0] < MAX_GRID - 1 and point[1] > MIN_GRID:   
                grad_point(point[0] + 1, point[1] - 1, cost + DIAGONAL_COST_INCREMENT)


The second function, called "thicken_walls", is a function that is responsible to increase the cost of the points near to the walls.

- **Thicken Walls function:**

        def thicken_walls(target, grid_map):
            for i in range(walls.qsize()):
                wall = walls.get()
                if wall[0] != target[0] or wall[1] != target[1]:
                    for i in range(wall[0]-CLOSE_WALLS_POINTS, wall[0]+CLOSE_WALLS_POINTS+1):
                        for j in range(wall[1]-CLOSE_WALLS_POINTS, wall[1]+CLOSE_WALLS_POINTS+1):
                            if MIN_GRID < i < MAX_GRID and MIN_GRID < j < MAX_GRID and grid_map[i][j] != 0:
                                if i != target[0] and j != target[1]:
                                    grid_map[i][j] = grid_map[i][j] * 2
                                    
            return grid_map


The third function, called "make_path", is a function that is responsible t look for the lowest cost and get a path to arrive to the target.

- **Make Path function:**
        
        def make_path(target, car, grid):
            path_ = []
            i = car[0]
            j = car[1]
            aux = []
            while(i != target[0] or j != target[1]):
                aux = [car[0], car[1]]
                directions = [(i, j) for i in range(-RADIUS, RADIUS) for j in range(-RADIUS, RADIUS) if (i, j) != (0, 0)]
                for dir_i, dir_j in directions:
                    if MIN_GRID <= i + dir_i < MAX_GRID and MIN_GRID <= j + dir_j < MAX_GRID and grid[i + dir_i][j + dir_j] != 0 and grid[i + dir_i][j + dir_j] <= grid[aux[0]][aux[1]]:
                        aux = [i + dir_i, j + dir_j]

                i = aux[0]
                j = aux[1]
                path_ += [[j, i]]               
                i, j = next_is_target(i, j)

            path_ += [[target[1],target[0]]]
            
            GUI.showPath(path_)

            return path_

## Critical Analysis

Finally, I have to mention that depending on the values of the thicken walls and the values of the cost it will get the path or not. 

Also you can modify the values of the range to look for the lowest cost around a point to get the path.

## Gifts and videos

There is a gift which shows the gradient path planning.

**Gif:**<br>
<iframe src="https://cdn.discordapp.com/attachments/828395914145431612/1176648889944772628/gif_gpp.gif?ex=656fa2ac&is=655d2dac&hm=9d87604ac2841580af16a191a7cf1906e06235430e5a680090792ee8b1b89e02&" width="480" height="259" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/rm_p4.gif"></a></p>

**Simulation Video**

This video shows the simulation of the global navigation with gradient path planning. 
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/8UtVWodl5YI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
 
