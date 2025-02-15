# Unsupervised learning to predict if cryptocurrencies are affected by 24-hour or 7-day price changes

## Authors
- Jim Cockerham, 2025
[@jimmycII](https://github.com/jimmycII)


## Development Steps Completed:

- Data Loading and Exploration:

    - Loaded the cryptocurrency market data from a CSV file into a Pandas DataFrame, using "coin_id" as the index.

    - Displayed sample data, summary statistics, and a line plot of the data to understand its characteristics.

- Data Preprocessing:

    - Handled missing and infinite values by replacing them with 0. Critical Step: Failing to do this prevents the script from executing.

    - Scaled the numerical data using StandardScaler to ensure that all features contribute equally to the distance calculations in K-means.

    - Created a new DataFrame (scaled_df) with the scaled data, preserving the original index.

    -  Determining the Optimal Number of Clusters (k) - Initial:

    - Implemented the elbow method to find the best value for k (the number of clusters).
    - Calculated the inertia for k values ranging from 1 to 11.

    - Plotted the elbow curve (inertia vs. k) using hvplot.

    - Determined that k=4 is the best value based on the elbow curve.

    - K-Means Clustering (Initial - Scaled Data):

    - Initialized a KMeans model with n_clusters=4.

    - Fit the KMeans model to the scaled data (scaled_df).

    - Predicted the cluster assignments for each cryptocurrency.

    - Created a copy of the scaled DataFrame (clustered_df) and added a new column named "cluster" to store the cluster assignments.

- Visualization (Initial - Scaled Data):

    - Created a scatter plot using hvplot to visualize the clusters, with "price_change_percentage_24h" on the x-axis and "price_change_percentage_7d" on the y-axis. The points were colored according to their cluster assignments, and the "coin_id" was added to the hover information for identification.

    - Observation: Some points that appeared visually close in the 2D projection were assigned to different clusters, suggesting that other features were influencing the clustering.

    - Principal Component Analysis (PCA):

    - PCA Overview: PCA is a dimensionality reduction technique used to transform a dataset into a new set of uncorrelated variables called principal components. These components capture the most significant variance in the original data, allowing us to reduce the number of features while retaining essential information.

    - Created a PCA model instance with n_components=3.

    - Used the PCA model with fit_transform to reduce the original scaled DataFrame down to three principal components.

    - Retrieved the explained variance ratio to determine how much information can be attributed to each principal component. The three components explain ~89.5% of the variance.

    - Determining the Optimal Number of Clusters (k) - PCA Data:

    - Repeated the elbow method analysis using the PCA-transformed data.

    - The best value for k remained at 4.

    - K-Means Clustering (PCA-Transformed Data):

    - Initialized a new KMeans model with n_clusters=4.

    - Fit the KMeans model to the PCA-transformed data (pca_df).

    - Predicted the cluster assignments for each cryptocurrency.

    - Created a copy of the PCA DataFrame (clustered_pca_df) and added a new column named "cluster" to store the cluster assignments.

- Visualization (PCA-Transformed Data):

    - Created a scatter plot using hvplot to visualize the clusters, with "PC1" on the x-axis and "PC2" on the y-axis. The points were colored according to their cluster assignments, and the "coin_id" was added to the hover information for identification.

- Comparison and Analysis:

    - Created composite plots to compare the elbow curves and cluster visualizations from the original scaled data and the PCA-transformed data.

    - Visually analyzed the impact of using fewer features on the cluster analysis results.

## Observations and Limitations:

- Cluster Proximity Discrepancy (Initial Clustering): Points that appear visually close to each other on the 2D scatter plot (based on "price_change_percentage_24h" and "price_change_percentage_7d") were sometimes assigned to different clusters due to K-Means taking all dimensions into account.

- K-Means Sensitivity:

    - Scaling: K-means relies on calculating distances between data points. The choice of scaling method can influence the results.

    - Initial Centroids: K-means is sensitive to the initial placement of cluster centroids. Different random initializations can lead to different cluster assignments. Increasing the n_init parameter mitigates this but doesn't eliminate the issue.

- High Dimensionality (Initial Clustering): The data is plotted in only two dimensions, whereas the clusters are calculated in the full feature space.

- K-Means Assumptions: K-means assumes clusters are somewhat spherical, equally sized, and have similar densities. If these assumptions are violated, the results may be suboptimal.

- Impact of PCA: Using fewer features (as achieved through PCA) in K-Means clustering simplifies the data representation, potentially leading to the formation of more distinct clusters (as visually observed in the PCA scatter plot). However, this simplification comes at a cost:

    - Because PCA reduces the dimensions of the feature space, the algorithm has less information about the points being clustered.

    - If important dimensions are dropped by PCA, then the generated clusters may be less accurate in terms of fully representing the underlying relationships in the original data.

## Final Conclusion:

- The use of PCA improved the visual distinctness of the clusters, likely by focusing on the dimensions with the highest variance. However, it's crucial to acknowledge that this dimensionality reduction inherently discards some information. The optimal approach depends on the specific goals of the analysis. If the goal is to create visually separated clusters for a presentation, then PCA is good. If the goal is to accurately label the data based on all factors, then this might not be the best choice.