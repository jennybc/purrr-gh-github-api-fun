
<!-- README.md is generated from README.Rmd. Please edit that file -->
Bulk operations via the GitHub API using `purrr` and `gh`
---------------------------------------------------------

*intro would go here*

### Close all the issues for a repo

For Greg Wilson.

You will need a GitHub personal access token (PAT): <https://github.com/settings/tokens>. The default scopes should be fine. Remember to capture it while it's displayed, because there's no way to see it a second time! If you goof that up, just "edit" and regenerate the token.

``` r
## use install.packages() to install anything that's on CRAN
## install.packages("purrr")
library(purrr)

## use devtools::install_github() to install anything that's on GitHub but
## not CRAN
## install.packages("devtools")
## devtools::install_github("gaborcsardi/gh")
library(gh)

## populate with your targetted user/owner and repo
owner <- "jennybc"
repo <- "foo"

## we usually have the GitHub PAT in an environment variable
## so we don't have to fiddle with it often
## you can temporarily emulate like so:
Sys.setenv("GITHUB_PAT" = "YOUR_GITHUB_PAT_GOES_HERE")

## get open issues
issues <- gh("/repos/:owner/:repo/issues", owner = owner, repo = repo)

## close them
issues <- issues %>% 
  map_int("number") %>% 
  map(~ gh("PATCH /repos/:owner/:repo/issues/:number",
           owner = owner, repo = repo, state = "closed", number = .x))
```
