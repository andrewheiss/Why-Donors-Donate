
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Why Donors Donate: Disentangling Organizational and Structural Heuristics for International Philanthropy

[Suparna Chaudhry](http://www.suparnachaudhry.com/) • Lewis & Clark
College  
[Marc Dotson](https://marriottschool.byu.edu/directory/details?id=50683)
• Brigham Young University  
[Andrew Heiss](https://www.andrewheiss.com) • Georgia State University

------------------------------------------------------------------------

## Abstract

As space for civil society has closed around the world, transnational
NGOs have faced a crisis of funding. We explore how NGOs have shifted
from traditionally Northern funding sources toward grassroots private
philanthropic money. How do individual donors in the US feel about
donating to legally besieged NGOs abroad? Do legal restrictions on NGOs
influence donors’ decision to donate? We use a conjoint survey
experiment to argue that domestic political environments of NGO host
countries influence preferences of private donors and that legal
crackdowns on NGOs serve as a heuristic of organizational deservingness.

------------------------------------------------------------------------

This repository contains the data and code for our paper. Our pre-print
is online here:

> Suparna Chaudhry, Marc Dotson, and Andrew Heiss. (2021). *Why Donors
> Donate: Disentangling Organizational and Structural Heuristics for
> International Philanthropy*. Accessed September 22, 2021. Online at
> `FUTURE_URL`

Our analysis notebook is accessible at
<https://stats.andrewheiss.com/why-donors-donate/>.

## How to cite

Please cite this compendium as:

> Suparna Chaudhry, Marc Dotson, and Andrew Heiss. (2021). *Compendium
> of R code and data for Why Donors Donate: Disentangling Organizational
> and Structural Heuristics for International Philanthropy*. Accessed
> September 22, 2021. Online at `FUTURE_URL`

## How to download and install

**NB: These instructions are all out of date and will be updated to
match how the project is actually run, eventually. We’re converting this
project from a `make`-based build system to a
[{targets}](https://books.ropensci.org/targets/)-based build system,
which is easier to manage and requires fewer external dependencies.**

*This stuff is accurate*:

Because the posterior draws from the models are large and take forever
to make, we’ve saved the results as `.rds` files. However, because those
files are fairly big and we don’t want to track them with git, we’ve
stored them at this project’s [OSF page](https://osf.io/r59xz/) in
[OSF’s internal storage system](https://osf.io/r59xz/files/). Running
`targets::tar_make(starts_with("file_"))` (or just `targets::tar_make()`
in general) will trigger a function (`get_from_osf()`) that will
automatically download all the `.rds` files and place them in
`data/raw_data/posterior_draws`, which is not tracked by git.

### Ignoring things in Dropbox

If you’re part of the research team and using Dropbox, make sure you
make it so that Dropbox ignores the `posterior_draws` folder and the
`_targets` folder. There are [complete instructions for doing that
here](https://help.dropbox.com/files-folders/restore-delete/ignored-files).
In short, run one of these commands in your terminal:

``` sh
# On macOS
cd path/to/this/project  # Not needed if you use the terminal panel in RStudio after opening the project
xattr -w com.dropbox.ignored 1 _targets
xattr -w com.dropbox.ignored 1 data/raw_data/posterior_draws

# On Windows (with PowerShell)
cd path/to/this/project  # Not needed if you use the terminal panel in RStudio after opening the project
Set-Content -Path '_targets' -Stream com.dropbox.ignored -Value 1
Set-Content -Path 'data/raw_data/posterior_draws' -Stream com.dropbox.ignored -Value 1
```

The icon next to those folders should then change to a gray minus sign,
which means that the folder is being ignored.

------------------------------------------------------------------------

*This stuff is out of date*:

You can either [download the compendium as a ZIP
file](/archive/master.zip) or use GitHub to clone or fork the compendium
repository (see the green “Clone or download” button at the top of the
GitHub page).

In order to reproduce this project, you’ll need to install the
compendium as an R package. After downloading the compendium, do the
following:

To reproduce the analysis, run `make build` from RStudio’s “Terminal”
panel. Open `analysis/_site/` to see the results. Run `make serve` to
serve the site at <http://localhost:7000>.

To reproduce the paper, run `make html` or `make tex` or `make docx` or
`make paper` (for all three output formats) from the terminal. Open
`manuscript/` (or `manuscript/tex_out/` for PDFs) to see the results.

------------------------------------------------------------------------

## Project layout

There are several subdirectories with specific purposes:

**Tracked with git**

-   `/data`: Data goes here. ***Temporarily* not tracked with git.**
-   `/analysis`: All analysis related files go here as standalone R
    Markdown files dedicated to specific tasks (like `01_clean-data.Rmd`
    and `02_analysis.Rmd`, etc.). The most current version of this
    analysis notebook is accessible online at
    <https://stats.andrewheiss.com/why-donors-donate/>.
-   `/R`: Various project-specific R scripts and functions go here.
-   `/manuscript`: The actual writing goes here. A separate Makefile
    here generates the output as HTML, PDF (through XeTeX), and Word.
    For now, that Makefile depends on a bunch of helper scripts that
    only live on Andrew Heiss’s computer, so he’s the only one that can
    build the actual paper. Eventually he’ll move those scripts (into
    the `/manuscript/pandoc` folder) here so anyone can build it.

**Not tracked with git**

These folders are ignored with `.gitignore` and instead live in a shared
Dropbox folder.

-   `/analysis/_site`: This is where the generated website lives. Don’t
    edit anything here—it gets deleted and rewritten all the time.
-   `/analysis/output`: All exported tables and figures go here.
-   `/admin`: For administrative tasks, like IRB, pre-registration,
    readings, and other general files.
-   `/manuscript/submissions`: For journal and conference sub missions.

## Project workflow

This project is designed to work with
[RStudio](https://www.rstudio.com/), and it is built as an [R Markdown
website](https://bookdown.org/yihui/rmarkdown/rmarkdown-site.html) which
gets [uploaded to the
internet](https://stats.andrewheiss.com/why-donors-donate/) via a
Makefile. Because of SSH keys and server credentials, only Andrew Heiss
can upload the compiled site.

To build the site, click on the “Build Website” button in the Build tab
in RStudio. All `.Rmd` files in the root of the project will be knit
individually in isolated R sessions. `.Rmd` files not in the project
root are not knit when building the site.

We follow a few style and workflow guidelines:

-   Write code in R Markdown files.

-   Try to follow the [tidyverse style
    guide](https://style.tidyverse.org/)

-   Use `here::here()` to specify file paths. This makes it easier to
    move work from the `sandbox` folder to an actual R Markdown file.
    Here are some examples of using it in scripts:

    ``` r
    library(tidyverse)
    library(here)

    # Load custom plot functions
    source(here("R", "graphics.R"))

    # Load general global options
    source(here("analysis", "options.R"))

    # Save a plot
    ggsave(plot_name, filename = here("analysis", "output", "figures", "figure1.pdf"), 
           width = 6, height = 4)

    # Save a table
    some_data_frame %>% 
      pandoc.table.return(caption = "Some table {#tbl:table-id}") %>% 
      cat(file = here("analysis", "output", "tables", "tbl-some-table.md"))
    ```

## Collaboration

We welcome contributions from everyone. Before you get started, please
see our [contributor guidelines](CONTRIBUTING.md). Please note that this
project is released with a [Contributor Code of Conduct](CONDUCT.md). By
participating in this project you agree to abide by its terms.

Because not all collaborators use git, we use Dropbox as the main method
for sharing code and data. Contact [Andrew
Heiss](mailto:andrew@andrewheiss.com) to get access to the shared
Dropbox folder.

At the same time, though, having actual version control with Git is
nice. To make Git and Dropbox work correctly together, [we follow this
system](http://www.math.cmu.edu/~gautam/sj/blog/20160406-dropbox-git.html).
Here’s how it works:

-   The shared Dropbox folder contains everything for the project—the
    current git repository + all untracked files.
-   **If you *are not* a git user**, make edits in the Dropbox folder
    and let Andrew know what you’ve done. He’ll commit changes for you.
-   **If you *are* a git user**, fork this repository to somewhere on
    your computer that’s *not* in Dropbox. You’ll work on your own local
    copy, pull any upstream changes, make edits, create branches, submit
    pull requests, and do all regular git things. Do not make any
    changes to the Dropbox folder itself, unless you’re adding files to
    the folders that aren’t tracked by git (e.g. adding a new paper to
    `/readings`).
-   When pull requests are accepted or other commits are made, Andrew
    will pull those changes into the shared Dropbox folder.

tl;dr version:

-   If you don’t use git, edit files in the shared Dropbox folder and
    let Andrew know what you’ve done.
-   If you do use git, fork this repository, edit files wherever you
    want on your computer (not in Dropbox, though), and make pull
    requests to the [main
    repository](https://github.com/andrewheiss/why-donors-donate). Only
    do stuff with the shared Dropbox folder if you’re changing things
    that aren’t tracked with git.
