# Minimize your domestic airport delays, version January 2015.
ipython notebook link
http://www.googledrive.com/host/0B9W7L1j7MW21ZFd3NkZpeUYzTGM



## 1. Intro

Here a few things I decided to look at using the [publicly available dataset of domestic flight in the US.](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time "Datalink") 


I focused on the month of January 2015, as with the seasonal snow weather there are more interesting things happening at airports. 

I focused on the delays, as that was one thing that was not correlated, but varying with airport, airline and day, which also gives the opportunity (as need) for some prediction models.

I used python, MongoDB as a database, Pandas as a dataframe and some other libraries for plotting, maths and Random Forests.

I used three other files for locations and Airline names. For the prediction I could have used more data (such as the weather records), which I might add later.


## 2. Results
**Here the steps:**

#### 1.	Let's peek at the different aspects of the data**

**•	1A. Parse airport data from January 2015 into a MongoDB database & add additional files**

**•	1B. Plot a scatter matrix of all variables for a single day**

Below is a scatter matrix of all the data I found useful to plot. A couple of things to note by going through each column and looking for something interesting:



1. Departure Times:
There are no flights leaving before 5am. There is no difference in when in the day short or long distance flights depart (compare with DISTANCE).  There seem to be an increase in departure delays as the day gets later (compare with DEP_DELAY).

2. Departure Delay:
Departure delay seem to be shorter with longer distance flights (compare with DISTANCE). That might be because the tower might give preference to larger planes/longer distances or it might also be the fact that people might check in more luggage and therefore earlier at the airport, compared to business flight who basically walk from the taxi into the plane, where there is less time to make up for passenger delay. 

3. Arrival Delay:
Arrival Delay seems to nice correlate with departure delay (0.934), which is at one hand understandable, as if the plane leaves later, it arrives later. Still, on longer distances that might not be the case as planes tend to catch up to still arrive on time.
Most other things (Weather Delays,  Taxi times, etc) seem to be more or less uncorrelated within this global correlation. 

![Fig1](http://i.imgur.com/pph3npI.jpg)


#### 2.	Where are the most flights and delays, in the West or the East of the USA?

**•	2A. Query mongoDB by coordinates larger or smaller than the center of USA longitude**

**•	2B. Create distributions and plot a bar plots of the mean departures and arrival for each day in one month (West vs East)**

**•	2C. Plot distributions of the delays for West and East USA for each day of one month**

Next we make use of the geographical locations we added to compare the differences in departures between the West and the East of the USA. We split the country in its true center and divided all flight by departure between that central longitude.

You can see that a lot departures  fall into the week, with a notable dip on all Saturdays. January 1st being a holiday also led to less flight departing.  The first saturday (3rd) in the year sees an above average departure rate as travelers return from the holiday before the Monday. 


![Fig2](http://i.imgur.com/imf2MOk.png)

In terms of delays, there we notably more delays in the early days of January with median delays on the order of more than 10 minutes. It was comparable for West and East USA and there was no snow storm apparent, therefore likely the delays came from understaffing after the ho lidays as well increased travel.


![Fig3](http://i.imgur.com/5mC4lap.jpg)




#### 3.	Which airline should I choose to quickly get out of the major airports?

**•	3A. Query mongoDB and load into Pandas each airline and its taxi out and carrier delay times**

**•	3B. Group the data into airlines within Pandas**

**•	3C. Plot the worst 10 airlines in terms of carrier delays and taxi times**

Next we group the Carrier delays (not including weather) to see what airlines are doing badly in terms of having delays. We focus on three major airports (Atlanta, LA, Chicago, Dallas and NYC).
We can see that certain airlines appear to do worse, but with the sizes of the error, that cannot really be concluded (see Hawaiian Airlines in JFK). Likely one major delay causes this distribution.


![Fig3](http://i.imgur.com/5seheJa.png)


Next we look at what airlines might get better treatment from the airports by looking at the time it takes them to taxi out (where there might be a lot of waiting). Cheap airlines usually negotiate special deals with fast taxi times to minimize time on the ground. From the data here,  there seems to be an even distribution (taking the errors into account). What can be seen is that taxi times on average are quicker in LA (around 15min) compared to JFK (with above 20min).

![Fig4](http://i.imgur.com/HoJlBUJ.png)



#### 4.	Where in the USA are the worst weather delays that affect my flights?
**•	4A. query mongoDB for weather delays and corresponding coordinates for each departure**

**•	4B. plot on map each incident of weather delay with the size of the circle matching delay time**

**•	4C. plot a density hex map with the color matching the number of delays per hex bin area**


Next we look at where the weather delays happened in January 2015. The map plots all delays and the size of the circle indicates the relative length of the delay. You can see that the metropolitan areas have a wide range of delays, whereas the small airport all over the country seem to have very few delays, likely because the amounts of departures are far less.


![Fig5](http://i.imgur.com/f81iQBJ.jpg)


To now look at the density distribution, most weather delays happen in the north east area of the USA (probably because freezing weather), around Chicago (winds and cold) but also in the area of LA and Southern California. 

![Fig6](http://i.imgur.com/y1R4svZ.jpg)


#### 5.	What would be an ideal route if I want to collect my own data visiting every single airport?

**•	5A. Query mongoDB for the airport coordinates and add all airport distances into a set**

**•	5B. Get each airport once and calculate all possible combinations and distances**

**•	5C. Use a genetic algorithm to calculate and optimize a path with the minimal distance between all airports**

Next we look into an ideal route to collect our own data. The idea is to take all flights, but spend the least possible time doing that. For that the use of a Genetic algorithm works well in finding a short path between all airports. The algorithm mutates (shuffles) subsections of the route and checks if that improves performance (decreases the length of the whole trip). After 10k generations a reasonable acceptable path has been found and no real decrease in total path length is achieved.


![Fig6](http://i.imgur.com/XlcQJ3m.png)


![Fig6](http://i.imgur.com/7ewC46F.png)




#### 6.	Predict future delays by finding classifiers for a given a day in January, city of origin and the length of the flight using Random Forests

**•	6A. Query mongoDB for the airport coordinates, day of month, trip lenght and delays**

**• 6B. Find classifiers using training data**

**•	6C. Measure prediction performance using test data**

To now make use of the data we have for January 2015, we want to predict the amount of delay we might experience if we take a flight that month. For that, Random Forests Classifiers are generates from the data aspects of Departure Day, Departure location and Duration of Flight. The algorithm is trained with 80% of the data and then the resulting prediction is made.
The performance of the algorithm can be seen below in the performance matrix. 

![Fig6](http://i.imgur.com/rQZjekX.jpg)


While it is possible to predict the delay for the first few minutes, mostly if the plane will arrive earlier (see the value for -1, which means 0-5 min early) with a precision of 0.61, everything else is pretty much not possible for the parameters uses (time of flight, day, length of flight). Likely more data needs to be added, especially weather data, as there is a clear dependence between bad weather and delay (as opposed to carrier delays).