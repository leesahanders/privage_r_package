# privage_r_package

## At a glance 

I always start from here - https://support.posit.co/hc/en-us/articles/360045105794-How-to-set-up-Git-backed-content-deployment-from-a-private-repository-in-RStudio-Connect, and here - https://docs.posit.co/connect/admin/content-management/git-backed/#private-repos

### The biggest problem: Improperly encrypting the PAT



## Create a sample package

Following: <https://statisticsglobe.com/create-package-r> 

Source the package with: 

```r
library(lisaPrivatePackage)
```

Git repo: <https://github.com/leesahanders/privage_r_package> 

Following: <https://cran.r-project.org/web/packages/githubinstall/vignettes/githubinstall.html> 
<https://remotes.r-lib.org/>

## Download a package from github 

```r
install.packages("devtools")
library(devtools)

install.packages("remotes")
library(remotes)
# remotes::install_github("leesahanders/lisaPrivatePackage")

# Create a GitHub PAT via R with 
usethis::create_github_token() 

# Set it with 
gitcreds::gitcreds_set() 
Sys.setenv(GITHUB_PAT = "") 

# Install our private package with renv
renv::install("leesahanders/private_r_package")

# Load the package 
library(lisaPrivatePackage)
```


## Deploying to Connect with private packages 

Just use "remotes::install_github"  and/or renv and point directly at the GH repo that has my packages.

These options might be useful to set so that authentication is being passed along: 

```r
options(packrat.authenticated.downloads.use.renv = TRUE)
options(renv.auth.lisaPrivatePackage = list(GITHUB_PAT = ""))
```

Follow <https://docs.posit.co/connect/admin/r/package-management/#from-private-git-repositories> to set the config to allow restoring credentials: 

```
; /etc/rstudio-connect/rstudio-connect.gcfg
[R]
RestoreUsesGitCredentials = true
```

You must also configure Git credentials as you would to deploy content from private Git repositories. See the section on [Git-backed content](https://docs.posit.co/connect/admin/content-management/git-backed/#private-repos) and refer to [GitCredential](https://docs.posit.co/connect/admin/appendix/configuration/#GitCredential) in the Configuration appendix for more information. The account you configure must be able to access any Git repositories containing R packages you wish to use.




More exploration and resources: 


reference: https://community.rstudio.com/t/setting-github-pat-for-rsconnect/127444 and https://docs.posit.co/connect/admin/r/package-management/#from-private-git-repositories 
Private package from: https://github.com/dgruenew/talkingheadr, https://rstudio.slack.com/archives/C04280LRVQT/p1667430400292769?thread_ts=1667422300.132889&cid=C04280LRVQT 

Create a GitHub PAT via R with usethis::create_github_token() 
Set it with gitcreds::gitcreds_set() and Sys.setenv(GITHUB_PAT = "") 
Store as env variable with usethis::edit_r_environ()
Follow: https://carpentries.github.io/sandpaper-docs/github-pat.html 
For package installation follow: https://rstudio.github.io/renv/reference/install.html?q=authentica#remotes-syntax 
Test our pat with: remotes::install_github("r-lib/conflicted") 
Install our private package: renv::install("dgruenew/talkingheadr")
For publishing to Connect: Try setting the option packrat.authenticated.downloads.use.renv to TRUE, or installing the httr package.

options(packrat.authenticated.downloads.use.renv = TRUE)
options(renv.auth.talkingheadr = list(GITHUB_PAT = ""))
renv:::renv_remotes_resolve("github::dgruenew/talkingheadr")
renv::install("dgruenew/talkingheadr")


Reference: <https://rstudioide.zendesk.com/agent/tickets/60807>

Thank you for contacting RStudio Support.  The workaround described here:

> Private Packages¶
> Packages available on CRAN, a private package repository, or a public GitHub repository are automatically downloaded and built when an application is deployed. RStudio Connect cannot automatically obtain packages from private GitHub repositories, but a workaround is available.


Follows in the admin guide, but as you likely noticed it's not applicable to the version you have in use.  For current versions, it's instead suggested that you host these packages on an internal CRAN mirror.  

This is covered here,  <https://docs.rstudio.com/connect/admin/r/package-management/#private-repositories>

An easy method to create your own repositories is provided by RStudio Package manager.  Do you have that in use already or would you like more information about it? 

An alternative method to RStudio Package manager is mentioned in the link above:  

> Learn how to create your own custom repository; this directory can then be shared over HTTP or through a shared filesystem.


Please let us know if you have any questions.

## Other approaches

Refer to the package manager demo repo: < https://github.com/rstudio/package-manager-demo/> 

