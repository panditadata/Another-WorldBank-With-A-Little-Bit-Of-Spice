
#### Coursera Project to calculate basic statistics of a World Bank Dataset
#### The indicator selected to do all the stats calculation is % Unemployment for Females (WorldWide)
#### Finally, the indicator data is displayed using folium choropleth
Table of Contents
<ul>
<li>Download data, upload data, read data into pandas</li>
<li>Cleaning the data by sorting and merging 2 data sets and dropping unnecessary columns</li>
<li>Selecting 1 indicator </li>
<li>Merging 2 data sets: % Unemployment and income group by country </li>
<li> Removing all the NAN values and countries without values </li>
<li> Calculating main stats of the data set (mean, std, quartiles) </li>
<li> Transpose the data year column, income groups as index </li>
<li> Display mean % Female Unemployment by Income Group and Year </li>
<img src="images/Mean Unemployment.PNG"/>
<li> Display mean % Female Unemployment by Region and Year </li>
<li> ANOVA Calculation of Income Group Data and boxplot display</li>  
<li> Selecting 2019 data </li>  
<li> the indicator data for selected year is displayed using folium choropleth </li>
<img src="images/map.PNG"/>
</ul>



{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {
    "id": "0fq0V22yg6XI"
   },
   "source": []
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "id": "o6Wszh4qnele"
   },
   "source": [
    "# New Section"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "id": "ivbOquPGg8yV"
   },
   "source": [
    "# World Bank Education Data Analysis"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "id": "6X1oMnqOhNFU"
   },
   "source": [
    "## Intro to Python\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 46,
   "metadata": {
    "colab": {
     "base_uri": "https://localhost:8080/"
    },
    "id": "N7zp2RIFiL6E",
    "outputId": "66a5b807-74b5-4cb3-a226-ae290496120f"
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "World Bank\n",
      "5\n",
      "5.5\n",
      "True\n"
