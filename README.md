# 04-Remix

> Remixing a data visualization

> Find the live website [here](http://kjmj.github.io/04-Remix)

## Build Setup

```bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn dev

# build for production and launch server
$ yarn build
$ yarn start

# generate static project
$ yarn generate

# deploy site to github pages
$ yarn generate:gh-pages
$ yarn deploy

```

For detailed explanation on how things work, check out [Nuxt.js docs](https://nuxtjs.org).

## Project Writeup

### Description and Screenshots

When I found the original viz online, I was lucky enough to also find the dataset associated with it. That dataset can be found [here](https://data.world/ansakoy/countries-in-new-york-times-2000-2016). It is important to mention that the dataset kept track of the attention different countries received from the NY Times in different years. They measured this by counting the number of articles published by the NY Times where a particular country is mentioned.

Keep in mind that this data isn’t normalized or anything, and number of mentions might not be an excellent metric to deduce conclusions from. However, I think that the metric has validity when we are trying to understand why a country was mentioned more than normal during a certain year when compared to other years.

#### Original Viz

Here is the original viz. It shows the number of times a country was mentioned in the NY Times for the year 2016. I felt that a choropleth map and heatmap were charts better suited to this data.

![Map](static/oldViz.png)

#### Choropleth Map - My Remix #1

Here is the choropleth map I created. You can select a year, and the countries will be colored darker if they have been mentioned in the NY Times more, and lighter if there were less mentions.

![Map](assets/map.png)

#### Heatmap - My Remix #2

Here is the heatmap I created. You can explore the data by hovering over a cell and getting the year/country/count of times mentioned in the NY Times. The heatmap is useful for determining outliers in our data.

![Heatmap](assets/heatmap.png)

### Technical Achievements

1. I was able to find missing data in the dataset and replace it with values of 'null'. This made the heatmap more interactive, since white boxes would be drawn for cells with missing data.
2. I also had to do some data cleaning to get the NY Times data and topographic data to match up fully.
3. I was able to build this website using vue.js. I tried to take advantage components, sharing data and functions, and its reactivity.

### Design Achievements

1. I used a color scale that was visually appealing and matched the purple theme of the website.
2. Clicking on an item in the navbar will automatically scroll you to that section. This is a nice touch to the website.
3. I implemented a tooltip that follows your mouse around when hovering over something in the viz. It displays useful info about the element you are hovering over.
