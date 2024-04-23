# Computational geometry

- Lines in 3d 
    - https://math.libretexts.org/Bookshelves/Calculus/CLP-3_Multivariable_Calculus_(Feldman_Rechnitzer_and_Yeager)/01%3A_Vectors_and_Geometry_in_Two_and_Three_Dimensions/1.05%3A_Equations_of_Lines_in_3d
    - https://mathworld.wolfram.com/Point-LineDistance3-Dimensional.html
    - https://stackoverflow.com/questions/4858264/find-the-distance-from-a-3d-point-to-a-line-segment
    
- Clipping algorithm
    - https://en.wikipedia.org/wiki/Greiner%E2%80%93Hormann_clipping_algorithm
    - https://davis.wpi.edu/~matt/courses/clipping/

- Point in polygon
    - https://en.wikipedia.org/wiki/Point_in_polygon#Winding_number_algorithm

- transformation matrix to flip y-up gltf points to z-up
    ```
    mat = np.array([[1, 0, 0, 0],
                    [0, 0, -1, 0],
                    [0, 1, 0, 0],
                    [0, 0, 0, 1]])
    ```
- point plane distance: https://mathworld.wolfram.com/Point-PlaneDistance.html
- closest point to triangle: https://gdbooks.gitbooks.io/3dcollisions/content/Chapter4/closest_point_to_triangle.html
    - this algorithm will probably work for polygon too
- Gilbert–Johnson–Keerthi: https://stackoverflow.com/questions/2433298/distance-from-a-point-to-a-polyhedron-or-to-a-polygon
- point in a convex solid: https://stackoverflow.com/questions/34379859/check-if-a-3d-point-lies-inside-a-3d-platonic-solid
- boolean two polyhedron: https://liris.cnrs.fr/Documents/Liris-4883.pdf