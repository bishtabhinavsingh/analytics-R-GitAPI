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
