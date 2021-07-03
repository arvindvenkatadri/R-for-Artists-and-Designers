---
title: "Lab 03a: Animal Word Cleaning"
subtitle: "CS631"
author: "Alison Hill"
output:
  html_document:
    theme: flatly
---



# Data
http://langcog.github.io/wordbankr/

# Packages


```r
library(tidyverse)
library(wordbankr)
library(here)
```


```r
my_sounds <- c("meow", "woof woof", "cockadoodledoo")

# first get the items in the animal category
sounds <- get_item_data(language = "English (American)", 
                         form = "WG") %>%
  filter(definition %in% my_sounds) 

# then get instrument data for those items
sounds_data <- get_instrument_data(
  language = "English (American)",
  form = "WG",
  items = sounds$item_id,
  administrations = TRUE,
  iteminfo = TRUE
  ) %>% 
  mutate(produces = value == "produces",
         understands = case_when(
           produces == TRUE | value == "understands" ~ TRUE,
           TRUE ~ FALSE
         )) %>% 
  drop_na(produces) %>% 
  rename(sound = uni_lemma)

# what proportion of kids at each age understand/produce each word?
sounds_summary <- sounds_data %>% 
  group_by(age, sound) %>%
  summarise(kids_produce = sum(produces, na.rm = TRUE),
            kids_understand = sum(understands, na.rm = TRUE),
            kids_respond = n_distinct(data_id),
            prop_produce = kids_produce / kids_respond,
            prop_understand = kids_understand / kids_respond)
```


Now let's export both data frames for the lab.


```r
write_csv(sounds_data, here::here("static/labs/data",
                                       "animal_sounds_data.csv"))
```

```
Error in open.connection(file, "wb"): cannot open the connection
```

```r
write_csv(sounds_summary, here::here("static/labs/data",
                                        "animal_sounds_summary.csv"))
```

```
Error in open.connection(file, "wb"): cannot open the connection
```

