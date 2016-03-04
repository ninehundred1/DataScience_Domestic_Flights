# Minimize your domestic airport delays, version January 2015



These are the things we will look at, and the steps to do that:

1. Let's peek at the different aspects of the data

    - 1A. Parse airport data from January 2015 into a MongoDB database & add additional files
    - 1B. Plot a scatter matrix of all variables for a single day

2. Where are the most flights and delays, in the West or the East of the USA?

    - 2A. Query mongoDB by coordinates larger or smaller than the center of USA longitude
    - 2B. Create distributions and plot a bar plots of the mean departures and arrival for each day in one month (West vs East)
    - 2C. Plot distributions of the delays for West and East USA for each day of one month.
    
3.  Which airline should I choose to quickly get out of the major aiports?
    
    - 3A. Query mongoDB and load into Pandas each airline and its taxi out and carrier delay times
    - 3B. Group the data into airlines within Pandas
    - 3C. Plot the worst 10 airlines in terms of carrier delays and taxi times
    
4.  Where in the USA are the worst weather delays that affect my flights?

    - 4A. query mongoDB for weather delays and corresponding coordinates for each departure.
    - 4B. plot on map each incident of weather delay with the size of the cirlce matching delay time
    - 4C. plot a density hex map with the color matching the number of delays per hex bin area
    
5. What would be an ideal route if I want to collect my own data visiting every single airport?

    - 5A. Query mongoDB for the airport coordinates and add all airport distances into a set
    - 5B. Get each airport once and calculate all possible combinations and distances
    - 5C. Use a genetic algorithm to calculate and optimize a path with the minimal distance between all airports
    
6. Predict future delays by finding classifiers for a given a day in January, city of origin and the length of the flight using Random Forests
    - 6A. Query mongoDB for the airport coordinates, day of month, trip lenght and delays
    - 6B. Find classifiers using training data
    - 6C. Measure prediction performance using test data


