# Clustering-using-Density-Based-Scan

Data records are classified based on the euclidean distance between them and clustered using DBScan to form clusters (objects of similar attributes).


 DBSCAN Approach: 

1.	Distance matrix is created with Euclidean distances of every pair of points in the input 
2.	Considered a point in the input file and calculated it neighbors by taking those points which were within a distance of ‘radius’ with the considered point. 
3.	By checking the length of the neighbors obtained from step# 2, the considered point is subjected to classification into core, border or noise points 
4.	If the length of neighbors is less than min_points (density), then the considered point is classified as a noise point 
5.	If the length of neighbors is greater than min_points(density), then the considered point is classified as a core point 
6.	The core point is added to a new cluster (say 1) 
7.	Now, the neighbors of each of the points in the list of neighbors of the obtained core point is calculated by following step#3 
8.	Again, each of the points in the list of neighbors are classified into core or border points based on the number of its neighbors being lesser or greater than min_points(density) 
9.	If a point in the list of neighbors has neighbors greater than min_points, it is classified as a core point and added to the cluster of the initial core point, to which it was neighbor 
10.	If a point in the list of neighbors has neighbors lesser than min_points, it is classified as a border point since, the point is within a ‘radius’ distance of a core point to which it was a neighbor 
11.	The obtained border point is then added to the same cluster as its core point 
12.	The process is continued till the edge of a cluster is reached. 

Approach of the Implementation: 

1. Build a CSR matrix to make use of sparsity of the input data in train.dat 
    1.	Formed a matrix of shape (length of rows * number of total unique features in input) 
    2.	Created val, ind and ptr to build the CSR matrix to read the data in train.dat 

2. Normalize the values of features in each point to avoid the impact of larger values when 
calculating Euclidean distance between the points 
    1.	Using L2 normalization, the effect of larger values of features are reduced 

3. Using truncated SVD, I limited the features of the input points to 150 
4. Created a distance matrix, distance_matrix (8580 * 8580) to have the euclidean distance 
between each point and every other point in the data 

Euclidean Distance is calculated as below for every pair of points in input data 

            def calc_dist(r1, r2): 
            d = 0 
            for val in range(len(r1)): 
            d += np.power((r1[val] - r2[val]), 2) 
            d = math.sqrt(d) 
        where r1 and r2 are any pair of points 
        
5. Implemented DBSCAN algorithm to cluster the points based on radius and min_points values 
6. The same procedure is done using values of min_points from 3 to 21, to identify the clusters 
7. Printed the clusters to which each row in the input file belonged and submitted to CLP to obtain the NMI Score
