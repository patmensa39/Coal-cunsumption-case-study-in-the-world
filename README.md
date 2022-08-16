# Coal-cunsumption-case-study-in-the-world

# Coal Consumption Case Study in the world

# Load the tidyverse and the dataset 
library(tidyverse)

# Read in the coal dataset
coal <- read_csv("http://594442.youcanlearnit.net/coal.csv")
glimpse(coal)

# Skipping  the first two lines
coal <- read_csv("http://594442.youcanlearnit.net/coal.csv", skip=2)
glimpse (coal)

# Renaming  the first column as region
colnames(coal)[1] <- "region"
summary(coal)

# Convert from a wide dataset to a long dataset using pivot_longer
coal_long <- pivot_longer(coal, !region, names_to='year', values_to='coal_consumption')
glimpse(coal_long)

# Convert years to integers and coal consumption to numeric
coal_long <- coal_long %>%
  mutate(year=as.integer(year),
         coal_consumption = as.numeric(coal_consumption))
summary(coal_long)

# Look at region values - they contain both continents and countries
unique(coal_long$region)

# Creating a vector of "noncountry" values that appear in the region variable
noncountries <- c("North America", "Central & South America", "Antarctica", "Europe", "Eurasia", 
                  "Middle East", "Africa", "Asia & Oceania", "World")

# creating  a tibble of regional values
coal_region <- coal_long %>% filter(region %in% noncountries)

# creating a tibble of country values
coal_country <- coal_long %>% filter(!(region %in% noncountries))

# check them out
unique(coal_region$region)
unique(coal_country$region)

# Start with a scatterplot
ggplot(data=coal_region, mapping=aes(x=year, y=coal_consumption)) +
  geom_point()

# That looks like it should be a line graph instead
ggplot(data=coal_region, mapping=aes(x=year, y=coal_consumption)) +
  geom_line()

# Geting a separate line for each  each region
ggplot(data=coal_region, mapping=aes(x=year, y=coal_consumption)) +
  geom_line(mapping=aes(color=region))

