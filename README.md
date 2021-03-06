### Source:
The data was originally pulled from [GitHub](https://github.com/) which is a website used by developers to share their code and work on projects as a developer community. The objective was to choose a popular Github user.

### GitHub user - George Hotz:
[George Hotz](https://en.wikipedia.org/wiki/George_Hotz) is an entrepreneur, cybersecurity expert and a software engieer. Known for his recent contribution in the field of AI and urban mobility with autonomous driving with his company [comma.ai](https://comma.ai).

At [GitHub](https://github.com/geohot) George Hotz has over 2,142 contributions last year, he has over 25,200 followers and 73 repositories.

### GitHub API:

GitHub API was used to pull out data after identifying the GitHub user of interest. For the purpose of this assignment following libraries were utilized.

- gh
- purrr
- dplyr
- tidyverse
- jsonlite

The data requested using the API returned as a gh_response object which was JSON formatted. This was converted using the jsonlite package to a dataframe to manipulate and explore the data further.
## Objective 1: Collecting data
### Step 1: Setup and using API

```{r setup, include=TRUE}
knitr::opts_chunk$set(echo = TRUE)
#install.packages("gh")
library(gh)
library(purrr)
library(dplyr)
library(tidyverse)
library(flexdashboard)
my_token = "(toke goes here)"
Sys.setenv(GITHUB_TOKEN = my_token)
```

### Step 2: A table showing the user's login, name, public_repos, followers

```{r t1}
geohot <- gh("/users/geohot")
geo_df = jsonlite::fromJSON(jsonlite::toJSON(geohot))
select <- c('login', 'name', 'public_repos', 'followers')
t1 <- as.tibble(geo_df[select])
knitr::kable(head(t1), "simple")
```


### Step 3: A table summarizing the user's follower's login, name, public_repos, followers
```{r t2}
hot_followers <- gh("/users/geohot/followers", .limit = Inf)
followers_detail <- list()
for (i in 1:1000){
  followers_detail[[i]] <- gh("/users/{login}", login = hot_followers[[i]]$login)
}
followers_detail_df = jsonlite::fromJSON(jsonlite::toJSON(followers_detail))
t2<- as.tibble(followers_detail_df[select])
t2 %>% 
  mutate(followers = as.double(followers)) %>%
  top_n(., n = 10, followers) -> t_temp 
knitr::kable(head(t_temp), "simple")
```

### Step 4: A table summarizing the repositories' name, size, fork_count, open_issues

```{r t3}
hot_repos <- gh("/users/geohot/repos", .limit = Inf) # get all repos
hot_repos_df = jsonlite::fromJSON(jsonlite::toJSON(hot_repos))
select_repo <- c('name', 'size', 'forks_count', 'open_issues')
t3 <- as.tibble(hot_repos_df[select_repo])
knitr::kable(head(t3), "simple")
```


Scatter graph 
-------------------------------------
    
> A graph between number of followers and public_repos for about 5,000 of the total 25,147 followers of GeoHotz.
> No correlation was observed in the two variables for the sample of followers of the user.
    
```{r}
t2 %>% 
  mutate(public_repos = as.double(public_repos),
         followers = as.double(followers)) %>%
  ggplot(., aes(x=public_repos, y=followers)) + geom_point()
```
Fork_count vs open_issues (for user geohot)
-------------------------------------

> A very weak correlation can be seen visually between fork_count and open_issues for the top 30 repos (by open_issues count)
    
```{r}
t3 %>%
  mutate(open_issues = as.double(open_issues),
         name = as.character(name),
         size = as.double(size),
         forks_count= as.double(forks_count)) %>%
  top_n(.,  n = 30, open_issues) %>%
  ggplot(., aes(x=forks_count, y=open_issues)) + geom_point() + scale_fill_hue(c = 40) + theme(axis.text.x = element_text(angle = 90)) 
```

Top 10 Forks and open issues  {.tabset .tabset-fade}
-------------------------------------
   
### Top 10 Forks

```{r}
t3 %>%
  mutate(forks_count = as.double(forks_count),
         name = as.character(name)) %>%
  top_n(.,  n = 10, forks_count) %>%
  ggplot(., aes(x=name, y=forks_count)) + geom_bar(stat="identity") + theme(axis.text.x = element_text(angle = 90))
```   
 
### Top 10 open issues
    
```{r}
t3 %>%
  mutate(open_issues = as.double(open_issues),
         name = as.character(name)) %>%
  top_n(.,  n = 10, open_issues) %>%
  ggplot(., aes(x=name, y=open_issues)) + geom_bar(stat="identity") + scale_fill_hue(c = 40) + theme(axis.text.x = element_text(angle = 90)) 
```
