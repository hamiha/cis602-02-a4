## Data Visualization (DSC 530/602-02) Assignment 4

## Instructor: Professor [David Koop](http://www.cis.umassd.edu/~dkoop/)

## Goals
Interaction and Linked Views in D3

## Instructions
There are three parts to the assignment. You may complete the assignment in a single HTML file or use multiple files (e.g. one for CSS, one for HTML, and one for JavaScript). **You must use D3 v4 for this assignment**. All visualization should be done using D3 calls. You may use other libraries (e.g. underscore.js or jQuery), but you must credit them in the HTML file you turn in. Extensive [documentation for D3](https://github.com/mbostock/d3/wiki) is available, and [Vadim Ogievetsky's example-based introduction](https://dakoop.github.io/IntroD3) that we went through in class is also a useful reference. In addition, Scott Murray has written a great book named [Interactive Data Visualization for the Web](http://chimera.labs.oreilly.com/books/1230000000345), but *this was written for D3 v3*.

## Due Date
The assignment is due at 11:59pm on Thursday, April 20.

## Submission
You should submit any files required for this assignment on [myCourses](https://webapps.umassd.edu/myumd/bblearn/?crs=myinstitution). You may complete the assignment in a single HTML file or use multiple files (e.g. one for HTML, one for CSS, and one for JavaScript). Note that the files should be linked to the main HTML document accordingly. The filename of the main HTML document should be `a3.html`. myCourses may complain about the files; if so, please zip the files and submit the zip file instead.

## Details
In this assignment, we will analyze [US occupational employment data](https://www.bls.gov/oes/) produced by the [Bureau of Labor Statistics](https://www.bls.gov/). This was inspired by NPR's Visualization of [The Most Common Job in Every State](http://www.npr.org/sections/money/2015/02/05/382664837/map-the-most-common-job-in-every-state), and MarketWatch's [follow-up analysis](http://www.marketwatch.com/story/no-truck-driver-isnt-the-most-common-job-in-your-state-2015-02-12). The statistics are provided for each year, and include breakdowns by state as well as occupation groupings. We will create interactive visualizations that use animated transitions as well as side-by-side views with linked highlighting.

A few important fields of the data:

- `area_title`: the name of the state (e.g. Massachusetts)
- `occ_code`: the code for the occupation category (e.g. 15-0000)
- `occ_title`: the occupation title as a string (e.g. "Computer and Mathematical Occupations:)
- `jobs_1000`: The number of jobs in the occupation category out of 1000
- `emp_total`: The raw number of jobs in the occupation category

### Data Links

Github-Hosted (HTTPS)

- [US Occupational Employment Data 2012-2016](https://cdn.rawgit.com/dakoop/69d42ee809c9e7985a2ff7ac77720656/raw/6707c376cfcd68a71f59f60c3f4569277f20b7cf/occupations.csv)
- [US Map](https://cdn.rawgit.com/dakoop/69d42ee809c9e7985a2ff7ac77720656/raw/6707c376cfcd68a71f59f60c3f4569277f20b7cf/us-states.json)

UMassD-Hosted (HTTP)

- [US Occupational Employment Data 2012-2016](http://www.cis.umassd.edu/~dkoop/cis468-2017sp/a4/occupations.csv)
- [US Map](http://www.cis.umassd.edu/~dkoop/cis468-2017sp/a4/us-states.json)

### 0. Info
Like Assignment 1, start by creating an HTML web page with the title "Assignment 3". It should contain the following text:

- Your name
- Your student id
- The course title ("Data Visualization (DSC 530/CIS 602-02)"), and
- The assignment title ("Assignment 3")
- The text "This assignment is all my own work. I did not copy the code from any other source." (Your inclusion of this text indicates that you understand the consequences of violating the [UMass Dartmouth Student Academic Integrity Policy](http://www.umassd.edu/studentaffairs/studenthandbook/academicregulationsandprocedures/).)

If you used any additional JavaScript libraries, please append a note to this section indicating their usage to the text above (e.g. "I used the [jQuery](http://jquery.com/) library to write callback functions.") Include links to the projects used. You do not need to adhere to any particular style for this text, but I would suggest using headings to separate the sections of the assignment.

A template for the assignment is provided: [html](http://www.cis.umassd.edu/~dkoop/dsc530-2017sp/a4/a4.html), [js](http://www.cis.umassd.edu/~dkoop/dsc530-2017sp/a4/a4.js); save them both as source. **It is highly recommended that you start from this template**.

### 1. Comparing Years (40 pts)
Here. we wish to compare the relative employment in Computer and Mathematical Occupations (`occ_code="15-0000"`) as it changed from 2012 to 2016. This visualization is restricted to the top **18** states. Given a bar chart from 2012, update it to show the bar chart from 2016 via (at least) two transitions. One transition should update the height of the bars; and a second transition should rearrange the bars so they are still ordered in decreasing order. This transition will show which states increased their workforce in this area and which decreased. At the same time, you will need to remove bars that correspond to states that have fallen out of the top 18 **and** add bars that correspond to states that have been added to the top 18. Feel free to be creative about how to show the additions and removals from the top list. Do note that the axis will also need to be updated as the bars moved/are added. In the provided code, you are given the function `createBars` and need to fill in the `updateBars` method. Note that `barH`, `barW`, `barXAxis` are defined as global variables so you may access them in the `updateBars` method.

Hints

- To transition an axis, select it, create a transition with a specified delay and/or duration, and then `call` the updated axis as in the original time it was created.
- To stage transitions so the second transition executes after the first, simply add the transition to the method chain after the first transition.
- You can coordinate transitions by creating them (e.g `t = d3.transition()`) and then passing them to each selection using `.transition(t)`

[Example Solution for part 1](http://www.cis.umassd.edu/~dkoop/dsc530-2017sp/a4/part1.mp4)

### 2. Comparing Occupations (50 pts)
In this part, we wish to compare the relative rankings of each type of occupation in each state. To do so, we have a horizontal bar chart of the occupation classifications used by the BLS, along with a map of the states.

#### a. State Rankings (20 pts)
First, we need to compute the relative state rankings. Fill in the `getStateRankings` method. Use `d3.nest` to create a variable `stateRankings` to reflect each states ranking for the selected occupation category. For example, for "Computer and Mathematical Operations" (code "15-0000"), you should produce an object or map that looks like:

    {"Alabama": 16, "Alaska": 18, "Arizona": 12, "Arkansas": 15, "California": 10, "Colorado": 9, "Connecticut": 14, "Delaware": 10,"District of Columbia": 4, "Florida": 15,"Georgia": 11, "Hawaii": 17, "Idaho": 14, "Illinois": 10, "Indiana": 15, "Iowa": 14, "Kansas": 14, "Kentucky": 15, "Louisiana": 17, "Maine": 15, "Maryland": 8, "Massachusetts": 9, "Michigan": 15, "Minnesota": 11, "Mississippi": 17, "Missouri": 13, "Montana": 16, "Nebraska": 12, "Nevada": 16, "New Hampshire": 13, "New Jersey": 10, "New Mexico": 16, "New York": 15, "North Carolina": 13, "North Dakota": 14, "Ohio": 13, "Oklahoma": 15, "Oregon": 13, "Pennsylvania": 14, "Rhode Island": 13, "South Carolina": 16, "South Dakota": 14, "Tennessee": 15, "Texas": 12, "Utah": 11, "Vermont": 14, "Virginia": 7, "Washington": 9, "West Virginia": 16, "Wisconsin": 13, "Wyoming": 19}

Hints

- Do not filter by occupation code first because you need to find the ranking
- You can sort an array using the `sort` method. D3's [ascending](https://github.com/d3/d3-array#ascending) and [descending](https://github.com/d3/d3-array#descending) methods can help with this.
- [findIndex](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex) can help determine the ranking given a sorted array.

#### b. Brushing (30 pts)

When a user drags the mouse over the bar chart, the selected bar should _highlight_ and the map should update to show the ranking of that occupation in the state. In the provided code, you will need to edit the `jobMouseEnter` function in the `createBrushedVis` method. In addition, you will need to edit the highlighting code for the bars to indicate which bar is selected. or network encoding but encode other attributes.

[Example Solution for part 2.b](http://www.cis.umassd.edu/~dkoop/dsc530-2017sp/a4/part2.mp4)

Hints

- In the event handler, the current category's data may be accessed via `d3.select(this).datum().key`.
- The colors for all states should be updated whenever the job category changes.
- Remember to remove highlights from bars other than the selected one.

### 3. Linked Line Chart (30 pts)
Create a line chart that shows the currently selected occupation's total employment from 2012-2016. When a user highlights a particular state, the line chart should update to show that state's emploment in that field.
