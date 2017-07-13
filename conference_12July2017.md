# SciPy 2017 Conference Day 1
### 12 July 2017

## Welcome

## Keynote: Academic Open Source
- Kathryn Huff

"The Practice of Reproducible Research"

Data versioning hasn't permiated through to academia yet. Although industry has developed some tools.

Some recommendations:
- Use tests.
- Explicitly set seeds for random numbers.
- Plain text data is preferred over funky formats.
- Avoid excessive dependencies (package everything together!)
- Use all open-source.
- Get DOIs for data and software.
- Be better about citing software.

Some pledges:
- Develop open software from the start when possible
- Contribute to the software I rely on
- Publish and package my own software.

Journal of Open Source Software (JOSS) just went live.

## Python + Tableau: Building an Interactive and Beautiful Data Visualization with TabPy
- Chloe Tseng  (Sr Data Analyst at Twitter)

Tabpy:

Allows you to create powerful visualizations with a Python wrapper around Tableau. 

Many people use D3 for public facing data interactions. Tableau, however, is easier to use.

	pip install tabpy-server

Design:

Should use preattentive attributes to get your audience to grasp in a few seconds the important thing: color, size, length, etc.

Your audiences: Managers (want reports), the public (want infographics), analysts (want analytic tools). 

In visualization of a subject, try to use that subject (chicken, beer) in the visual. 

## Dataflow Notebooks: Encoding and Using Cell Dependencies
- David Koop (University of Massachusetts Dartmouth)

How to restructure a notebook so it doesn't need to be rewritten once you "know what is going on"?

A dataflow is a set of computational modules with connections that connet outputs of one output to intput of another. 

We would like a notebook to be like a dataflow, where cells don't have to be run linearly but can be connected recursevely. Connect cells by ID and if run a cell upstream, automatically re-run the downstream dependent cells.

	Out['cellid'].plot("bar")
	# do other usual stuff in cell

## From the Field to Geostationary Orbit: Mapping Lightning with Python
- Eric Burning

## Robust Color Matching of Geospatial Data
- Joe Kington

"Planet" company makes small satellites, imaging Earth daily.

Remote sensing data is challenging. How seperate atmosphere from actual changes on ground. Sensers change. Seasons make changes. How mosaic such images seemlessly?

Histogram matching: make histograms of each band have the same shape as the histogram of the reference image.  Easy to implement. No parameters to tune. Works really well when input and reference have similar content.

This goes crazy when try to match with images with clouds, snow, seasonal changes...

Need better method that can handle snow, clouds, and preserve white-balance (black should stay black, white should stay white). Use functions that change linear to nonlinear curves for hour channels; want curves that are very smooth. 

	github.com/planetlabs/robust_color

## Reskit - A Library for Creating and Currating Reproducible Pipelines for Scientific Machine Learning
- Dmitry Petrov

## Matchpy: A Pattern Matching Library
- Manuel Krebber

## Lightning Talks

- Github pages magically makes a website by putting HTML on a page.  Doctr will make Gitbub pages using Travis.
- FigureFirst: package to make figures using Matplotlib and Inkspace.
- synthpy: library for synthetic data. 
- PlasmaPy: a Python ecosystem for plasma science. 
- bit.ly/burritorev








