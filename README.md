
# Hunting Store Site Selection

In this project, we will use deer harvest data to select the ideal locations for 6 new branches of a hunting apparel store (k-means). We are assuming that the best locations for these new stores are in the zip codes where people are harvesting the most deer.

## Authors

- [@PhilipHarvey20](https://github.com/PhilipHarvey20)


## Demo


https://github.com/PhilipHarvey20/Network-Optimization---K-Means/blob/master/K-Means.ipynb



### *** STEP 1 - Import zip code data 


First, we will want to import four csv files that contain all of the zip codes in AL, MS, GA, and SC. In addition we will need to import a lookup file to pull in the LAT/LON details for each of these zip codes (link below)

``` us_coordinate_lookup = pd.read_csv('https://raw.githubusercontent.com/PhilipHarvey20/Network-Optimization---K-Means/master/USZIPCodes202204.csv') ```

### *** STEP 2 - set num of clusters (stores) 
We know that we want to open 6 new stores in the Southeast, so we will set the number of centroids (K) to 6

``` no_of_clusters = 6 ```

### *** STEP 3 - randomly generate harvest data 
Harvest data is not currently available at the zip code level for each state, however, let's assume that every hunter from 2018 that purchased a hunting license was able to harvest one deer each. With that assumption, we can use the 2018 hunting license purchase data below as a representation of the number of harvested deer for AL, GA, MS, SC.

- AL - 548k
- GA - 652k
- MS - 300k
- SC - 181k

Next, let's randomly generate harvest counts for each zip code in the four states making sure that the total harvest count is equal to the number of hunters that purchased a license in a given state.   


``` AL_ZIP_CODES['harvest_count'] = np.around(np.random.dirichlet(np.ones(AL_ZIP_CODES.shape[0]),size=10)[0], decimals = 15) ```

``` AL_ZIP_CODES['harvest_count'] = AL_ZIP_CODES['harvest_count'] * 388000 ```


We will also want to create a few "high-density" counties where more deer were harvested. Based on feedback from hunters, I have identified 2-4 popular hunting counties per state (AL shown below). We will want to generate a larger number of harvest counts for these "high-density" counties.

```AL_HIGH_DENSITY_COUNTIES = ['TUSCALOOSA','BALDWIN','BARBOUR','CHEROKEE']```

### *** STEP 4 - Plot deer harvest data 


![App Screenshot](https://github.com/PhilipHarvey20/Network-Optimization---K-Means/blob/master/2018_Deer_Harvest_Data_Plotted.jpg?raw=true "Optional Title")

### *** STEP 5 - K Means (Clustering Algorithm)

Next, we will use ```Kmeans``` from the Scikit-learn library to identify the optimal locations for our 6 stores. We have already set the number of centroids (K) to 6 and will set the num of iterations to 1000. We will also want to use the ```harvest_count``` field as a weight to skew the centroids towards the zip codes with a greater count of harvested deer.

``` kmeans = KMeans(n_clusters=(no_of_clusters), random_state=0, max_iter=1000)```

```X = np.array(df.drop(['zip', 'Zipcode name', 'City', 'State', 'County Name','harvest_count'], 1).astype(float))```

```Y = np.array(df['harvest_count'].astype(float))```


### *** STEP 6 - Plot results

After convergence, let's plot the results on the same map using ```plotly``` The red Xs will represent the optimal locations for our hunting stores  and the green lines assign each hunter to his or her closest store.


![App Screenshot](https://github.com/PhilipHarvey20/Network-Optimization---K-Means/blob/master/Store_Locations_Plotted.jpg?raw=true "Optional Title")
