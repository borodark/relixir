# relixir
R/Elixir bindings for a more scalable, concurrent, fault-tolerant, distributed web tech world intimately connected to the R data science ecosystem.

## Goal
Position R firmly in the high-traffic web application world, where stability and performance under massive concurrency and load are a right, not a pay-walled privilege.  

## Why?
Because Shiny (shinyapps.io and Shiny Server Pro) and RStudio Connect are often [prohibitively expensive](https://www.rstudio.com/pricing/) (e.g., $74,995 per year for 1000 named users, with each additional named user packs of 250 for $14,995) and are often too slow to [load](https://community.rstudio.com/t/improving-shiny-app-loading-speed/5126) and/or [run](https://support.rstudio.com/hc/en-us/articles/115000171848-Why-are-my-Shiny-apps-are-running-slowly-).  Alternatives like [openCPU](https://github.com/opencpu/opencpu) and [fiery](https://github.com/thomasp85/fiery) are needed and very welcome, but can they support [2 million concurrent connections](http://phoenixframework.org/blog/the-road-to-2-million-websocket-connections) on a single server like Elixir can?  And that was from 2015!  By contrast, Shiny _just_ recently announced the ability to handle 10000 active users at rstudio::conf 2018 ([skip to 17:16 in this video for live demo and applause](https://www.rstudio.com/resources/videos/scaling-shiny/)).  

## Impact
For Elixirians, `relixir` will open up the R data science ecosystem of packages (CRAN and Bioconductor), making it native to call any statistics, machine learning, visualization, or bioinformatics library directly from within Elixir.  For useRs, `relixir` will open up high-availability enterprise-scale web applications deployable through [Digital Ocean](https://www.digitalocean.com), [SSD Nodes](https://www.ssdnodes.com), [AWS](https://aws.amazon.com), etc.  

## Rage against the machine
Paywalling web applications is good business but bad for the open-source community and can often be a slippery slope -- if one had a dollar to give every time they heard "use Python for building serious large-scale web apps", (s)he would be bankrupt.  For the Djangonauts out there, [the following](https://cheesecakelabs.com/blog/phoenix-framework-guide-django-programmers/) can be a sobering read from a fellow Djangonaut.

## Open-source community outreach
At [Bioquilt](http://www.bioquilt.com), we use Elixir to power our web stack while using R and Python to power our data science and machine learning.  Other companies using Elixir and open-sourcing tools for the community include [Pinterest](https://venturebeat.com/2015/12/18/pinterest-elixir/) and [WhatsApp](https://www.wired.com/2015/09/whatsapp-serves-900-million-users-50-engineers/), recently sold to Facebook for $16 billion.  Here's a list of [10 companies that use Elixir in production](https://www.netguru.co/blog/10-companies-use-elixir).  

## Roadmap
- [ ] Extend [erserve](https://github.com/del/erserve) from Erlang to Elixir for the purposes of making calls to R from Elixir (where Elixir is effectively "Erlang++")
- [ ] Trailblaze new code for calling Elixir from R
- [ ] Once `R <--> Elixir` bridge is complete, wrap up the methods into a package like [reticulate](https://github.com/rstudio/reticulate) 

## On-going work

At the time of this writing, `relixir` provides a wrapper around the R command-line utility [littler](http://dirk.eddelbuettel.com/code/littler.html).
`Relixir` supports executing a chunk of R code, returning a variable in the format of an R-object, or a JSON string. Support for R-object is intended for a future feature: piping R-returned output in Elixir style. For direct use of the output, JSON is recommended.

Below are some example calls of `Relixir.runR`. `bioq` repo gives an example how `Relixir` can be used to provide R computational service in a web-app.

```elixir
# Generate n samples from the normal distribution with default mean and sd, return a histogram in JSON format
rCode = """
    x <- rnorm(n=#{size})
    y <- hist(x,plot=FALSE)
    """
result = Relixir.runR(rCode, "y", %{"output" => "json"})
```

```elixir
# Read a csv file and return the column names
cnames = Relixir.runR("""
    X <-read.csv("#{csvFilePath}")
    cnames <- colnames(X)
    ""","cnames")
```

## Prospective contributors
Interest and/or skills in R internals and/or Elixir software development required.  

## Authors
[Trang Tran](https://github.com/ttdtrang) and [Bohdan Khomtchouk, Ph.D.](https://github.com/Bohdan-Khomtchouk)

## Preliminary testing
Prelim testing is temporarily being conducted in [bioq](https://github.com/Bioquilt/bioq).
