# SciPy 2017 Conference Day 2
### 13 July 2017

## Keynote: Coding for Science and Innovation
- Gae:l Varoquaux

Main enemy to reproducible research is cognitive overload. So many tools to learn and must apply them to obscure theoretical concepts, chances you get it right are small and chances you are using right tool is small.

Don't over-engineer too early. It freezes bad ideas. 

Design principles:
- Consistency.
- Functions are easier to understand than classes.
- A library should hinge on a small number of concepts.
- Common data containers make the ecosystem stronger.
- Each function should have one and only one purpose.
- Shallow is better than deep. Objects are understood by their surface (don't have objects with 100 methods)
- Error messages matter. Explain the problem and print the offending value.

With scikit-learn, API is clear and user-friendly.
To fit, call fit method. To transform data, call transform method.

	estimator = Estimator(param1=param1)
	estimator.fit(X_train, y_train)

Software is like infrastructure (lay people don't even realize it). Everybody use core packages every day, they need maintenance. 

[Astropy people need to watch this presentation]

## Bringing the Generic Mapping Tools (GMT) to Python
- Leonardo Uieda

GMT is a collection of C commmand line prgrams that process spatial data and math on the sphere. Makes very nice maps.

	github/genericmappingtools

## DASH: A New Framework for Building User Interfaces for Techical Computing

Everything is customizable. Full power of CSS available to you.
No javascript dependencies. Pure Python.

	import dash
	import ...

	app = dash.Dash()
	app.layout = html.Div([ ... ])

	if __name__=='__main__':
		app.run_server(debug=True)

`React` has tutorials.

	github.com/plotly/dash

# Introducing JOSS: Journal of Open Source Software

Publishes short software articles; basically metadata + code
Peer review article, software and associated artifacts.
Receive crossdef DOI upon publication.
Arfon is editor-in-chief.

An article about computational result is advertising, not scholarshop. The actual scholarship is the full software environment, code and data, that produced the result.

Submission: Brief summary article + software itself

Idea is that if you developed it properly, this should not be much additional work.

Requirements:
- Be open source.
- Have a research applications.
- Be a significant new contribution to the available open-source software that enables some new research challenges.
- Be feature-complete (at least Version 1). Can submit again to JOSS if version 2 comes out and it is significantly different.
- Submitter must be a major contributor to project.

	http://joss.theoj.org

JOSS itself is on Github. Reviews all happen through github.

Articles are open-access and authors return copyright. Code snippets are subject to an MIT license. 

Don't reject articles.

Don't look too much at the style.

Encourage submitting your software to JOSS first so you can cite it in your science paper.

*Should cite software that you use in your research and could contribute to your results (so if version changes, your research might change)*

## Sacred: How to Stop Worrying and Enjoy the Research

Frame work for running reproducible computational experiments. 

Saved all standard out, copy of your code, your configuration. Results collected in a central MongoDB.

Sacredboard is a web interface so you can see your runs live.

`Sumatra` is a similar tool (can run on other languages). Other environment capturing tools are ReproZip, CDE, PTU, CARE...

## Lightning Talks

- Will soon be a journal for open source education.
- `tex2ipy` converts LaTeX Beamer slides to Jupyter notebooks.
- `ipyaml`: edit your notebooks in an editor, not on browser
- `data-retriever` creates schemas, inserts into databases, standardizes it all in a line. 




