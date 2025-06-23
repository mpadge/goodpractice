# goodpractice <img src="man/figures/logo.png" align="right" width="20%" height="20%" />

<!-- badges: start -->
[![R-CMD-check](https://github.com/ropensci-review-tools/goodpractice/workflows/R-CMD-check/badge.svg)](https://github.com/ropensci-review-tools/goodpractice/actions)
[![CRAN
status](https://www.r-pkg.org/badges/version/goodpractice)](https://CRAN.R-project.org/package=goodpractice)
[![CRAN RStudio mirror
downloads](https://cranlogs.r-pkg.org/badges/goodpractice)](https://www.r-pkg.org/pkg/goodpractice)
[![Codecov test
coverage](https://codecov.io/gh/ropensci-review-tools/goodpractice/branch/main/graph/badge.svg)](https://app.codecov.io/gh/ropensci-review-tools/goodpractice?branch=main)
<!-- badges: end -->

## Advice on R Package Building

Give advice about good practices when building R packages. Advice
includes functions and syntax to avoid, package structure, code
complexity, code formatting, etc.

## Installation

You can install the release version from CRAN

``` r
install.packages("goodpractice")
```

and the development version from GitHub

``` r
pak::pak("ropensci-review-tools/goodpractice")
```

## Usage

``` r
library(goodpractice)
gp("<my-package>")
```

## Example

``` r
library(goodpractice)
# use example package contained in the goodpractice package
pkg_path <- system.file("bad1", package = "goodpractice")
g <- gp(pkg_path)
```

    #> ── R CMD build ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    #>      checking for file ‘/tmp/RtmpToCodX/remotes3aa707ad21a85/badpackage/DESCRIPTION’ ...  ✔  checking for file ‘/tmp/RtmpToCodX/remotes3aa707ad21a85/badpackage/DESCRIPTION’
    #>   ─  preparing ‘badpackage’:
    #>    checking DESCRIPTION meta-information ...  ✔  checking DESCRIPTION meta-information
    #>      checking vignette meta-information ...  ✔  checking vignette meta-information
    #>   ─  checking for LF line-endings in source and make files and shell scripts (389ms)
    #>   ─  checking for empty or unneeded directories
    #>   ─  building ‘badpackage_1.0.0.tar.gz’
    #>      
    #> 

``` r
g
```

    #> ── GP badpackage ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    #> 
    #> It is good practice to
    #> 
    #>   ✖ not use "Depends" in DESCRIPTION, as it can cause name clashes, and poor interaction with other packages. Use "Imports" instead.
    #>   ✖ omit "Date" in DESCRIPTION. It is not required and it gets invalid quite often. A build date will be added to the package when you perform `R CMD build` on it.
    #>   ✖ add a "URL" field to DESCRIPTION. It helps users find information about your package online. If your package does not have a homepage, add an URL to GitHub, or the CRAN package package page.
    #>   ✖ add a "BugReports" field to DESCRIPTION, and point it to a bug tracker. Many online code hosting services provide bug trackers for free, https://github.com, https://gitlab.com, etc.
    #>   ✖ omit trailing semicolons from code lines. They are not needed and most R coding standards forbid them
    #> 
    #>     'R/semicolons.R:4:30'
    #>     'R/semicolons.R:5:29'
    #>     'R/semicolons.R:9:38'
    #> 
    #>   ✖ not import packages as a whole, as this can cause name clashes between the imported packages. Instead, import only the specific functions you need.
    #>   ✖ fix this R CMD check ERROR: VignetteBuilder package not declared: ‘knitr’ See section ‘The DESCRIPTION file’ in the ‘Writing R Extensions’ manual.
    #>   ✖ avoid 'T' and 'F', as they are just variables which are set to the logicals 'TRUE' and 'FALSE' by default, but are not reserved words and hence can be overwritten by the user.  Hence, one should always use 'TRUE' and 'FALSE' for the logicals.
    #> 
    #>     'R/tf.R'
    #>     'R/tf.R'
    #>     'R/tf.R'
    #>     'R/tf.R'
    #>     'R/tf.R'
    #>     ... and 4 more lines
    #> 
    #> ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

``` r
# show all available checks
# all_checks()

# run only a specific check
g_url <- gp(pkg_path, checks = "description_url")
g_url
```

    #> ── GP badpackage ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    #> 
    #> It is good practice to
    #> 
    #>   ✖ add a "URL" field to DESCRIPTION. It helps users find information about your package online. If your package does not have a homepage, add an URL to GitHub, or the CRAN package package page.
    #> ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

``` r
# which checks were carried out?
checks(g_url)
```

    #> [1] "description_url"

``` r
# which checks failed?
failed_checks(g)
```

    #> [1] "no_description_depends"                
    #> [2] "no_description_date"                   
    #> [3] "description_url"                       
    #> [4] "description_bugreports"                
    #> [5] "lintr_semicolon_linter"                
    #> [6] "no_import_package_as_a_whole"          
    #> [7] "rcmdcheck_package_dependencies_present"
    #> [8] "truefalse_not_tf"

``` r
# show the first 5 checks carried out and their results
results(g)[1:5,]
```

    #>                    check result
    #> 1                   covr     NA
    #> 2              cyclocomp   TRUE
    #> 3 no_description_depends  FALSE
    #> 4    no_description_date  FALSE
    #> 5        description_url  FALSE

## Contributing

We welcome any and all contributions to this package. See
[CONTRIBUTING.md](https://github.com/ropensci-review-tools/goodpractice/blob/main/CONTRIBUTING.md)
for details.

## License

MIT © 2022 Ascent Digital Services UK Limited

## Contributors

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->

All contributions to this project are gratefully acknowledged using the
[`allcontributors` package](https://github.com/ropensci/allcontributors)
following the [all-contributors](https://allcontributors.org)
specification. Contributions of any kind are welcome!

### Code

<table>
<tr>
<td align="center">
<a href="https://github.com/gaborcsardi">
<img src="https://avatars.githubusercontent.com/u/660288?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=gaborcsardi">gaborcsardi</a>
</td>
<td align="center">
<a href="https://github.com/hfrick">
<img src="https://avatars.githubusercontent.com/u/12950918?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=hfrick">hfrick</a>
</td>
<td align="center">
<a href="https://github.com/mpadge">
<img src="https://avatars.githubusercontent.com/u/6697851?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=mpadge">mpadge</a>
</td>
<td align="center">
<a href="https://github.com/owenjonesuob">
<img src="https://avatars.githubusercontent.com/u/21007837?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=owenjonesuob">owenjonesuob</a>
</td>
<td align="center">
<a href="https://github.com/ddbortoli">
<img src="https://avatars.githubusercontent.com/u/25244497?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=ddbortoli">ddbortoli</a>
</td>
<td align="center">
<a href="https://github.com/KarinaMarks">
<img src="https://avatars.githubusercontent.com/u/20110563?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=KarinaMarks">KarinaMarks</a>
</td>
<td align="center">
<a href="https://github.com/olivroy">
<img src="https://avatars.githubusercontent.com/u/52606734?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=olivroy">olivroy</a>
</td>
</tr>
<tr>
<td align="center">
<a href="https://github.com/dougmet">
<img src="https://avatars.githubusercontent.com/u/5878305?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=dougmet">dougmet</a>
</td>
<td align="center">
<a href="https://github.com/fabian-s">
<img src="https://avatars.githubusercontent.com/u/998541?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=fabian-s">fabian-s</a>
</td>
<td align="center">
<a href="https://github.com/noamross">
<img src="https://avatars.githubusercontent.com/u/571752?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=noamross">noamross</a>
</td>
<td align="center">
<a href="https://github.com/MichaelChirico">
<img src="https://avatars.githubusercontent.com/u/7606389?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=MichaelChirico">MichaelChirico</a>
</td>
<td align="center">
<a href="https://github.com/fkohrt">
<img src="https://avatars.githubusercontent.com/u/12914806?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=fkohrt">fkohrt</a>
</td>
<td align="center">
<a href="https://github.com/anasimmons">
<img src="https://avatars.githubusercontent.com/u/95026699?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=anasimmons">anasimmons</a>
</td>
<td align="center">
<a href="https://github.com/andrewl776">
<img src="https://avatars.githubusercontent.com/u/64008720?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=andrewl776">andrewl776</a>
</td>
</tr>
<tr>
<td align="center">
<a href="https://github.com/HAlexander23">
<img src="https://avatars.githubusercontent.com/u/70958859?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=HAlexander23">HAlexander23</a>
</td>
<td align="center">
<a href="https://github.com/jsta">
<img src="https://avatars.githubusercontent.com/u/7844578?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=jsta">jsta</a>
</td>
<td align="center">
<a href="https://github.com/LiNk-NY">
<img src="https://avatars.githubusercontent.com/u/4392950?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=LiNk-NY">LiNk-NY</a>
</td>
<td align="center">
<a href="https://github.com/nfultz">
<img src="https://avatars.githubusercontent.com/u/418638?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=nfultz">nfultz</a>
</td>
<td align="center">
<a href="https://github.com/russHyde">
<img src="https://avatars.githubusercontent.com/u/7734886?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=russHyde">russHyde</a>
</td>
<td align="center">
<a href="https://github.com/marberts">
<img src="https://avatars.githubusercontent.com/u/62676717?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/commits?author=marberts">marberts</a>
</td>
</tr>
</table>

### Issue Authors

<table>
<tr>
<td align="center">
<a href="https://github.com/peterhurford">
<img src="https://avatars.githubusercontent.com/u/5100840?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Apeterhurford">peterhurford</a>
</td>
<td align="center">
<a href="https://github.com/stillmatic">
<img src="https://avatars.githubusercontent.com/u/4743676?u=ac3fc940694e8fea8013e09465b70ba9980e1482&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Astillmatic">stillmatic</a>
</td>
<td align="center">
<a href="https://github.com/eribul">
<img src="https://avatars.githubusercontent.com/u/7790927?u=b673c206f8bd2cc32217318673b347b5ae09cf9d&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Aeribul">eribul</a>
</td>
<td align="center">
<a href="https://github.com/richelbilderbeek">
<img src="https://avatars.githubusercontent.com/u/2098230?u=eb0321e52ef07f95057edef9a69350878b4e7a0d&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Arichelbilderbeek">richelbilderbeek</a>
</td>
<td align="center">
<a href="https://github.com/nathaneastwood">
<img src="https://avatars.githubusercontent.com/u/9799530?u=59f2fcbe9ba5ec17471672f351e10014fbd92068&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Anathaneastwood">nathaneastwood</a>
</td>
<td align="center">
<a href="https://github.com/daroczig">
<img src="https://avatars.githubusercontent.com/u/495736?u=0b78c574d39075f2b7e0b8c059cf3be834241a95&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Adaroczig">daroczig</a>
</td>
<td align="center">
<a href="https://github.com/mdozmorov">
<img src="https://avatars.githubusercontent.com/u/864945?u=73720ea73dedcc007bc666956113fc22d3d3cac2&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Amdozmorov">mdozmorov</a>
</td>
</tr>
<tr>
<td align="center">
<a href="https://github.com/vdicolab">
<img src="https://avatars.githubusercontent.com/u/5879492?u=47750b7543a8a588494ef89aa7db563a3a8c6384&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Avdicolab">vdicolab</a>
</td>
<td align="center">
<a href="https://github.com/cboettig">
<img src="https://avatars.githubusercontent.com/u/222586?u=dfbe54d3b4d538dc2a8c276bb5545fdf4684752f&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Acboettig">cboettig</a>
</td>
<td align="center">
<a href="https://github.com/barryrowlingson">
<img src="https://avatars.githubusercontent.com/u/888980?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Abarryrowlingson">barryrowlingson</a>
</td>
<td align="center">
<a href="https://github.com/HenrikBengtsson">
<img src="https://avatars.githubusercontent.com/u/1616850?u=3db13be6479d854fd363b262ae8d379dbd982f91&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3AHenrikBengtsson">HenrikBengtsson</a>
</td>
<td align="center">
<a href="https://github.com/kbenoit">
<img src="https://avatars.githubusercontent.com/u/2182246?u=7b5ff3ddf6c621c156bad611654bbeca6cbe128c&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Akbenoit">kbenoit</a>
</td>
<td align="center">
<a href="https://github.com/njtierney">
<img src="https://avatars.githubusercontent.com/u/6488485?u=3eacd57f61342d1c3cecd5c8ac741b1c4897e1de&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Anjtierney">njtierney</a>
</td>
<td align="center">
<a href="https://github.com/maelle">
<img src="https://avatars.githubusercontent.com/u/8360597?u=824f03caa87c92420352e3dd9a05470320a67412&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Amaelle">maelle</a>
</td>
</tr>
<tr>
<td align="center">
<a href="https://github.com/erleholgersen">
<img src="https://avatars.githubusercontent.com/u/5060086?u=4ab0621f32b784ff78ed98ba2c6c763601db1aa0&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Aerleholgersen">erleholgersen</a>
</td>
<td align="center">
<a href="https://github.com/adfi">
<img src="https://avatars.githubusercontent.com/u/1760262?u=9d618125bc4ade9569e7122611735eb3673c6b2c&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Aadfi">adfi</a>
</td>
<td align="center">
<a href="https://github.com/maurolepore">
<img src="https://avatars.githubusercontent.com/u/5856545?u=640bf30b4798fd06becb5a22d56f38d63cab78d2&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Amaurolepore">maurolepore</a>
</td>
<td align="center">
<a href="https://github.com/jasonserviss">
<img src="https://avatars.githubusercontent.com/u/11668021?u=f4e519e3edf5f12658b0e440403afd29b0ac39bf&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Ajasonserviss">jasonserviss</a>
</td>
<td align="center">
<a href="https://github.com/jackwasey">
<img src="https://avatars.githubusercontent.com/u/7832270?u=5d02a5e29f6fc02f17ad8277f8b6caa33bd143ad&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Ajackwasey">jackwasey</a>
</td>
<td align="center">
<a href="https://github.com/dragosmg">
<img src="https://avatars.githubusercontent.com/u/13176361?u=971be341a6706a4dc6e6e68458dc3eddee5efbac&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Adragosmg">dragosmg</a>
</td>
<td align="center">
<a href="https://github.com/Bisaloo">
<img src="https://avatars.githubusercontent.com/u/10783929?u=38e3754466eaa200e20f0609709467b6331cdfbe&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3ABisaloo">Bisaloo</a>
</td>
</tr>
<tr>
<td align="center">
<a href="https://github.com/bfgray3">
<img src="https://avatars.githubusercontent.com/u/20310144?u=12e6c4b62ad37dc6ff193cbaec61a7828a11b91a&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Abfgray3">bfgray3</a>
</td>
<td align="center">
<a href="https://github.com/wlandau">
<img src="https://avatars.githubusercontent.com/u/1580860?u=6ed1edc717e0853259312206ae59a3aa81fe3bbc&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Awlandau">wlandau</a>
</td>
<td align="center">
<a href="https://github.com/florianm">
<img src="https://avatars.githubusercontent.com/u/762815?u=4cc24043df2493b6395f3d6c1dbc83491ef23163&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Aflorianm">florianm</a>
</td>
<td align="center">
<a href="https://github.com/wibeasley">
<img src="https://avatars.githubusercontent.com/u/1372890?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Awibeasley">wibeasley</a>
</td>
<td align="center">
<a href="https://github.com/dpprdan">
<img src="https://avatars.githubusercontent.com/u/1423562?u=641a09e8d193d9a34951e623a97a8ab67e8bf3e4&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Adpprdan">dpprdan</a>
</td>
<td align="center">
<a href="https://github.com/kwstat">
<img src="https://avatars.githubusercontent.com/u/484260?u=31a8a31b1c2877c2abda463a8332684f69ec6341&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3Akwstat">kwstat</a>
</td>
<td align="center">
<a href="https://github.com/HenningLorenzen-ext-bayer">
<img src="https://avatars.githubusercontent.com/u/89191115?u=9283c073f303b932fcfe78a3b21421c949090dc2&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+author%3AHenningLorenzen-ext-bayer">HenningLorenzen-ext-bayer</a>
</td>
</tr>
</table>

### Issue Contributors

<table>
<tr>
<td align="center">
<a href="https://github.com/drisso">
<img src="https://avatars.githubusercontent.com/u/8451432?u=0e43f4e1f1e18804efcc98ba68b05b5fadc7fc4a&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+commenter%3Adrisso">drisso</a>
</td>
<td align="center">
<a href="https://github.com/HarryJAlexander">
<img src="https://avatars.githubusercontent.com/u/96472942?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+commenter%3AHarryJAlexander">HarryJAlexander</a>
</td>
<td align="center">
<a href="https://github.com/gmbecker">
<img src="https://avatars.githubusercontent.com/u/908721?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+commenter%3Agmbecker">gmbecker</a>
</td>
<td align="center">
<a href="https://github.com/daattali">
<img src="https://avatars.githubusercontent.com/u/952340?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+commenter%3Adaattali">daattali</a>
</td>
<td align="center">
<a href="https://github.com/joelnitta">
<img src="https://avatars.githubusercontent.com/u/13459362?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+commenter%3Ajoelnitta">joelnitta</a>
</td>
<td align="center">
<a href="https://github.com/lgallindo">
<img src="https://avatars.githubusercontent.com/u/15238731?u=7987ba266f3b80d21696e95ddeb23753c0f18651&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+commenter%3Algallindo">lgallindo</a>
</td>
<td align="center">
<a href="https://github.com/annakrystalli">
<img src="https://avatars.githubusercontent.com/u/5583057?u=1e992c8f98d38a959fa064afaeb182bee6e9f666&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+commenter%3Aannakrystalli">annakrystalli</a>
</td>
</tr>
<tr>
<td align="center">
<a href="https://github.com/sda030">
<img src="https://avatars.githubusercontent.com/u/13221371?v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+commenter%3Asda030">sda030</a>
</td>
<td align="center">
<a href="https://github.com/mccroweyclinton-EPA">
<img src="https://avatars.githubusercontent.com/u/37708338?u=58f68265725538054769fe2d2cd39a01bacb3161&v=4" width="100px;" alt=""/>
</a><br>
<a href="https://github.com/ropensci-review-tools/goodpractice/issues?q=is%3Aissue+commenter%3Amccroweyclinton-EPA">mccroweyclinton-EPA</a>
</td>
</tr>
</table>
<!-- markdownlint-enable -->
<!-- prettier-ignore-end -->
<!-- ALL-CONTRIBUTORS-LIST:END -->
