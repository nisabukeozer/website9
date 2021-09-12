---
categories:
- ""
- ""
date: "2017-10-31T21:28:43-05:00"
description: "Projects that I worked on"
draft: false
image: pic10.jpg
keywords: ""
slug: projects
title: Projects
---

Projects that I worked on

```{r load-libraries, echo=FALSE, message=FALSE, warning=FALSE}
library(tidyverse)  # Load ggplot2, dplyr, and all the other tidyverse packages
library(gapminder)  # gapminder dataset
library(here)
library(janitor)
```


# Task 2: `gapminder` country comparison

You have seen the `gapminder` dataset that has data on life expectancy, population, and GDP per capita for 142 countries from 1952 to 2007. To get a glimpse of the dataframe, namely to see the variable names, variable types, etc., we use the `glimpse` function. We also want to have a look at the first 20 rows of data.

```{r}
glimpse(gapminder)

head(gapminder, 20) # look at the first 20 rows of the dataframe

```

Your task is to produce two graphs of how life expectancy has changed over the years for the `country` and the `continent` you come from.

I have created the `country_data` and `continent_data` with the code below.

```{r}
country_data <- gapminder %>% 
            filter(country == "Turkey") 

head(country_data)
continent_data <- gapminder %>% 
            filter(continent == "Europe")

head(continent_data)
```

First, create a plot of life expectancy over time for the single country you chose. Map `year` on the x-axis, and `lifeExp` on the y-axis. You should also use `geom_point()` to see the actual data points and `geom_smooth(se = FALSE)` to plot the underlying trendlines. You need to remove the comments **\#** from the lines below for your code to run.


```{r, lifeExp_one_country}
plot1 <- ggplot(data = country_data , mapping = aes(x = year, y = lifeExp))+
   geom_point() +
   geom_smooth(se = FALSE)+
   NULL 

plot1
```

Next we need to add a title. Create a new plot, or extend plot1, using the `labs()` function to add an informative title to the plot.

```{r, lifeExp_one_country_with_label}
 plot1<- plot1 +
   labs(title = " Life Expactancy Over Time in Turkey ",
       x = " Year ",
       y = " Life Expectancy ") +
   NULL


 plot1
```

Secondly, produce a plot for all countries in the *continent* you come from. (Hint: map the `country` variable to the colour aesthetic. You also want to map `country` to the `group` aesthetic, so all points for each country are grouped together).

```{r lifeExp_one_continent}
 ggplot(continent_data, mapping = aes(x = year , y = lifeExp , colour = country, group = country)) +
   geom_point() + 
   geom_smooth(se = FALSE) +
   NULL
```


Finally, using the original `gapminder` data, produce a life expectancy over time graph, grouped (or faceted) by continent. We will remove all legends, adding the `theme(legend.position="none")` in the end of our ggplot.

```{r lifeExp_facet_by_continent}
 ggplot(gapminder , mapping = aes(x = year , y = lifeExp, colour = continent))+
   geom_point() +
   geom_smooth(se = FALSE) +
   facet_wrap(~continent) +
   theme(legend.position="none") + #remove all legends
   NULL
```

Given these trends, what can you say about life expectancy since 1952? Again, don't just say what's happening in the graph. Tell some sort of story and speculate about the differences in the patterns.

> Type your answer after this blockquote.

## A positive life expectency trend is evident in all continents

First of all, generally speaking, for every, the life expectancy over years has been demonstrating a **positive trend**. This positive trend in life expectancy can be tied to the developments in medicine, biomedical engineering and technology in the world.

## Africa's stagnation after the 1990s

Africa's graph is especially interesting since the life expectancy starts of with an average of 40 years of age, increases to 50 and shows a stagnation after 1990. Further research can be conducted to understand what happened in 1990, or how the **resources** have been negatively affected. As far as I know and after doing a very little confirmation research, Europe and Americas had been contributing to Africa's well being by performing concerts and help packages before the 1990s and they had stopped doing so after the 90s. This might be one of the reasons and should be further investigated. 

## Increase in longevity from 1950s to 2007  

**Oceania** is the continent with the highest longevity. Oceania starts off well with an average of 70 years of age and its average longevity is almost 80 or a bit more. Since there are not many countries in Oceania, its graph is fairly uniform and does not consist of many different data points. Europe is following Ocenia in longevity, starting off with 65 and increasing up to 80 years of age. Americas is the third in the longevity trend ranking, followed by Asia and Africa. The reason for the difference among the longevity between continents might be related to the overall **GDP** for each continent as well as the **advances in medicine and technology**. 

The average approximated increase in each continent from 1950 to 2007 is as follows:

* Africa: 40 to 54  -> increase by ~14
* Americas: 54 to 74 -> increase by ~20
* Asia 47 to 70 -> increase by ~23
* Europe 63 to 77 -> increased by ~14
* Oceania: 70 to 80 -> increased by ~10


Approximately, **Asia and Americas** are the continents that showed **the most significant increase in longevity** from the 1950s to 2007 compared to the other continents. This makes sense since the world have been witnessing **a technology and production boom** from the Americas and Asia for a very long time. 

## About the variations in each continent

Europe's dataset is the one that caught my eye initially. There is not a very significant variance among the different European countries. **But** there seems to be an outlier country in the graph, and not surprisingly it is my country **Turkey**. It has been always debated whether Turkey belongs to Europe or Asia. Since this data set has been relatively not so new, it is recorded in Europe, as it was the case during my childhood before all the political unrest that started with the new government: aka *Geopolitics*. Since Turkey's resources and the GDP are not as high as the other European countries, Turkey's data points stick out. If the data recorders had kept recording data after the 2010s, Turkey would have been probably moved to Asia. And it might fit in well with the high variance of longevity in Asian countries. -Or we should all give up and create a Euroasia factor in the continent feature and add Turkey and Russia there. :-) -

Asia shows the highest variance among all the continents, since the **GDP** is not as uniform throughout the Asian countries unlike the European countries. Americans and Africas also show a high variance since there are many different countries with varying GDPs and technologies advancements in Africa, Americas and Asia. 


# Task 3: Brexit vote analysis

We will have a look at the results of the 2016 Brexit vote in the UK. First we read the data using `read_csv()` and have a quick glimpse at the data

```{r load_brexit_data, warning=FALSE, message=FALSE}
brexit_results <- read_csv(here::here("data","brexit_results.csv"))

glimpse(brexit_results)
```

The data comes from [Elliott Morris](https://www.thecrosstab.com/), who cleaned it and made it available through his [DataCamp class on analysing election and polling data in R](https://www.datacamp.com/courses/analyzing-election-and-polling-data-in-r).

Our main outcome variable (or y) is `leave_share`, which is the percent of votes cast in favour of Brexit, or leaving the EU. Each row is a UK [parliament constituency](https://en.wikipedia.org/wiki/United_Kingdom_Parliament_constituencies).

To get a sense of the spread, or distribution, of the data, we can plot a histogram, a density plot, and the empirical cumulative distribution function of the leave % in all constituencies.

```{r brexit_histogram, warning=FALSE, message=FALSE}

# histogram
histogram <- ggplot(brexit_results, aes(x = leave_share)) +
  geom_histogram(binwidth = 2.5) 

histogram2<- histogram +
   labs(title = "Histogram Plot of Leave Share",
        subtitle = "Leave Share: Percent of votes in cast in favour of Brexit",
       x = "Leave Share",
       y = "Count ") +
   NULL
histogram2
  
# density plot-- think smoothed histogram
density_plot <- ggplot(brexit_results, aes(x = leave_share)) +
  geom_density() 

density_plot2 <- density_plot +
   labs(title = "Cumulative Density Distribution Plot of Leave Share",
        subtitle = "Leave Share: Percent of votes in cast in favour of Brexit", 
       x = "Leave Share ",
       y = "Density ") +
   NULL
density_plot2

# The empirical cumulative distribution function (ECDF) 
ecdf <- ggplot(brexit_results, aes(x = leave_share)) +
  stat_ecdf(geom = "step", pad = FALSE) +
  scale_y_continuous(labels = scales::percent)

ecdf2 <- ecdf + 
  labs(title= "Empirical Cumulative Distribution Function of Leave Share",
       x = "Leave Share: Percent of votes in cast in favour of Brexit",
       y= "Cumulative Probability ") +
  NULL
ecdf2 


  
```

One common explanation for the Brexit outcome was fear of immigration and opposition to the EU's more open border policy. We can check the relationship (or correlation) between the proportion of native born residents (`born_in_uk`) in a constituency and its `leave_share`. To do this, let us get the correlation between the two variables

```{r brexit_immigration_correlation}
brexit_results %>% 
  select(leave_share, born_in_uk) %>% 
  cor() 
```

The correlation is almost 0.5, which shows that the two variables are positively correlated.

We can also create a scatterplot between these two variables using `geom_point`. We also add the best fit line, using `geom_smooth(method = "lm")`.

```{r brexit_immigration_plot}
immigrationPlot <- ggplot(brexit_results, aes(x = born_in_uk, y = leave_share)) +
  geom_point(alpha=0.3) +
  
  # add a smoothing line, and use method="lm" to get the best straight-line
  geom_smooth(method = "lm") + 
  
  # use a white background and frame the plot with a black box
  theme_bw() +

  NULL

immigrationPlot2 <- immigrationPlot +
  labs(title="Scatter Plot of Leave Share vs Born UK",
       subtitle= "Born UK: Percentage of voters that were born in the UK",
       x = "Leave Share ",
       y ="Born in UK ")

immigrationPlot2
```


You have the code for the plots, I would like you to revisit all of them and use the `labs()` function to add an informative title, subtitle, and axes titles to all plots.

What can you say about the relationship shown above? Again, don't just say what's happening in the graph. Tell some sort of story and speculate about the differences in the patterns.
 > Type your answer after, and outside, this blockquote.

First of all, the histogram of the leave share is slighlt left skewed, which means that voters more likely to vote in favour of leaving the EU. 

The 0.5 correlation between the leave share and born in the UK, shows that there is a  positive correlation between the percentage of pro-Brexit voters and percentage of UK born voters. According to the scatter plot of Leave Share vs Born UK, we also can see that there is an obvious positive trend between Born in UK and Leave Share. This makes sense since, people who do not support immigration are more likely to be UK born and bred people rather than immigrants. 

The upper left side of the graph is almost empty. This emptiness is kind of expected since that area represents UK born and bred majority whose leave share is relatively low. While the upper right area is fully crowded, in those area the born in the UK percentage is relatively higher while the leave share is also higher, which represents the general trend in the country.   


# Task 4: Animal rescue incidents attended by the London Fire Brigade

[The London Fire Brigade](https://data.london.gov.uk/dataset/animal-rescue-incidents-attended-by-lfb) attends a range of non-fire incidents (which we call 'special services'). These 'special services' include assistance to animals that may be trapped or in distress. The data is provided from January 2009 and is updated monthly. A range of information is supplied for each incident including some location information (postcode, borough, ward), as well as the data/time of the incidents. We do not routinely record data about animal deaths or injuries.

Please note that any cost included is a notional cost calculated based on the length of time rounded up to the nearest hour spent by Pump, Aerial and FRU appliances at the incident and charged at the current Brigade hourly rate.
```{r load_animal_rescue_data, warning=FALSE, message=FALSE}

url <- "https://data.london.gov.uk/download/animal-rescue-incidents-attended-by-lfb/8a7d91c2-9aec-4bde-937a-3998f4717cd8/Animal%20Rescue%20incidents%20attended%20by%20LFB%20from%20Jan%202009.csv"

animal_rescue <- read_csv(url,
                          locale = locale(encoding = "CP1252")) %>% 
  janitor::clean_names()


glimpse(animal_rescue)
```

One of the more useful things one can do with any data set is quick counts, namely to see how many observations fall within one category. For instance, if we wanted to count the number of incidents by year, we would either use `group_by()... summarise()` or, simply [`count()`](https://dplyr.tidyverse.org/reference/count.html)

```{r, instances_by_calendar_year}

animal_rescue %>% 
  dplyr::group_by(cal_year) %>% 
  summarise(count=n())

animal_rescue %>% 
  count(cal_year, name="count")

```

Let us try to see how many incidents we have by animal group. Again, we can do this either using group_by() and summarise(), or by using count()

```{r, animal_group_percentages}
animal_rescue %>% 
  group_by(animal_group_parent) %>% 
  
  #group_by and summarise will produce a new column with the count in each animal group
  summarise(count = n()) %>% 
  
  # mutate adds a new column; here we calculate the percentage
  mutate(percent = round(100*count/sum(count),2)) %>% 
  
  # arrange() sorts the data by percent. Since the default sorting is min to max and we would like to see it sorted
  # in descending order (max to min), we use arrange(desc()) 
  arrange(desc(percent))


animal_rescue %>% 
  
  #count does the same thing as group_by and summarise
  # name = "count" will call the column with the counts "count" ( exciting, I know)
  # and 'sort=TRUE' will sort them from max to min
  count(animal_group_parent, name="count", sort=TRUE) %>% 
  mutate(percent = round(100*count/sum(count),2))


```

Do you see anything strange in these tables? 

Finally, let us have a look at the notional cost for rescuing each of these animals. As the LFB says,

> Please note that any cost included is a notional cost calculated based on the length of time rounded up to the nearest hour spent by Pump, Aerial and FRU appliances at the incident and charged at the current Brigade hourly rate.

There is two things we will do:

1. Calculate the mean and median `incident_notional_cost` for each `animal_group_parent`
2. Plot a boxplot to get a feel for the distribution of `incident_notional_cost` by `animal_group_parent`.


Before we go on, however, we need to fix `incident_notional_cost` as it is stored as a `chr`, or character, rather than a number.

```{r, parse_incident_cost,message=FALSE, warning=FALSE}

# what type is variable incident_notional_cost from dataframe `animal_rescue`
typeof(animal_rescue$incident_notional_cost)

# readr::parse_number() will convert any numerical values stored as characters into numbers
animal_rescue <- animal_rescue %>% 

  # we use mutate() to use the parse_number() function and overwrite the same variable
  mutate(incident_notional_cost = parse_number(incident_notional_cost))

# incident_notional_cost from dataframe `animal_rescue` is now 'double' or numeric
typeof(animal_rescue$incident_notional_cost)

```
Now that incident_notional_cost is numeric, let us quickly calculate summary statistics for each animal group. 


```{r, stats_on_incident_cost,message=FALSE, warning=FALSE}

animal_rescue %>% 
  
  # group by animal_group_parent
  group_by(animal_group_parent) %>% 
  
  # filter resulting data, so each group has at least 6 observations
  filter(n()>6) %>% 
  
  # summarise() will collapse all values into 3 values: the mean, median, and count  
  # we use na.rm=TRUE to make sure we remove any NAs, or cases where we do not have the incident cos
  summarise(mean_incident_cost = mean (incident_notional_cost, na.rm=TRUE),
            median_incident_cost = median (incident_notional_cost, na.rm=TRUE),
            sd_incident_cost = sd (incident_notional_cost, na.rm=TRUE),
            min_incident_cost = min (incident_notional_cost, na.rm=TRUE),
            max_incident_cost = max (incident_notional_cost, na.rm=TRUE),
            count = n()) %>% 
  
  # sort the resulting data in descending order. You choose whether to sort by count or mean cost.
  arrange(desc(mean_incident_cost))

```

Compare the mean and the median for each animal group. what do you think this is telling us?
Anything else that stands out? Any outliers?

The difference between mean and median can help us infer whether the data points have extreme values. If a data set has a couple of very extreme values on the positive cost side, its mean will be more than its median. The more the difference between mean and median, the more extreme values a data set has on one side of the spectrum. 
For this data set, this information can be used to detect if an animal requires very costly expenditures. The more the difference between the mean and the median is, the more the animal may have extreme data points in its cost data set. 

From this table only, i cannot seem to detect outliers since the data is arranged in descending order and the values are descending in harmony. 


Finally, let us plot a few plots that show the distribution of incident_cost for each animal group.

```{r, plots_on_incident_cost_by_animal_group,message=FALSE, warning=FALSE}

# base_plot
base_plot <- animal_rescue %>% 
  group_by(animal_group_parent) %>% 
  filter(n()>6) %>% 
  ggplot(aes(x=incident_notional_cost))+
  facet_wrap(~animal_group_parent, scales = "free")+
  theme_bw()

base_plot + geom_histogram()
base_plot + geom_density()
base_plot + geom_boxplot()
base_plot + stat_ecdf(geom = "step", pad = FALSE) +
  scale_y_continuous(labels = scales::percent)



```

Which of these four graphs do you think best communicates the variability of the `incident_notional_cost` values? Also, can you please tell some sort of story (which animals are more expensive to rescue than others, the spread of values) and speculate about the differences in the patterns.


All of the four graphs are connected to one another. But especially histogram, density plot's and box plot's shapes can be roughly deduced from one another.

In my humble opinion, **box plot** might be the best option to infer variability. Since the interquartile ranges include data points between an upper quartile and lower quartile that were calculated by the variance, it. We can deduce how the data points are distributed more clearly since we know that 50% of the data lies in the intequartile range. We can also see the median which is the line between the upper and lower quartile. Outliers are more visible in box plots as well. 

But I'd look at all the graphs to infer insights about variance. And it is always the easiest to do a data analysis on an histogram since it has a straightforward  approach and tells you how many data points each bin has. 

It is important to note that not every animal has the same **amount of data points.** The cats are by far the most rescued animals, followed by birds and dogs. These three are the famous pets that everyone loves and this might be the reason they are being consistently reported by the owners to the rescue team. In the meanwhile cows seem to have the least data points. Although it is costly to rescue cows, they are not being rescued very frequently. 

**Bigger animals like horses, cows and deers** seem to require more money to be rescued compared to smaller animals like ferret and rabbits. Bigger bodies may require more advanced equipment and manpower to be rescued and this may result in more expenditure. Horse can require especially high costs and it is the only animal that have more than a coupple of points after 2000. 

**Ferret and rabit** have very similar notional costs and hence the variability in costs for these two animals is relatively low as it is apparent in the graphs.
The data points are close to each other and they show almost like a uniform distribution pattern.

**Fox, bird, dog, deer and cat** also show similar patterns in terms of variability. Their interquartile ranges are small and they show a spike in the beginning at around 500. Their costs tend to stay under 500 for most of the cases. But they all have extreme values that exceed 1500. The similarity might be attricuted to a cat, a bird and adog being pets and needing similar procedures to be rescued. Fox is not a pet yet its nature is close to a mix of cat and a dog. Unknown wild animal's average cost and variability are similar to bird, dog and dear and are almost same the as the fox, which cannot be explained without further research. But it is for sure that they might carry similar traits with these animals. 

**Hamster and squirrel** show very similar patterns in terms of cost and variability as well. Their costs both fluctate around 300 in a very similar fashion with similar variance. They both have outliers around 600 as well. This makes sense since hamster and squirrel have also similar physical attributes. 






