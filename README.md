# Character names and their influence on baby names

_P2 milestone deliverable_, due on `2023-11-17`.\
**Main dataset**: CMU Movie Summary Corpus.

---
**Team name**: `abADAkor`\
**Team members**: Maxime Theurillat, Luca Rosati, Daniel Demko, Thomas Delacroix, Linkai Dai

---

<!-- TOC -->
- [Abstract](#abstract)
- [Research questions](#research-questions)
    - [**Main question**: Can we find a metric to measure the influence of a movie on the name given to newborns corresponding to the name of one of the characters?](#main-question-can-we-find-a-metric-to-measure-the-influence-of-a-movie-on-the-name-given-to-newborns-corresponding-to-the-name-of-one-of-the-characters)
    - [Parallel questions](#parallel-questions)
        - [**Conclusion**: What characteristics of a movie influence the baby naming the most?](#conclusion-what-characteristics-of-a-movie-influence-the-baby-naming-the-most)
- [Proposed additional datasets](#proposed-additional-datasets)
- [Methods](#methods)
    - [Preprocessing & Scraping](#preprocessingipynb)
    - [Data analysis](#data-analysis)
    - [Visualization](#visualization)
- [Missing data and biases](#missing-data-and-biases)
- [Proposed timeline](#proposed-timeline)
- [Team organization](#team-organization)
<!-- /TOC -->


## Abstract

This project aims to explore the cultural impact of cinema on baby naming trends. By examining the occurrence of the characters’ first or last names in movies, we first investigate the potential correlation between these names and the frequency of those same names being given to newborns in the years following the movie’s release. The study will then consider all movies to uncover patterns and provide insight into the extent of cinema's influence and determine whether key parameters are influencing naming conventions, highlighting the interplay between popular culture and personal identity.

## Research questions

### **Main question**: Can we find a metric to measure the influence of a movie on the name given to newborns corresponding to the name of one of the characters?

**Method**: For all movies, match movie characters names to baby names distribution and look at trends of the latter over ten years before the movie release and the five following years, considering the year of release as one of the following years. At first it was chosen to work with three following years, but in order to give some statistical sense to our analysis, it was extended to five years in order to dampen fluctuations in the data, as done for the ten years preeceding the movie release.

### Parallel questions

1. Looking at a [study](https://rightasrain.uwmedicine.org/life/parenthood/winter-baby-conception-trend) estimating that baby conception rates are at the highest in fall/winter season leading to higher birth in summer, will movies released in the summer show the highest correlation with newborn naming?\
**Method**: Differentiate movies by grouping on release month and look at magnitude (if any) of the influence.
2. Does the genre of a movie play a role in the influence of character names on baby naming trends?\
**Method**: Group movies by genre and study their average magnitude of influence.
3. Looking at movie attendance, does its popularity have an effect on the influence of its character names? What about its rating?\
**Method**: Using IMDb dataset, differentiate movies by box office revenue, proportion of rating number and rating and quantify the groups respective influence.
4. Is it possible to identify a difference in characters’ name impact depending on the status they have in the movie (main/secondary character, extra, etc.)?\
**Method**: Use web scraping API with TMDB dataset to order movies’ characters name, then look at average impact per group.
5. Is it possible to differentiate character influence between its gender?\
**Method**: Group by gender and assess magnitude mean influence, then perform t-test and show confidence interval.

#### **Conclusion**: What characteristics of a movie influence the baby naming the most?

## Proposed additional datasets

1. [Social Security Administration (SSA) Baby Names Dataset](https://www.ssa.gov/oact/babynames/limits.html).\
**Why?** It provides the frequency of each name given to babies born in the US each year. It will serve as the primary dataset for analyzing baby name trends.\
**Processing?** Movie character names are often full names or unconventional (“dos”, “lieutenant”, “man”, ...), we split the character names and made sure to keep only those that are in the baby names dataset.
2. [IMDb movie ratings (`title.ratings`)](https://developer.imdb.com/non-commercial-datasets/)\
**Why?** Not only are a lot of box office revenues missing in the initial dataset, but also a large box office doesn’t necessarily mean that a movie is well received, which could influence its cultural impact and the adoption of character names. By including IMDB ratings, we can better gauge a film's popularity and reception, potentially offering a more nuanced understanding of its influence on naming trends. Since we have the number or ratings, we can estimate the uncertainty on the average rating for movies with few ratings.\
**Processing?** 
    - We needed to translate the `tconst` (unique identifier for a movie) into linkable identifiers with our CMU dataset.
    - We filtered out the movies that have few ratings to avoid ratings bias.
3. Wikidata\
**Why?** Wikidata is our middleman between the freebase movie IDs and their IMDB titles (to get the ratings). We will convert the freebase IDs in the CMU dataset to wikidata IDs.\
**Processing?** Web-scraping to get the [IMDB ID tag](https://www.wikidata.org/wiki/Property:P345).
4. [TMDB](https://www.themoviedb.org/bible/movie/59f3b16d9251414f20000003#59f73ca49251416e7100000e](https://developer.themoviedb.org/reference/movie-credits)(https://developer.themoviedb.org/reference/movie-credits))\
**Why?** This dataset allows us to obtain the order of importance of the film's characters thus determining which are the main characters and order their names depending on their movie role.\
**Processing?** Web API to get `movie_id/credits`.

## Methods

### `preprocessing.ipynb`
- **Acquire datasets** from various sources and import them into the notebook
    - Data crawling, scraping, Web API
- **Cleaning the data**…
    - Get rid of data associated with missing character names.
    - Split character names. Remove those that are not existing in the baby names dataset.
    - Find and process all indexes to bind the various tables (freebase to Wikidata data, Wikidata data to IMDb ratings)
    - Perform movie character ordering through use of TMDB database.
- **Definition of influence metric**\
Measure of baby name trend/mean variation. For a given character name in a movie, look at the trend of this name given to babies: focus on 10 years before movie release and 5 years after it (counting the year of release in those 5 years). Perform this analysis for all character names in all movies.
- **Measure influence validation**\
Perform $t$-test to assess significance of change in baby name trend due to character name after movie release. Keep values that are significant at 0.1, others have influenced set to 0.

### Data analysis
Once the influence factor has been computed for all characters from all movies, perform grouping by themes explained in **_parallel questions_** stated above. Use further statistical analysis comprising CI in order to enunciate results supported by data.

### Visualization
Use of `Plotly` Python graphing library to display interactive results and support data story.

## Missing data and biases

- Baby name data only in the US, so we study only the movie impact in the USA.
- About 1/15 of the movie entries have no corresponding IMDB entry.


## Proposed timeline

`(W09)` Project P2\
`(W10)` Finish computation of film influence on baby names trend + sanity check\
`(W11)` Homework 2 deliverables - Project in standby\
`(W12)` Start parallel analysis\
`(W13)` Finish parallel analysis with visualization. Start storytelling web implementation\
`(W14)` Project P3, Final Run: Refine story telling, finish web page implementation

## Team organization

`Thomas`: Parralell questions\
`Linkai`: Vizualisation\
`Daniel`: Web creation master\
`Maxime`: Influence metric + parallel questions\
`Luca`: Influence metric + parallel questions

## Navigating through our repository
To start the journey, please open the `data/preprocessing.ipynb` file [here](./data/preprocessing.ipynb) and follow along.
