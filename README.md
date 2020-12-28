
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


Entire Project


Open In Colab
New Section
World Bank Education Data Analysis
Intro to Python
In [46]:
print("World Bank")
print(5)
print(5.5)
print(True)
World Bank
5
5.5
True
Loading the Data
Data taken from World Bank Repository

Download data

Upload data

Read data into pandas

In [47]:
import pandas as pd

main_data = pd.read_csv("/content/API_4_DS2_en_csv_v2_1741864.csv", skiprows= 4)
main_data.head()
Out[47]:

In [48]:
import pandas as pd

country_data=pd.read_csv("/content/Metadata_Country_API_4_DS2_en_csv_v2_1741864.csv")
country_data.head()
Out[48]:
Country Code	Region	IncomeGroup	SpecialNotes	TableName	Unnamed: 5
0	ABW	Latin America & Caribbean	High income	NaN	Aruba	NaN
1	AFG	South Asia	Low income	NaN	Afghanistan	NaN
2	AGO	Sub-Saharan Africa	Lower middle income	NaN	Angola	NaN
3	ALB	Europe & Central Asia	Upper middle income	NaN	Albania	NaN
4	AND	Europe & Central Asia	High income	NaN	Andorra	NaN
Cleaning the Data
Sort and merge 2 datasets

Creating dataset

In [49]:
main_data.columns
Out[49]:
Index(['Country Name', 'Country Code', 'Indicator Name', 'Indicator Code',
       '1960', '1961', '1962', '1963', '1964', '1965', '1966', '1967', '1968',
       '1969', '1970', '1971', '1972', '1973', '1974', '1975', '1976', '1977',
       '1978', '1979', '1980', '1981', '1982', '1983', '1984', '1985', '1986',
       '1987', '1988', '1989', '1990', '1991', '1992', '1993', '1994', '1995',
       '1996', '1997', '1998', '1999', '2000', '2001', '2002', '2003', '2004',
       '2005', '2006', '2007', '2008', '2009', '2010', '2011', '2012', '2013',
       '2014', '2015', '2016', '2017', '2018', '2019', '2020', 'Unnamed: 65'],
      dtype='object')
In [50]:
main_data['Indicator Name'].unique()
Out[50]:
array(['Population ages 15-64 (% of total population)',
       'Population ages 0-14 (% of total population)',
       'Unemployment, total (% of total labor force) (modeled ILO estimate)',
       'Unemployment, male (% of male labor force) (modeled ILO estimate)',
       'Unemployment, female (% of female labor force) (modeled ILO estimate)',
       'Labor force, total',
       'Labor force, female (% of total labor force)',
       'Probability of dying among youth ages 20-24 years (per 1,000)',
       'Probability of dying among adolescents ages 15-19 years (per 1,000)',
       'Probability of dying among adolescents ages 10-14 years (per 1,000)',
       'Probability of dying among children ages 5-9 years (per 1,000)',
       'Number of deaths ages 20-24 years',
       'Number of deaths ages 15-19 years',
       'Number of deaths ages 10-14 years',
       'Number of deaths ages 5-9 years',
       'Government expenditure on education, total (% of GDP)',
       'Government expenditure on education, total (% of government expenditure)',
       'Expenditure on tertiary education (% of government expenditure on education)',
       'Government expenditure per student, tertiary (% of GDP per capita)',
       'Expenditure on secondary education (% of government expenditure on education)',
       'Government expenditure per student, secondary (% of GDP per capita)',
       'Expenditure on primary education (% of government expenditure on education)',
       'Government expenditure per student, primary (% of GDP per capita)',
       'Current education expenditure, total (% of total expenditure in public institutions)',
       'Current education expenditure, tertiary (% of total expenditure in tertiary public institutions)',
       'Current education expenditure, secondary (% of total expenditure in secondary public institutions)',
       'Current education expenditure, primary (% of total expenditure in primary public institutions)',
       'Tertiary education, academic staff (% female)',
       'School enrollment, tertiary, male (% gross)',
       'School enrollment, tertiary, female (% gross)',
       'School enrollment, tertiary (% gross)',
       'Pupil-teacher ratio, tertiary',
       'Educational attainment, at least completed short-cycle tertiary, population 25+, total (%) (cumulative)',
       'Educational attainment, at least completed short-cycle tertiary, population 25+, male (%) (cumulative)',
       'Educational attainment, at least completed short-cycle tertiary, population 25+, female (%) (cumulative)',
       "Educational attainment, at least Master's or equivalent, population 25+, total (%) (cumulative)",
       "Educational attainment, at least Master's or equivalent, population 25+, male (%) (cumulative)",
       "Educational attainment, at least Master's or equivalent, population 25+, female (%) (cumulative)",
       'Educational attainment, Doctoral or equivalent, population 25+, total (%) (cumulative)',
       'Educational attainment, Doctoral or equivalent, population 25+, male (%) (cumulative)',
       'Educational attainment, Doctoral or equivalent, population 25+, female (%) (cumulative)',
       "Educational attainment, at least Bachelor's or equivalent, population 25+, total (%) (cumulative)",
       "Educational attainment, at least Bachelor's or equivalent, population 25+, male (%) (cumulative)",
       "Educational attainment, at least Bachelor's or equivalent, population 25+, female (%) (cumulative)",
       'Adolescents out of school (% of lower secondary school age)',
       'Adolescents out of school, male (% of male lower secondary school age)',
       'Adolescents out of school, female (% of female lower secondary school age)',
       'Secondary education, teachers (% female)',
       'Secondary education, teachers, female',
       'Secondary education, teachers',
       'Trained teachers in secondary education (% of total teachers)',
       'Trained teachers in upper secondary education (% of total teachers)',
       'Trained teachers in upper secondary education, male (% of male teachers)',
       'Trained teachers in upper secondary education, female (% of female teachers)',
       'Trained teachers in secondary education, male (% of male teachers)',
       'Trained teachers in lower secondary education (% of total teachers)',
       'Trained teachers in lower secondary education, male (% of male teachers)',
       'Trained teachers in lower secondary education, female (% of female teachers)',
       'Trained teachers in secondary education, female (% of female teachers)',
       'Progression to secondary school (%)',
       'Progression to secondary school, male (%)',
       'Progression to secondary school, female (%)',
       'School enrollment, secondary, private (% of total secondary)',
       'School enrollment, secondary, male (% net)',
       'School enrollment, secondary, female (% net)',
       'School enrollment, secondary (% net)',
       'School enrollment, secondary, male (% gross)',
       'School enrollment, secondary, female (% gross)',
       'School enrollment, secondary (% gross)',
       'Secondary education, vocational pupils (% female)',
       'Secondary education, vocational pupils',
       'Pupil-teacher ratio, upper secondary',
       'Pupil-teacher ratio, secondary',
       'Pupil-teacher ratio, lower secondary',
       'Secondary education, general pupils (% female)',
       'Secondary education, general pupils',
       'Secondary education, pupils (% female)',
       'Secondary education, pupils',
       'Secondary education, duration (years)',
       'Educational attainment, at least completed upper secondary, population 25+, total (%) (cumulative)',
       'Educational attainment, at least completed upper secondary, population 25+, male (%) (cumulative)',
       'Educational attainment, at least completed upper secondary, population 25+, female (%) (cumulative)',
       'Educational attainment, at least completed post-secondary, population 25+, total (%) (cumulative)',
       'Educational attainment, at least completed post-secondary, population 25+, male (%) (cumulative)',
       'Educational attainment, at least completed post-secondary, population 25+, female (%) (cumulative)',
       'Educational attainment, at least completed lower secondary, population 25+, total (%) (cumulative)',
       'Educational attainment, at least completed lower secondary, population 25+, male (%) (cumulative)',
       'Educational attainment, at least completed lower secondary, population 25+, female (%) (cumulative)',
       'Lower secondary completion rate, total (% of relevant age group)',
       'Lower secondary completion rate, male (% of relevant age group)',
       'Lower secondary completion rate, female (% of relevant age group)',
       'Lower secondary school starting age (years)',
       'Children out of school (% of primary school age)',
       'Children out of school, male (% of male primary school age)',
       'Children out of school, primary, male',
       'Children out of school, female (% of female primary school age)',
       'Children out of school, primary, female',
       'Children out of school, primary',
       'Adjusted net enrollment rate, primary, male (% of primary school age children)',
       'Adjusted net enrollment rate, primary, female (% of primary school age children)',
       'Adjusted net enrollment rate, primary (% of primary school age children)',
       'Primary education, teachers (% female)',
       'Primary education, teachers',
       'Trained teachers in primary education (% of total teachers)',
       'Trained teachers in primary education, male (% of male teachers)',
       'Trained teachers in primary education, female (% of female teachers)',
       'Repeaters, primary, total (% of total enrollment)',
       'Repeaters, primary, male (% of male enrollment)',
       'Repeaters, primary, female (% of female enrollment)',
       'Persistence to last grade of primary, total (% of cohort)',
       'Persistence to last grade of primary, male (% of cohort)',
       'Persistence to last grade of primary, female (% of cohort)',
       'Persistence to grade 5, total (% of cohort)',
       'Persistence to grade 5, male (% of cohort)',
       'Persistence to grade 5, female (% of cohort)',
       'School enrollment, primary, private (% of total primary)',
       'Over-age students, primary (% of enrollment)',
       'Over-age students, primary, male (% of male enrollment)',
       'Over-age students, primary, female (% of female enrollment)',
       'Net intake rate in grade 1 (% of official school-age population)',
       'Net intake rate in grade 1, male (% of official school-age population)',
       'Net intake rate in grade 1, female (% of official school-age population)',
       'School enrollment, primary, male (% net)',
       'School enrollment, primary, female (% net)',
       'School enrollment, primary (% net)',
       'Gross intake ratio in first grade of primary education, total (% of relevant age group)',
       'Gross intake ratio in first grade of primary education, male (% of relevant age group)',
       'Gross intake ratio in first grade of primary education, female (% of relevant age group)',
       'School enrollment, primary, male (% gross)',
       'School enrollment, primary, female (% gross)',
       'School enrollment, primary (% gross)',
       'Pupil-teacher ratio, primary',
       'Primary education, pupils (% female)',
       'Primary education, pupils', 'Primary education, duration (years)',
       'Educational attainment, at least completed primary, population 25+ years, total (%) (cumulative)',
       'Educational attainment, at least completed primary, population 25+ years, male (%) (cumulative)',
       'Educational attainment, at least completed primary, population 25+ years, female (%) (cumulative)',
       'Primary completion rate, total (% of relevant age group)',
       'Primary completion rate, male (% of relevant age group)',
       'Primary completion rate, female (% of relevant age group)',
       'Primary school starting age (years)',
       'Trained teachers in preprimary education (% of total teachers)',
       'Trained teachers in preprimary education, male (% of male teachers)',
       'Trained teachers in preprimary education, female (% of female teachers)',
       'School enrollment, preprimary, male (% gross)',
       'School enrollment, preprimary, female (% gross)',
       'School enrollment, preprimary (% gross)',
       'Pupil-teacher ratio, preprimary',
       'Preprimary education, duration (years)',
       'School enrollment, tertiary (gross), gender parity index (GPI)',
       'School enrollment, secondary (gross), gender parity index (GPI)',
       'School enrollment, primary and secondary (gross), gender parity index (GPI)',
       'School enrollment, primary (gross), gender parity index (GPI)',
       'Compulsory education, duration (years)',
       'Literacy rate, adult total (% of people ages 15 and above)',
       'Literacy rate, adult male (% of males ages 15 and above)',
       'Literacy rate, adult female (% of females ages 15 and above)',
       'Literacy rate, youth total (% of people ages 15-24)',
       'Literacy rate, youth male (% of males ages 15-24)',
       'Literacy rate, youth (ages 15-24), gender parity index (GPI)',
       'Literacy rate, youth female (% of females ages 15-24)'],
      dtype=object)
== is a comparison operator and check each column that has the required indicator

In [51]:
main_data_unem=main_data[main_data['Indicator Name']=='Unemployment, female (% of female labor force) (modeled ILO estimate)']
main_data_unem.head()
Out[51]:
Country Name	Country Code	Indicator Name	Indicator Code	1960	1961	1962	1963	1964	1965	1966	1967	1968	1969	1970	1971	1972	1973	1974	1975	1976	1977	1978	1979	1980	1981	1982	1983	1984	1985	1986	1987	1988	1989	1990	1991	1992	1993	1994	1995	1996	1997	1998	1999	2000	2001	2002	2003	2004	2005	2006	2007	2008	2009	2010	2011	2012	2013	2014	2015	2016	2017	2018	2019	2020	Unnamed: 65
4	Aruba	ABW	Unemployment, female (% of female labor force)...	SL.UEM.TOTL.FE.ZS	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN
166	Afghanistan	AFG	Unemployment, female (% of female labor force)...	SL.UEM.TOTL.FE.ZS	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	14.226	14.348000	14.391	14.515000	14.803000	14.505	14.699000	14.710	14.794	14.733000	14.702000	15.036	14.859	14.877000	14.910000	14.431000	14.724	14.154	14.911	14.815	14.781	14.820	14.680	14.505	14.427	14.314	14.090	13.906	14.004	14.062	NaN
328	Angola	AGO	Unemployment, female (% of female labor force)...	SL.UEM.TOTL.FE.ZS	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	2.637	2.723000	2.695	2.882000	2.948000	2.994	2.876000	2.967	2.932	2.798000	2.811000	2.899	2.833	2.882000	2.852000	2.746000	2.723	2.710	2.845	10.922	7.718	7.788	7.772	7.719	7.681	7.563	7.467	7.327	6.942	6.631	NaN
490	Albania	ALB	Unemployment, female (% of female labor force)...	SL.UEM.TOTL.FE.ZS	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	15.804	16.247999	16.711	16.749001	16.766001	16.739	16.434999	16.829	16.916	16.898001	16.992001	17.063	17.104	17.011999	16.919001	16.643999	16.399	13.752	15.734	15.881	13.762	11.467	13.345	15.153	17.098	14.573	12.563	11.229	11.604	12.190	NaN
652	Andorra	AND	Unemployment, female (% of female labor force)...	SL.UEM.TOTL.FE.ZS	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN
In [52]:
main_data_unem= main_data_unem[['Country Name', 'Country Code','1991', '1992', '1993', '1994', '1995',
       '1996', '1997', '1998', '1999', '2000', '2001', '2002', '2003', '2004',
       '2005', '2006', '2007', '2008', '2009', '2010', '2011', '2012', '2013',
       '2014', '2015', '2016', '2017', '2018', '2019', '2020']]
main_data_unem.head()
Out[52]:
Country Name	Country Code	1991	1992	1993	1994	1995	1996	1997	1998	1999	2000	2001	2002	2003	2004	2005	2006	2007	2008	2009	2010	2011	2012	2013	2014	2015	2016	2017	2018	2019	2020
4	Aruba	ABW	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN
166	Afghanistan	AFG	14.226	14.348000	14.391	14.515000	14.803000	14.505	14.699000	14.710	14.794	14.733000	14.702000	15.036	14.859	14.877000	14.910000	14.431000	14.724	14.154	14.911	14.815	14.781	14.820	14.680	14.505	14.427	14.314	14.090	13.906	14.004	14.062
328	Angola	AGO	2.637	2.723000	2.695	2.882000	2.948000	2.994	2.876000	2.967	2.932	2.798000	2.811000	2.899	2.833	2.882000	2.852000	2.746000	2.723	2.710	2.845	10.922	7.718	7.788	7.772	7.719	7.681	7.563	7.467	7.327	6.942	6.631
490	Albania	ALB	15.804	16.247999	16.711	16.749001	16.766001	16.739	16.434999	16.829	16.916	16.898001	16.992001	17.063	17.104	17.011999	16.919001	16.643999	16.399	13.752	15.734	15.881	13.762	11.467	13.345	15.153	17.098	14.573	12.563	11.229	11.604	12.190
652	Andorra	AND	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN
In [53]:
country_data.columns
Out[53]:
Index(['Country Code', 'Region', 'IncomeGroup', 'SpecialNotes', 'TableName',
       'Unnamed: 5'],
      dtype='object')
In [54]:
country_data.columns
country_data=country_data[['Country Code', 'Region', 'IncomeGroup']]
country_data.head()
Out[54]:
Country Code	Region	IncomeGroup
0	ABW	Latin America & Caribbean	High income
1	AFG	South Asia	Low income
2	AGO	Sub-Saharan Africa	Lower middle income
3	ALB	Europe & Central Asia	Upper middle income
4	AND	Europe & Central Asia	High income
In [55]:
merged_data = pd.merge(main_data_unem, country_data, on='Country Code')
merged_data.head()
Out[55]:
Country Name	Country Code	1991	1992	1993	1994	1995	1996	1997	1998	1999	2000	2001	2002	2003	2004	2005	2006	2007	2008	2009	2010	2011	2012	2013	2014	2015	2016	2017	2018	2019	2020	Region	IncomeGroup
0	Aruba	ABW	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	Latin America & Caribbean	High income
1	Afghanistan	AFG	14.226	14.348000	14.391	14.515000	14.803000	14.505	14.699000	14.710	14.794	14.733000	14.702000	15.036	14.859	14.877000	14.910000	14.431000	14.724	14.154	14.911	14.815	14.781	14.820	14.680	14.505	14.427	14.314	14.090	13.906	14.004	14.062	South Asia	Low income
2	Angola	AGO	2.637	2.723000	2.695	2.882000	2.948000	2.994	2.876000	2.967	2.932	2.798000	2.811000	2.899	2.833	2.882000	2.852000	2.746000	2.723	2.710	2.845	10.922	7.718	7.788	7.772	7.719	7.681	7.563	7.467	7.327	6.942	6.631	Sub-Saharan Africa	Lower middle income
3	Albania	ALB	15.804	16.247999	16.711	16.749001	16.766001	16.739	16.434999	16.829	16.916	16.898001	16.992001	17.063	17.104	17.011999	16.919001	16.643999	16.399	13.752	15.734	15.881	13.762	11.467	13.345	15.153	17.098	14.573	12.563	11.229	11.604	12.190	Europe & Central Asia	Upper middle income
4	Andorra	AND	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	Europe & Central Asia	High income
In [56]:
merged_data.columns
Out[56]:
Index(['Country Name', 'Country Code', '1991', '1992', '1993', '1994', '1995',
       '1996', '1997', '1998', '1999', '2000', '2001', '2002', '2003', '2004',
       '2005', '2006', '2007', '2008', '2009', '2010', '2011', '2012', '2013',
       '2014', '2015', '2016', '2017', '2018', '2019', '2020', 'Region',
       'IncomeGroup'],
      dtype='object')
In [57]:
merged_data=merged_data[['Country Name', 'Country Code', '1991', '1992', '1993', '1994', '1995',
       '1996', '1997', '1998', '1999', '2000', '2001', '2002', '2003', '2004',
       '2005', '2006', '2007', '2008', '2009', '2010', '2011', '2012', '2013',
       '2014', '2015', '2016', '2017', '2018', '2019', '2020', 'Region',
       'IncomeGroup']]
merged_data.head()
Out[57]:
Country Name	Country Code	1991	1992	1993	1994	1995	1996	1997	1998	1999	2000	2001	2002	2003	2004	2005	2006	2007	2008	2009	2010	2011	2012	2013	2014	2015	2016	2017	2018	2019	2020	Region	IncomeGroup
0	Aruba	ABW	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	Latin America & Caribbean	High income
1	Afghanistan	AFG	14.226	14.348000	14.391	14.515000	14.803000	14.505	14.699000	14.710	14.794	14.733000	14.702000	15.036	14.859	14.877000	14.910000	14.431000	14.724	14.154	14.911	14.815	14.781	14.820	14.680	14.505	14.427	14.314	14.090	13.906	14.004	14.062	South Asia	Low income
2	Angola	AGO	2.637	2.723000	2.695	2.882000	2.948000	2.994	2.876000	2.967	2.932	2.798000	2.811000	2.899	2.833	2.882000	2.852000	2.746000	2.723	2.710	2.845	10.922	7.718	7.788	7.772	7.719	7.681	7.563	7.467	7.327	6.942	6.631	Sub-Saharan Africa	Lower middle income
3	Albania	ALB	15.804	16.247999	16.711	16.749001	16.766001	16.739	16.434999	16.829	16.916	16.898001	16.992001	17.063	17.104	17.011999	16.919001	16.643999	16.399	13.752	15.734	15.881	13.762	11.467	13.345	15.153	17.098	14.573	12.563	11.229	11.604	12.190	Europe & Central Asia	Upper middle income
4	Andorra	AND	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	NaN	Europe & Central Asia	High income
Exploratory Data Analysis
Removing all the NAN
In [58]:
merged_data.isna()
Out[58]:
Country Name	Country Code	1991	1992	1993	1994	1995	1996	1997	1998	1999	2000	2001	2002	2003	2004	2005	2006	2007	2008	2009	2010	2011	2012	2013	2014	2015	2016	2017	2018	2019	2020	Region	IncomeGroup
0	False	False	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	False	False
1	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False
2	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False
3	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False
4	False	False	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	False	False
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
258	False	False	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	True	False	False
259	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False
260	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False
261	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False
262	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False	False
263 rows × 34 columns

In [59]:
merged_data.isna().sum()
Out[59]:
Country Name     0
Country Code     0
1991            30
1992            30
1993            30
1994            30
1995            30
1996            30
1997            30
1998            30
1999            30
2000            30
2001            30
2002            30
2003            30
2004            30
2005            30
2006            30
2007            30
2008            30
2009            30
2010            30
2011            30
2012            30
2013            30
2014            30
2015            30
2016            30
2017            30
2018            30
2019            30
2020            30
Region          46
IncomeGroup     46
dtype: int64
In [60]:
merged_data.isna().shape
Out[60]:
(263, 34)
In [61]:
merged_data_clean = merged_data.dropna()
merged_data_clean
Out[61]:
Country Name	Country Code	1991	1992	1993	1994	1995	1996	1997	1998	1999	2000	2001	2002	2003	2004	2005	2006	2007	2008	2009	2010	2011	2012	2013	2014	2015	2016	2017	2018	2019	2020	Region	IncomeGroup
1	Afghanistan	AFG	14.226000	14.348000	14.391000	14.515000	14.803000	14.505000	14.699000	14.710	14.794000	14.733000	14.702000	15.036000	14.859	14.877000	14.910000	14.431000	14.724000	14.154000	14.911000	14.815000	14.781000	14.820	14.680000	14.505	14.427000	14.314000	14.090000	13.906000	14.004000	14.062000	South Asia	Low income
2	Angola	AGO	2.637000	2.723000	2.695000	2.882000	2.948000	2.994000	2.876000	2.967	2.932000	2.798000	2.811000	2.899000	2.833	2.882000	2.852000	2.746000	2.723000	2.710000	2.845000	10.922000	7.718000	7.788	7.772000	7.719	7.681000	7.563000	7.467000	7.327000	6.942000	6.631000	Sub-Saharan Africa	Lower middle income
3	Albania	ALB	15.804000	16.247999	16.711000	16.749001	16.766001	16.739000	16.434999	16.829	16.916000	16.898001	16.992001	17.063000	17.104	17.011999	16.919001	16.643999	16.399000	13.752000	15.734000	15.881000	13.762000	11.467	13.345000	15.153	17.098000	14.573000	12.563000	11.229000	11.604000	12.190000	Europe & Central Asia	Upper middle income
6	United Arab Emirates	ARE	2.431000	2.115000	2.259000	2.259000	2.359000	2.441000	2.501000	2.513	2.529000	2.718000	3.355000	3.978000	5.107	6.390000	7.221000	6.682000	5.829000	5.419000	5.843000	5.883000	5.983000	5.956	5.851000	5.214	4.703000	4.200000	7.136000	6.187000	6.046000	6.042000	Middle East & North Africa	High income
7	Argentina	ARG	5.747000	6.711000	12.558000	13.927000	22.195999	19.190001	17.631001	14.029	15.147000	16.344999	17.191999	18.830000	17.549	15.789000	13.561000	12.392000	10.544000	9.720000	9.855000	9.196000	8.496000	8.811	8.484000	8.383	8.851000	9.118000	9.464000	10.538000	10.922000	11.487000	Latin America & Caribbean	Upper middle income
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
257	Samoa	WSM	2.667000	3.014000	3.278000	3.656000	3.952000	4.296000	4.505000	4.889	5.232000	5.700000	6.095000	6.167000	6.235	6.207000	6.279000	6.088000	6.026000	6.120000	6.236000	6.508000	6.691000	10.345	10.355000	10.350	10.265000	10.293000	9.970000	9.806000	9.837000	9.797000	East Asia & Pacific	Upper middle income
259	Yemen, Rep.	YEM	3.728000	3.832000	3.833000	3.877000	4.561000	5.341000	6.207000	7.146	8.157000	8.936000	9.696000	10.629000	11.572	12.460000	13.378000	14.102000	15.009000	16.077999	17.962999	19.591999	20.389999	22.830	24.614000	26.115	25.295000	25.466999	25.469000	25.306999	24.879999	24.462000	Middle East & North Africa	Low income
260	South Africa	ZAF	35.853001	35.848000	36.375999	36.492001	36.417000	36.494999	36.243000	36.069	36.351002	36.330002	36.787998	41.546001	39.480	36.366001	36.550999	35.971001	33.179001	25.836000	25.590000	27.032000	27.247000	27.087	26.547001	27.045	27.542999	29.006001	29.297001	29.059999	30.334999	30.809999	Sub-Saharan Africa	Upper middle income
261	Zambia	ZMB	22.099001	21.347000	20.601000	18.608000	17.459999	15.826000	13.489000	11.494	11.612000	11.432000	12.078000	12.867000	13.739	14.416000	15.054000	12.345000	9.842000	7.705000	10.413000	13.275000	10.429000	8.007	8.864000	9.731	10.687000	11.629000	12.570000	12.448000	12.237000	11.981000	Sub-Saharan Africa	Lower middle income
262	Zimbabwe	ZWE	2.791000	2.819000	2.940000	2.955000	3.542000	4.286000	5.000000	4.791	4.596000	4.633000	4.664000	4.679000	4.629	4.525000	4.639000	4.711000	4.781000	4.919000	5.508000	5.793000	5.990000	5.984	5.984000	5.947	5.895000	5.841000	5.739000	5.598000	5.458000	5.522000	Sub-Saharan Africa	Lower middle income
187 rows × 34 columns

In [62]:
merged_data_clean.shape
Out[62]:
(187, 34)
In [63]:
grouped_data_income = merged_data_clean.groupby(['IncomeGroup']).mean()
grouped_data_income
Out[63]:
1991	1992	1993	1994	1995	1996	1997	1998	1999	2000	2001	2002	2003	2004	2005	2006	2007	2008	2009	2010	2011	2012	2013	2014	2015	2016	2017	2018	2019	2020
IncomeGroup																														
High income	7.736467	8.250783	9.243850	9.437617	9.237233	9.212950	8.843950	8.693183	8.703783	8.479150	8.252700	8.419200	8.672367	8.563100	8.345683	7.703300	7.011633	6.799433	8.279067	8.769983	8.888267	9.132533	9.244683	8.827083	8.370100	7.900950	7.301283	6.753733	6.569350	6.594800
Low income	5.191034	5.122586	5.241103	5.521621	5.722138	5.862035	5.939793	6.142310	6.236034	6.476931	6.796276	6.910207	6.998379	7.046828	7.092172	7.077345	7.154931	7.056069	7.421069	7.645621	7.609897	7.646276	7.719931	7.709862	7.734690	7.701448	7.599138	7.491828	7.458897	7.440862
Lower middle income	7.770750	7.832417	8.115313	8.255917	8.423167	8.522042	8.436583	8.552500	8.596146	8.522167	8.632187	8.800479	8.815729	8.629063	8.552938	8.074896	7.795771	7.745271	8.212021	8.366792	8.326208	8.061479	8.041271	7.941438	8.267479	8.405146	8.481042	8.283375	8.333917	8.284333
Upper middle income	11.855680	11.943880	12.369380	12.909100	13.475140	13.965980	14.108920	14.097900	14.203700	13.907960	13.776640	14.130820	13.996420	13.734020	13.419940	12.613340	11.910920	11.348980	12.007120	12.239020	12.157240	12.124720	12.233620	12.294740	12.246720	12.158600	11.795040	11.403020	11.610500	11.690940
In [64]:
grouped_data_income_describe = merged_data_clean.groupby(['IncomeGroup']).describe()
grouped_data_income_describe
Out[64]:
1991	1992	1993	1994	1995	...	2016	2017	2018	2019	2020
count	mean	std	min	25%	50%	75%	max	count	mean	std	min	25%	50%	75%	max	count	mean	std	min	25%	50%	75%	max	count	mean	std	min	25%	50%	75%	max	count	mean	std	min	25%	50%	75%	max	...	count	mean	std	min	25%	50%	75%	max	count	mean	std	min	25%	50%	75%	max	count	mean	std	min	25%	50%	75%	max	count	mean	std	min	25%	50%	75%	max	count	mean	std	min	25%	50%	75%	max
IncomeGroup																																																																																	
High income	60.0	7.736467	5.462825	0.950	3.31900	6.4480	10.781250	23.796000	60.0	8.250783	5.555074	0.941	3.86550	7.2745	10.7155	25.673000	60.0	9.243850	5.694475	1.941	5.15075	8.0525	12.882000	28.754999	60.0	9.437617	5.917561	1.701	5.13950	8.0965	12.88925	31.612000	60.0	9.237233	5.574981	1.685	4.96625	8.2605	12.103750	30.554001	...	60.0	7.900950	5.015357	0.717	4.76175	6.548	9.86525	28.143000	60.0	7.301283	4.628991	0.639	4.31700	5.7610	9.37525	26.115000	60.0	6.753733	4.506657	0.479	3.91675	5.4690	8.83825	24.290001	60.0	6.569350	4.338601	0.432	3.60575	5.2840	8.182500	22.114000	60.0	6.594800	4.264579	0.385	3.75375	5.3650	8.22600	22.454000
Low income	29.0	5.191034	5.147265	0.149	2.12800	3.1830	6.633000	20.309999	29.0	5.122586	4.996712	0.197	2.20300	3.1860	6.6590	20.263000	29.0	5.241103	4.894268	0.248	2.28600	3.3970	6.455000	20.337999	29.0	5.521621	5.044311	0.287	2.29500	3.4060	7.74100	20.386000	29.0	5.722138	5.205193	0.347	2.27900	3.5820	8.388000	20.440001	...	29.0	7.701448	7.734406	0.406	2.32900	3.708	11.26200	29.875000	29.0	7.599138	7.687203	0.387	2.27500	3.6240	11.11300	29.513000	29.0	7.491828	7.620008	0.369	2.21100	3.5520	11.00800	29.125999	29.0	7.458897	7.446798	0.360	2.22400	3.5650	11.079000	27.768999	29.0	7.440862	7.336123	0.363	2.22700	3.5800	11.13500	27.171000
Lower middle income	48.0	7.770750	8.743691	0.313	1.90375	4.7465	10.250750	45.993999	48.0	7.832417	8.727702	0.343	2.40075	4.6000	10.4555	46.556000	48.0	8.115313	8.898775	0.301	2.59600	4.6410	10.475750	46.983002	48.0	8.255917	8.910337	0.338	2.82550	5.0400	10.61950	47.308998	48.0	8.423167	9.070520	0.334	2.97425	4.9965	10.513500	47.395000	...	48.0	8.405146	8.068344	0.625	3.15525	5.815	10.61175	37.929001	48.0	8.481042	8.467212	0.605	3.29375	5.6260	10.39750	42.769001	48.0	8.283375	8.379838	0.585	3.00875	5.5540	10.28800	41.849998	48.0	8.333917	8.201198	0.569	3.48350	5.6590	10.179250	40.945000	48.0	8.284333	8.057929	0.547	3.49675	5.7435	10.03575	40.616001
Upper middle income	50.0	11.855680	10.078790	0.118	3.63400	8.7430	16.647749	39.773998	50.0	11.943880	10.088228	0.443	4.11700	7.9385	18.1920	40.058998	50.0	12.369380	9.840402	0.442	4.89500	9.7990	18.579999	40.325001	50.0	12.909100	9.476483	1.334	6.03175	10.4980	16.69925	40.691002	50.0	13.475140	9.391157	1.311	6.55975	11.2455	17.850501	40.675999	...	50.0	12.158600	8.192098	0.714	5.51200	10.785	17.34325	29.976999	50.0	11.795040	8.160169	0.845	5.13225	10.0515	16.93575	31.020000	50.0	11.403020	7.903636	0.743	4.68400	10.2675	16.34250	30.851000	50.0	11.610500	7.967784	0.742	4.62200	10.3435	16.614751	30.403999	50.0	11.690940	7.991920	0.791	4.83225	10.2210	15.78975	30.809999
4 rows × 240 columns

In [65]:
transpose_income_group = grouped_data_income.transpose()
transpose_income_group
Out[65]:
IncomeGroup	High income	Low income	Lower middle income	Upper middle income
1991	7.736467	5.191034	7.770750	11.85568
1992	8.250783	5.122586	7.832417	11.94388
1993	9.243850	5.241103	8.115313	12.36938
1994	9.437617	5.521621	8.255917	12.90910
1995	9.237233	5.722138	8.423167	13.47514
1996	9.212950	5.862035	8.522042	13.96598
1997	8.843950	5.939793	8.436583	14.10892
1998	8.693183	6.142310	8.552500	14.09790
1999	8.703783	6.236034	8.596146	14.20370
2000	8.479150	6.476931	8.522167	13.90796
2001	8.252700	6.796276	8.632187	13.77664
2002	8.419200	6.910207	8.800479	14.13082
2003	8.672367	6.998379	8.815729	13.99642
2004	8.563100	7.046828	8.629063	13.73402
2005	8.345683	7.092172	8.552938	13.41994
2006	7.703300	7.077345	8.074896	12.61334
2007	7.011633	7.154931	7.795771	11.91092
2008	6.799433	7.056069	7.745271	11.34898
2009	8.279067	7.421069	8.212021	12.00712
2010	8.769983	7.645621	8.366792	12.23902
2011	8.888267	7.609897	8.326208	12.15724
2012	9.132533	7.646276	8.061479	12.12472
2013	9.244683	7.719931	8.041271	12.23362
2014	8.827083	7.709862	7.941438	12.29474
2015	8.370100	7.734690	8.267479	12.24672
2016	7.900950	7.701448	8.405146	12.15860
2017	7.301283	7.599138	8.481042	11.79504
2018	6.753733	7.491828	8.283375	11.40302
2019	6.569350	7.458897	8.333917	11.61050
2020	6.594800	7.440862	8.284333	11.69094
In [66]:
import matplotlib.pyplot as plt
%matplotlib inline
In [67]:
transpose_income_group.plot(figsize=(20,10))
plt.title("Mean % Female Unemployment")
plt.xlabel("Year")
plt.ylabel("% UnEmployment")
Out[67]:
Text(0, 0.5, '% UnEmployment')

In [68]:
grouped_data_region = merged_data_clean.groupby(['Region']).describe()
grouped_data_region = merged_data_clean.groupby(['Region']).mean()
transposed_region = grouped_data_region.transpose()
transposed_region.plot(figsize=(20,10))
plt.title('Mean % Female Unemployment by Region')
plt.xlabel('Year')
plt.ylabel("% UnEmployment")
Out[68]:
Text(0, 0.5, '% UnEmployment')

ANOVA
In [69]:
import scipy.stats as stats
In [70]:
merged_data_clean.head()
Out[70]:
Country Name	Country Code	1991	1992	1993	1994	1995	1996	1997	1998	1999	2000	2001	2002	2003	2004	2005	2006	2007	2008	2009	2010	2011	2012	2013	2014	2015	2016	2017	2018	2019	2020	Region	IncomeGroup
1	Afghanistan	AFG	14.226	14.348000	14.391	14.515000	14.803000	14.505000	14.699000	14.710	14.794	14.733000	14.702000	15.036	14.859	14.877000	14.910000	14.431000	14.724	14.154	14.911	14.815	14.781	14.820	14.680	14.505	14.427	14.314	14.090	13.906	14.004	14.062	South Asia	Low income
2	Angola	AGO	2.637	2.723000	2.695	2.882000	2.948000	2.994000	2.876000	2.967	2.932	2.798000	2.811000	2.899	2.833	2.882000	2.852000	2.746000	2.723	2.710	2.845	10.922	7.718	7.788	7.772	7.719	7.681	7.563	7.467	7.327	6.942	6.631	Sub-Saharan Africa	Lower middle income
3	Albania	ALB	15.804	16.247999	16.711	16.749001	16.766001	16.739000	16.434999	16.829	16.916	16.898001	16.992001	17.063	17.104	17.011999	16.919001	16.643999	16.399	13.752	15.734	15.881	13.762	11.467	13.345	15.153	17.098	14.573	12.563	11.229	11.604	12.190	Europe & Central Asia	Upper middle income
6	United Arab Emirates	ARE	2.431	2.115000	2.259	2.259000	2.359000	2.441000	2.501000	2.513	2.529	2.718000	3.355000	3.978	5.107	6.390000	7.221000	6.682000	5.829	5.419	5.843	5.883	5.983	5.956	5.851	5.214	4.703	4.200	7.136	6.187	6.046	6.042	Middle East & North Africa	High income
7	Argentina	ARG	5.747	6.711000	12.558	13.927000	22.195999	19.190001	17.631001	14.029	15.147	16.344999	17.191999	18.830	17.549	15.789000	13.561000	12.392000	10.544	9.720	9.855	9.196	8.496	8.811	8.484	8.383	8.851	9.118	9.464	10.538	10.922	11.487	Latin America & Caribbean	Upper middle income
Income Groups and the length
In [71]:
income_groups = merged_data_clean['IncomeGroup'].unique()
print(income_groups)
len(income_groups)
['Low income' 'Lower middle income' 'Upper middle income' 'High income']
Out[71]:
4
Verification of Number of countries
In [72]:
from IPython.display import display
with pd.option_context('display.max_rows', 299, 'display.max_columns', 40):
    display(merged_data_clean) #need display to show all data
Country Name	Country Code	1991	1992	1993	1994	1995	1996	1997	1998	1999	2000	2001	2002	2003	2004	2005	2006	2007	2008	2009	2010	2011	2012	2013	2014	2015	2016	2017	2018	2019	2020	Region	IncomeGroup
1	Afghanistan	AFG	14.226000	14.348000	14.391000	14.515000	14.803000	14.505000	14.699000	14.710000	14.794000	14.733000	14.702000	15.036000	14.859000	14.877000	14.910000	14.431000	14.724000	14.154000	14.911000	14.815000	14.781000	14.820000	14.680000	14.505000	14.427000	14.314000	14.090000	13.906000	14.004000	14.062000	South Asia	Low income
2	Angola	AGO	2.637000	2.723000	2.695000	2.882000	2.948000	2.994000	2.876000	2.967000	2.932000	2.798000	2.811000	2.899000	2.833000	2.882000	2.852000	2.746000	2.723000	2.710000	2.845000	10.922000	7.718000	7.788000	7.772000	7.719000	7.681000	7.563000	7.467000	7.327000	6.942000	6.631000	Sub-Saharan Africa	Lower middle income
3	Albania	ALB	15.804000	16.247999	16.711000	16.749001	16.766001	16.739000	16.434999	16.829000	16.916000	16.898001	16.992001	17.063000	17.104000	17.011999	16.919001	16.643999	16.399000	13.752000	15.734000	15.881000	13.762000	11.467000	13.345000	15.153000	17.098000	14.573000	12.563000	11.229000	11.604000	12.190000	Europe & Central Asia	Upper middle income
6	United Arab Emirates	ARE	2.431000	2.115000	2.259000	2.259000	2.359000	2.441000	2.501000	2.513000	2.529000	2.718000	3.355000	3.978000	5.107000	6.390000	7.221000	6.682000	5.829000	5.419000	5.843000	5.883000	5.983000	5.956000	5.851000	5.214000	4.703000	4.200000	7.136000	6.187000	6.046000	6.042000	Middle East & North Africa	High income
7	Argentina	ARG	5.747000	6.711000	12.558000	13.927000	22.195999	19.190001	17.631001	14.029000	15.147000	16.344999	17.191999	18.830000	17.549000	15.789000	13.561000	12.392000	10.544000	9.720000	9.855000	9.196000	8.496000	8.811000	8.484000	8.383000	8.851000	9.118000	9.464000	10.538000	10.922000	11.487000	Latin America & Caribbean	Upper middle income
8	Armenia	ARM	2.188000	2.422000	7.499000	8.613000	10.265000	14.891000	17.125999	13.642000	15.212000	14.492000	13.817000	13.307000	12.666000	11.875000	11.258000	10.467000	9.767000	13.946000	19.681999	21.285000	19.652000	18.181999	18.139000	19.465000	19.252001	17.483999	17.471001	17.198999	17.295000	17.451000	Europe & Central Asia	Upper middle income
11	Australia	AUS	9.148000	9.925000	9.995000	9.360000	8.104000	8.235000	8.070000	7.326000	6.657000	6.060000	6.443000	6.161000	5.979000	5.536000	5.221000	4.923000	4.787000	4.573000	5.400000	5.377000	5.305000	5.326000	5.609000	6.172000	6.073000	5.779000	5.668000	5.318000	5.328000	5.415000	East Asia & Pacific	High income
12	Austria	AUT	3.544000	3.771000	4.500000	3.966000	4.899000	5.205000	5.272000	5.594000	4.756000	4.594000	4.128000	4.526000	4.337000	5.893000	5.858000	5.603000	5.301000	4.411000	5.085000	4.629000	4.584000	4.786000	5.294000	5.382000	5.308000	5.543000	5.031000	4.650000	4.547000	4.706000	Europe & Central Asia	High income
13	Azerbaijan	AZE	0.996000	1.949000	4.863000	6.671000	7.772000	8.855000	10.035000	10.942000	11.950000	12.765000	11.540000	10.370000	9.239000	8.053000	7.353000	6.333000	5.256000	4.931000	6.586000	6.925000	6.451000	6.094000	5.945000	5.839000	5.867000	5.908000	5.927000	5.766000	6.335000	6.762000	Europe & Central Asia	Upper middle income
14	Burundi	BDI	1.278000	1.315000	1.400000	1.405000	1.397000	1.435000	1.410000	1.387000	1.404000	1.386000	1.381000	1.418000	1.419000	1.386000	1.337000	1.246000	1.166000	1.145000	1.280000	1.294000	1.259000	1.237000	1.217000	1.155000	1.126000	1.123000	1.073000	1.033000	1.009000	0.986000	Sub-Saharan Africa	Low income
15	Belgium	BEL	10.652000	9.505000	10.820000	12.429000	12.214000	12.409000	11.546000	11.704000	10.215000	8.282000	6.922000	7.799000	7.996000	8.283000	9.512000	9.314000	8.448000	7.600000	8.106000	8.516000	7.180000	7.409000	8.170000	7.940000	7.768000	7.560000	7.054000	5.562000	5.136000	5.315000	Europe & Central Asia	High income
16	Benin	BEN	0.547000	0.580000	0.617000	0.603000	0.622000	0.583000	0.572000	0.536000	0.541000	0.497000	0.458000	0.427000	0.501000	0.570000	0.619000	0.677000	0.724000	0.781000	0.991000	1.146000	2.872000	2.884000	2.919000	2.835000	2.776000	2.748000	2.664000	2.567000	2.336000	2.060000	Sub-Saharan Africa	Lower middle income
17	Burkina Faso	BFA	2.631000	2.569000	2.698000	2.685000	2.689000	2.667000	2.583000	2.585000	2.698000	2.733000	2.878000	2.992000	3.146000	3.795000	4.620000	4.223000	3.928000	4.463000	5.368000	6.209000	6.898000	7.750000	8.682000	9.506000	9.388000	9.320000	9.130000	8.950000	9.223000	9.465000	Sub-Saharan Africa	Low income
18	Bangladesh	BGD	2.082000	2.121000	2.115000	2.122000	2.118000	2.116000	2.382000	2.646000	3.005000	3.332000	3.715000	4.248000	4.841000	5.951000	7.084000	6.494000	6.591000	6.734000	7.385000	4.440000	5.395000	6.432000	7.609000	7.522000	7.467000	7.379000	6.745000	6.623000	6.204000	5.977000	South Asia	Lower middle income
19	Bulgaria	BGR	13.123000	13.238000	13.314000	13.445000	13.507000	13.407000	13.489000	11.775000	14.154000	15.812000	18.857000	17.358999	13.191000	11.621000	9.833000	9.301000	7.292000	5.760000	6.621000	9.606000	10.107000	10.839000	11.844000	10.404000	8.416000	6.968000	5.946000	4.653000	3.969000	3.507000	Europe & Central Asia	Upper middle income
20	Bahrain	BHR	3.874000	3.897000	4.943000	4.540000	4.396000	4.249000	3.973000	4.027000	4.813000	4.485000	4.394000	3.770000	3.943000	4.210000	3.602000	3.172000	2.692000	2.598000	3.709000	3.704000	4.023000	3.894000	4.209000	3.893000	3.745000	3.280000	2.792000	2.656000	2.963000	3.218000	Middle East & North Africa	High income
21	Bahamas, The	BHS	12.100000	15.854000	13.032000	15.243000	11.758000	14.700000	11.387000	9.851000	9.299000	8.021000	7.031000	9.515000	11.800000	11.035000	11.201000	8.057000	9.107000	9.261000	14.035000	15.054000	15.362000	13.968000	16.259001	14.641000	13.004000	14.483000	11.391000	9.948000	10.801000	11.854000	Latin America & Caribbean	High income
22	Bosnia and Herzegovina	BIH	21.281000	22.247999	23.260000	24.559000	25.451000	26.624001	27.228001	28.110001	28.975000	29.695000	30.593000	31.816000	32.723999	33.605000	34.344002	34.868999	32.948002	26.797001	25.648001	29.931000	29.926001	30.649000	28.915001	31.118999	30.653999	29.976999	23.099001	20.294001	21.002001	21.333000	Europe & Central Asia	Upper middle income
23	Belarus	BLR	0.118000	1.892000	6.757000	10.721000	15.384000	22.198000	15.128000	12.038000	10.809000	10.124000	9.388000	8.784000	8.173000	7.548000	6.834000	6.154000	5.497000	4.993000	4.629000	4.741000	4.634000	4.552000	4.532000	4.423000	4.262000	4.186000	4.039000	3.554000	3.350000	3.352000	Europe & Central Asia	Upper middle income
24	Belize	BLZ	15.028000	14.397000	14.377000	15.723000	18.058001	18.667999	20.756001	21.794001	20.629000	18.291000	15.505000	15.165000	16.040001	16.530001	17.205999	15.073000	13.124000	12.795000	12.948000	12.956000	12.810000	12.726000	12.582000	14.009000	11.065000	11.115000	9.801000	9.721000	9.653000	9.542000	Latin America & Caribbean	Upper middle income
26	Bolivia	BOL	3.504000	3.431000	3.662000	3.654000	3.659000	3.668000	3.695000	3.704000	3.571000	3.671000	3.597000	3.697000	3.685000	3.670000	3.610000	3.485000	3.375000	3.396000	3.457000	3.077000	2.670000	2.802000	2.973000	2.847000	3.797000	4.075000	4.071000	3.629000	3.809000	3.837000	Latin America & Caribbean	Lower middle income
27	Brazil	BRA	8.169000	8.098000	7.446000	7.818000	8.113000	9.567000	10.874000	12.597000	13.321000	12.886000	12.222000	11.861000	12.664000	11.987000	12.535000	11.375000	11.135000	9.947000	11.365000	10.636000	9.439000	9.059000	8.771000	8.114000	10.055000	13.458000	14.718000	14.230000	14.139000	14.292000	Latin America & Caribbean	Upper middle income
28	Barbados	BRB	23.796000	25.673000	27.716999	27.976000	22.862000	17.768999	18.025999	16.540001	13.478000	11.391000	11.811000	12.207000	12.557000	10.451000	10.799000	9.814000	8.431000	9.430000	9.887000	10.472000	12.629000	12.263000	11.433000	12.652000	10.322000	10.103000	10.042000	9.600000	10.692000	11.415000	Latin America & Caribbean	High income
29	Brunei Darussalam	BRN	6.712000	7.120000	7.493000	7.596000	7.775000	7.664000	7.364000	7.271000	7.434000	7.308000	7.365000	7.551000	7.595000	7.314000	7.006000	6.765000	6.488000	6.286000	7.537000	7.967000	8.065000	8.215000	8.226000	7.902000	8.798000	9.490000	9.985000	9.642000	9.907000	9.979000	East Asia & Pacific	High income
30	Bhutan	BTN	1.267000	1.303000	1.355000	1.375000	1.418000	1.414000	1.398000	1.400000	1.635000	2.190000	2.871000	2.766000	2.566000	3.308000	3.330000	3.798000	4.733000	4.948000	5.535000	4.054000	4.875000	2.215000	3.765000	3.610000	3.245000	3.227000	3.158000	3.071000	3.200000	3.304000	South Asia	Lower middle income
31	Botswana	BWA	16.924999	18.840000	21.184999	22.976999	23.764999	24.114000	23.959999	23.650999	20.580999	17.250999	22.069000	24.480000	26.485001	24.302999	22.237000	20.176001	19.860001	19.655001	19.707001	21.535999	21.301001	21.275000	21.351999	20.971001	20.844000	20.905001	20.649000	20.521000	21.222000	21.760000	Sub-Saharan Africa	Upper middle income
32	Central African Republic	CAF	3.624000	3.661000	3.762000	3.793000	3.782000	3.759000	3.797000	3.770000	3.833000	3.802000	3.808000	3.864000	3.848000	3.825000	3.750000	3.619000	3.500000	3.471000	3.744000	3.821000	3.816000	3.847000	3.798000	3.916000	3.769000	3.708000	3.624000	3.552000	3.565000	3.580000	Sub-Saharan Africa	Low income
33	Canada	CAN	9.687000	10.188000	10.674000	9.793000	9.113000	9.287000	8.918000	7.986000	7.292000	6.694000	6.855000	7.123000	7.156000	6.852000	6.463000	6.092000	5.648000	5.649000	7.007000	7.233000	7.007000	6.820000	6.596000	6.389000	6.285000	6.238000	5.849000	5.492000	5.263000	5.064000	North America	High income
35	Switzerland	CHE	2.517000	3.529000	4.567000	4.422000	3.920000	4.084000	3.989000	4.135000	3.568000	3.130000	3.457000	3.096000	4.486000	4.763000	5.091000	4.709000	4.513000	3.966000	4.520000	5.188000	4.800000	4.729000	4.918000	4.986000	4.912000	5.006000	5.058000	5.109000	4.768000	4.975000	Europe & Central Asia	High income
36	Channel Islands	CHI	7.909000	8.055000	8.350000	8.394000	8.417000	8.362000	8.310000	8.218000	8.244000	8.236000	8.100000	8.013000	8.024000	7.944000	7.890000	7.694000	7.527000	7.576000	8.371000	8.566000	8.566000	8.664000	8.727000	8.515000	8.326000	8.110000	7.884000	7.681000	7.695000	7.665000	Europe & Central Asia	High income
37	Chile	CHL	5.691000	5.320000	5.105000	6.777000	5.239000	9.967000	9.559000	9.321000	13.088000	12.460000	11.916000	11.790000	12.002000	12.982000	12.251000	11.733000	10.862000	12.100000	13.401000	9.909000	8.862000	8.130000	7.174000	7.124000	7.137000	7.261000	7.455000	7.979000	7.702000	7.793000	Latin America & Caribbean	High income
38	China	CHN	2.060000	2.063000	2.335000	2.517000	2.610000	2.709000	2.809000	2.817000	2.826000	2.833000	3.302000	3.685000	3.979000	3.902000	3.918000	3.844000	3.770000	3.978000	4.092000	3.923000	3.942000	3.960000	3.978000	3.994000	4.012000	3.929000	3.847000	3.703000	3.727000	3.761000	East Asia & Pacific	Upper middle income
39	Cote d'Ivoire	CIV	6.402000	6.446000	5.633000	4.811000	4.129000	4.133000	4.098000	4.101000	4.260000	4.393000	4.638000	4.822000	5.080000	5.271000	5.429000	5.465000	5.569000	5.764000	6.288000	6.615000	6.678000	7.343000	5.212000	4.549000	3.948000	3.353000	3.858000	3.795000	3.772000	3.788000	Sub-Saharan Africa	Lower middle income
40	Cameroon	CMR	6.145000	6.198000	6.323000	6.361000	6.487000	6.515000	6.525000	6.529000	6.597000	6.641000	6.696000	6.279000	5.788000	5.292000	4.607000	4.059000	3.508000	3.831000	4.427000	4.899000	4.682000	4.478000	4.293000	4.048000	4.006000	3.961000	3.884000	3.841000	3.872000	3.936000	Sub-Saharan Africa	Lower middle income
41	Congo, Dem. Rep.	COD	2.128000	2.203000	2.227000	2.215000	2.223000	2.292000	2.334000	2.353000	2.263000	2.243000	2.233000	2.260000	2.267000	2.236000	2.195000	2.288000	2.380000	2.516000	2.928000	3.192000	3.371000	3.608000	3.641000	3.612000	3.562000	3.488000	3.426000	3.353000	3.404000	3.423000	Sub-Saharan Africa	Low income
42	Congo, Rep.	COG	20.514999	20.639000	20.844999	21.042999	21.193001	21.120001	20.849001	21.058001	20.968000	21.228001	21.069000	21.223000	21.153999	21.120001	21.076000	20.028999	19.028000	18.413000	18.334999	15.556000	12.891000	10.707000	10.735000	10.864000	10.767000	10.582000	10.361000	10.265000	10.105000	9.908000	Sub-Saharan Africa	Lower middle income
43	Colombia	COL	13.802000	13.364000	11.335000	12.536000	11.485000	15.120000	15.312000	18.306000	23.716999	25.065001	19.372000	19.987000	18.627001	17.995001	15.863000	15.386000	14.805000	14.763000	15.831000	14.189000	13.095000	12.657000	11.666000	11.028000	10.840000	11.204000	11.513000	11.794000	12.713000	12.699000	Latin America & Caribbean	Upper middle income
44	Comoros	COM	4.653000	4.900000	4.834000	4.898000	4.995000	4.858000	4.932000	4.866000	4.933000	4.908000	4.916000	4.965000	4.950000	4.894000	4.867000	4.677000	4.590000	4.576000	4.833000	4.910000	4.908000	4.935000	4.969000	4.890000	4.860000	4.838000	4.748000	4.666000	4.702000	4.750000	Sub-Saharan Africa	Lower middle income
45	Cabo Verde	CPV	10.541000	10.655000	10.946000	10.963000	10.962000	10.953000	10.990000	10.949000	11.148000	10.931000	10.955000	11.058000	11.110000	11.017000	10.982000	10.885000	10.832000	10.701000	10.836000	11.121000	11.460000	11.570000	11.857000	12.104000	12.363000	12.669000	12.769000	11.548000	11.491000	11.476000	Sub-Saharan Africa	Lower middle income
46	Costa Rica	CRI	7.102000	5.310000	4.991000	5.574000	6.512000	8.251000	7.346000	7.571000	7.924000	6.640000	7.388000	7.762000	7.995000	8.291000	9.317000	8.355000	6.755000	5.877000	9.594000	9.114000	12.724000	11.613000	10.409000	11.248000	11.153000	10.839000	10.293000	12.192000	14.846000	15.910000	Latin America & Caribbean	Upper middle income
48	Cuba	CUB	12.445000	12.629000	12.876000	13.212000	13.207000	12.504000	11.332000	9.465000	9.767000	8.361000	5.789000	4.652000	3.423000	2.207000	2.209000	2.215000	1.871000	1.962000	1.989000	2.708000	3.510000	3.613000	3.547000	3.133000	2.582000	2.182000	1.637000	1.825000	1.776000	1.728000	Latin America & Caribbean	Upper middle income
51	Cyprus	CYP	2.209000	2.230000	2.984000	3.419000	4.496000	5.134000	5.620000	6.782000	7.829000	7.447000	5.816000	4.214000	4.600000	5.478000	6.534000	5.434000	4.598000	4.253000	5.472000	6.421000	7.624000	11.023000	15.129000	15.019000	14.766000	13.333000	11.269000	8.755000	8.122000	8.247000	Europe & Central Asia	High income
52	Czech Republic	CZE	2.065000	3.261000	5.443000	5.192000	4.807000	4.661000	5.123000	7.555000	10.127000	10.536000	9.612000	8.558000	9.645000	9.652000	9.790000	8.842000	6.736000	5.609000	7.727000	8.455000	7.895000	8.206000	8.287000	7.395000	6.074000	4.670000	3.579000	2.803000	2.384000	2.341000	Europe & Central Asia	High income
53	Germany	DEU	6.538000	8.241000	9.327000	10.319000	9.633000	9.596000	10.593000	10.440000	9.236000	8.273000	7.780000	8.214000	9.281000	10.076000	10.887000	10.173000	8.795000	7.649000	7.309000	6.494000	5.580000	5.176000	4.914000	4.629000	4.220000	3.741000	3.315000	2.910000	2.694000	2.656000	Europe & Central Asia	High income
54	Djibouti	DJI	10.154000	10.389000	10.319000	10.505000	10.364000	10.408000	10.462000	10.494000	10.596000	10.508000	10.536000	10.675000	10.721000	10.655000	10.584000	10.514000	10.397000	10.377000	10.606000	10.658000	10.750000	10.703000	10.880000	10.760000	10.837000	10.730000	10.507000	10.357000	10.402000	10.432000	Middle East & North Africa	Lower middle income
56	Denmark	DNK	10.017000	9.864000	11.085000	9.009000	8.656000	8.408000	6.451000	6.413000	5.902000	4.974000	4.783000	4.318000	5.750000	5.439000	5.278000	4.535000	4.199000	3.735000	5.314000	6.463000	7.466000	7.546000	7.272000	6.820000	6.444000	6.639000	5.936000	5.149000	5.194000	5.138000	Europe & Central Asia	High income
57	Dominican Republic	DOM	10.658000	10.892000	10.809000	10.759000	10.852000	10.821000	10.830000	10.757000	10.788000	10.719000	11.282000	10.188000	10.067000	10.058000	9.746000	8.924000	7.607000	7.424000	7.922000	7.068000	8.379000	9.251000	10.565000	9.714000	10.905000	10.731000	8.122000	8.075000	8.279000	8.415000	Latin America & Caribbean	Upper middle income
58	Algeria	DZA	16.606001	20.558001	22.551001	25.139000	30.298000	28.357000	26.167999	27.761999	28.671000	29.718000	27.747000	27.148001	25.443001	18.205999	18.034000	16.055000	20.023001	18.253000	18.086000	19.090000	17.141001	17.007000	16.271000	15.635000	16.674999	18.629999	21.114000	21.058001	21.080000	20.451000	Middle East & North Africa	Lower middle income
64	Ecuador	ECU	6.160000	6.163000	6.266000	6.352000	6.284000	6.297000	6.362000	6.312000	6.159000	6.387000	5.241000	5.767000	6.161000	5.717000	4.714000	4.374000	3.919000	5.010000	5.886000	4.969000	4.290000	3.823000	3.666000	4.146000	4.524000	5.849000	4.929000	4.400000	4.958000	5.229000	Latin America & Caribbean	Upper middle income
65	Egypt, Arab Rep.	EGY	21.334000	17.312000	22.396999	22.843000	23.714001	18.569000	19.858999	19.709999	19.097000	22.961000	21.985001	24.062000	23.677999	25.378000	25.110001	23.924999	18.424000	18.874001	22.406000	22.118000	22.454000	24.013000	24.156000	23.981001	24.910999	23.683001	23.087000	22.995001	22.150000	21.396999	Middle East & North Africa	Lower middle income
67	Eritrea	ERI	4.913000	4.947000	5.176000	5.204000	5.135000	5.255000	5.233000	5.223000	5.355000	5.226000	5.375000	5.303000	5.341000	5.285000	5.209000	5.020000	4.906000	4.848000	5.201000	5.229000	5.256000	5.247000	5.273000	5.211000	5.172000	5.120000	5.031000	4.923000	4.953000	4.980000	Sub-Saharan Africa	Low income
68	Spain	ESP	23.250999	25.257000	28.754999	31.612000	30.554001	29.552000	28.014000	26.648001	22.854000	20.368000	15.062000	16.066999	15.702000	15.052000	11.993000	11.351000	10.701000	12.840000	18.127001	20.225000	21.813000	25.034000	26.673000	25.427999	23.551001	21.385000	19.035000	17.033001	16.204000	15.550000	Europe & Central Asia	High income
69	Estonia	EST	1.527000	3.464000	6.640000	7.930000	8.863000	9.226000	9.539000	8.504000	10.129000	11.607000	13.237000	8.676000	10.380000	8.828000	6.905000	5.585000	3.755000	5.136000	10.294000	14.087000	11.577000	9.099000	8.155000	6.812000	6.134000	6.066000	5.299000	5.318000	4.966000	5.145000	Europe & Central Asia	High income
70	Ethiopia	ETH	3.118000	3.182000	3.334000	3.269000	3.582000	3.938000	4.186000	4.547000	4.993000	4.668000	4.366000	4.148000	3.886000	3.630000	3.282000	3.118000	2.963000	2.882000	3.044000	3.047000	3.022000	2.994000	2.951000	2.924000	2.901000	2.857000	2.811000	2.746000	2.756000	2.763000	Sub-Saharan Africa	Low income
73	Finland	FIN	5.031000	9.644000	14.416000	14.870000	16.150999	15.555000	15.176000	13.582000	12.447000	11.994000	10.760000	10.170000	9.938000	10.576000	8.614000	8.083000	7.192000	6.690000	7.572000	7.640000	7.103000	7.053000	7.517000	7.953000	8.842000	8.582000	8.389000	7.298000	6.296000	6.324000	Europe & Central Asia	High income
74	Fiji	FJI	4.815000	5.131000	5.109000	5.206000	5.139000	5.254000	5.069000	5.290000	5.472000	5.102000	5.276000	5.375000	5.336000	5.415000	5.161000	5.027000	4.685000	4.657000	4.691000	4.754000	4.615000	4.791000	5.188000	5.293000	5.452000	5.562000	5.553000	5.451000	5.320000	5.272000	East Asia & Pacific	Upper middle income
75	France	FRA	11.642000	12.865000	13.430000	14.508000	14.081000	14.511000	14.441000	14.130000	13.917000	12.210000	10.521000	9.803000	9.136000	9.881000	9.274000	9.115000	8.089000	7.444000	8.802000	9.067000	9.117000	9.358000	9.789000	10.028000	9.881000	9.852000	9.336000	9.097000	8.364000	8.219000	Europe & Central Asia	High income
78	Gabon	GAB	15.816000	15.544000	16.013000	16.549999	17.228001	17.757999	18.584999	19.190001	19.299999	20.202999	20.955999	21.584999	22.216999	22.632000	22.986000	23.724001	24.778000	25.483999	27.299999	28.965000	28.862000	28.886999	29.014999	28.851000	28.718000	28.555000	28.266001	28.101000	28.507000	28.743999	Sub-Saharan Africa	Upper middle income
79	United Kingdom	GBR	7.401000	7.429000	7.755000	7.398000	6.926000	6.332000	5.807000	5.361000	5.149000	4.858000	4.111000	4.360000	4.062000	4.189000	4.291000	4.944000	4.959000	5.084000	6.417000	6.872000	7.338000	7.384000	7.037000	5.820000	5.124000	4.683000	4.220000	3.940000	3.575000	3.865000	Europe & Central Asia	High income
80	Georgia	GEO	1.918000	4.438000	4.451000	7.420000	6.719000	10.732000	10.674000	13.717000	12.142000	10.499000	10.643000	10.993000	11.542000	11.762000	12.669000	11.646000	12.556000	17.587000	19.056999	17.532000	17.363001	18.337000	16.146000	14.445000	13.901000	12.531000	12.691000	12.547000	12.880000	13.109000	Europe & Central Asia	Upper middle income
81	Ghana	GHA	5.628000	5.657000	6.260000	6.758000	7.283000	7.812000	8.352000	8.827000	10.773000	10.678000	9.563000	8.540000	7.573000	6.637000	5.719000	4.770000	4.810000	5.090000	5.409000	5.807000	6.049000	6.099000	6.326000	6.389000	6.566000	5.433000	4.385000	4.252000	4.464000	4.664000	Sub-Saharan Africa	Lower middle income
83	Guinea	GIN	3.260000	3.310000	3.397000	3.406000	3.435000	3.455000	3.460000	3.452000	3.517000	3.504000	3.505000	3.545000	3.554000	3.544000	3.476000	3.353000	3.255000	3.205000	3.421000	3.531000	3.509000	3.535000	3.536000	3.485000	3.458000	3.493000	3.326000	3.286000	3.316000	3.341000	Sub-Saharan Africa	Low income
84	Gambia, The	GMB	12.273000	12.432000	12.653000	12.606000	12.761000	12.702000	12.667000	12.688000	12.701000	12.645000	12.704000	12.688000	12.875000	12.712000	12.535000	12.416000	12.181000	12.078000	12.521000	12.623000	12.555000	12.788000	12.679000	12.563000	12.584000	12.407000	12.406000	12.240000	12.237000	12.287000	Sub-Saharan Africa	Low income
85	Guinea-Bissau	GNB	2.231000	2.279000	2.360000	2.419000	2.369000	2.379000	2.389000	2.326000	2.484000	2.369000	2.408000	2.433000	2.457000	2.413000	2.364000	2.245000	2.170000	2.139000	2.344000	2.398000	2.399000	2.378000	2.432000	2.384000	2.357000	2.304000	2.239000	2.182000	2.210000	2.227000	Sub-Saharan Africa	Low income
86	Equatorial Guinea	GNQ	6.151000	6.305000	6.316000	6.491000	6.703000	6.982000	6.725000	6.898000	6.801000	6.730000	6.677000	6.714000	6.651000	6.632000	6.401000	6.259000	6.195000	6.148000	6.484000	6.542000	6.609000	6.689000	6.576000	6.538000	6.412000	6.316000	6.208000	6.084000	6.184000	6.273000	Sub-Saharan Africa	Upper middle income
87	Greece	GRC	13.002000	13.045000	13.792000	13.826000	13.943000	15.672000	15.056000	16.809000	18.327999	17.170000	16.017000	15.439000	14.551000	16.058001	15.507000	13.810000	12.941000	11.531000	13.296000	16.350000	21.535999	28.239000	31.375999	30.204000	28.893000	28.143000	26.115000	24.290001	22.016001	20.218000	Europe & Central Asia	High income
90	Guatemala	GTM	3.235000	3.388000	3.364000	3.392000	3.439000	3.336000	3.432000	3.449000	3.433000	3.368000	3.382000	3.509000	3.419000	3.378000	3.400000	3.492000	3.490000	3.334000	3.517000	3.844000	3.736000	3.453000	3.655000	3.380000	3.541000	3.472000	3.522000	3.468000	3.412000	3.401000	Latin America & Caribbean	Upper middle income
91	Guam	GUM	3.443000	3.694000	5.191000	6.940000	7.238000	7.506000	8.659000	7.031000	11.903000	12.984000	11.210000	9.560000	8.712000	7.760000	7.779000	7.580000	9.974000	9.232000	9.899000	9.399000	16.024000	11.505000	10.977000	7.436000	6.763000	5.336000	5.100000	4.881000	5.305000	5.647000	East Asia & Pacific	High income
92	Guyana	GUY	15.031000	15.147000	15.174000	15.130000	15.134000	15.211000	15.207000	15.085000	15.256000	15.150000	15.212000	15.260000	15.009000	14.739000	14.410000	14.055000	13.962000	13.922000	14.519000	14.775000	14.956000	15.109000	15.264000	15.289000	15.321000	15.385000	15.369000	15.225000	15.189000	15.167000	Latin America & Caribbean	Upper middle income
94	Hong Kong SAR, China	HKG	1.589000	1.891000	1.941000	1.701000	2.936000	2.385000	2.074000	4.024000	5.078000	4.014000	3.857000	5.895000	6.184000	5.546000	4.410000	3.723000	3.373000	2.979000	4.327000	3.510000	2.793000	2.761000	2.969000	3.011000	3.191000	3.078000	2.774000	2.616000	3.198000	3.636000	East Asia & Pacific	High income
95	Honduras	HND	5.385000	2.936000	3.161000	3.143000	3.389000	4.415000	3.320000	4.349000	3.793000	4.148000	4.079000	4.793000	6.549000	8.300000	6.445000	4.431000	4.335000	3.865000	4.295000	5.443000	6.383000	5.174000	5.132000	6.982000	8.960000	9.031000	8.016000	7.241000	7.003000	6.758000	Latin America & Caribbean	Lower middle income
97	Croatia	HRV	11.169000	10.635000	10.935000	11.056000	10.854000	10.110000	10.150000	12.005000	14.352000	17.340000	17.907000	17.318001	15.640000	15.278000	13.838000	12.677000	11.340000	10.344000	10.707000	12.305000	13.720000	15.983000	16.759001	18.264999	16.898001	13.787000	11.905000	9.340000	8.644000	8.262000	Europe & Central Asia	High income
98	Haiti	HTI	8.090000	8.118000	8.230000	8.237000	8.388000	8.239000	8.239000	8.255000	8.322000	9.468000	10.727000	12.163000	13.640000	14.978000	16.448999	17.701000	19.041000	18.410999	18.568001	18.135000	17.757000	17.136999	17.212000	17.090000	17.041000	16.952999	16.773001	16.615000	16.693001	16.743000	Latin America & Caribbean	Low income
99	Hungary	HUN	7.999000	8.717000	10.382000	9.363000	8.686000	9.025000	7.857000	8.088000	6.214000	5.786000	4.873000	5.075000	5.384000	5.857000	7.428000	7.914000	7.735000	8.005000	9.740000	10.661000	10.983000	10.610000	10.135000	7.913000	7.039000	5.108000	4.571000	3.986000	3.480000	3.519000	Europe & Central Asia	High income
104	Indonesia	IDN	2.910000	2.814000	3.380000	3.994000	4.694000	5.445000	5.696000	6.133000	6.846000	6.710000	7.381000	7.856000	8.308000	9.183000	10.053000	9.290000	9.911000	8.107000	6.701000	6.395000	5.566000	4.785000	4.266000	3.883000	4.430000	3.865000	3.921000	4.251000	4.510000	4.700000	East Asia & Pacific	Upper middle income
107	India	IND	5.060000	5.347000	5.372000	5.497000	5.522000	5.516000	5.381000	5.492000	5.633000	5.390000	5.480000	5.497000	5.694000	5.633000	5.634000	5.474000	5.385000	5.079000	5.647000	5.741000	5.599000	5.603000	5.646000	5.619000	5.571000	5.505000	5.375000	5.300000	5.233000	5.303000	South Asia	Lower middle income
108	Ireland	IRL	16.590000	15.307000	15.843000	14.707000	12.067000	11.785000	10.164000	7.338000	5.618000	4.267000	3.549000	3.747000	4.006000	3.830000	4.010000	4.211000	4.928000	5.703000	9.510000	11.411000	12.501000	12.752000	12.420000	10.875000	8.907000	7.598000	6.293000	5.691000	4.674000	4.813000	Europe & Central Asia	High income
109	Iran, Islamic Rep.	IRN	24.450001	21.648001	19.202999	16.766001	15.006000	13.408000	14.340000	15.709000	17.003000	18.621000	19.844999	21.841000	18.930000	16.183001	18.231001	16.163000	15.940000	16.826000	16.975000	20.667000	20.577999	20.618000	19.754000	19.761000	19.490999	20.760000	19.872999	19.105000	18.566999	18.965000	Middle East & North Africa	Upper middle income
110	Iraq	IRQ	7.345000	7.720000	7.697000	7.544000	7.600000	7.707000	7.754000	7.739000	7.773000	7.488000	7.574000	7.623000	7.927000	7.791000	7.552000	7.555000	7.436000	8.369000	9.421000	10.478000	11.426000	12.486000	16.858999	22.038000	22.174000	22.339001	31.020000	30.851000	30.403999	30.474001	Middle East & North Africa	Upper middle income
111	Iceland	ISL	2.889000	4.900000	5.601000	5.542000	4.904000	3.450000	3.818000	3.804000	2.787000	2.622000	2.238000	2.649000	3.969000	2.756000	2.491000	3.056000	2.268000	2.588000	5.691000	6.730000	6.205000	5.663000	5.076000	4.785000	4.051000	3.046000	2.718000	2.540000	2.835000	3.186000	Europe & Central Asia	High income
112	Israel	ISR	16.431000	17.108999	14.867000	12.347000	10.743000	9.687000	10.902000	11.210000	11.542000	11.164000	12.013000	12.741000	13.811000	13.767000	11.633000	11.043000	9.813000	7.857000	9.181000	7.950000	6.951000	6.977000	6.238000	5.918000	5.393000	4.942000	4.320000	3.954000	3.898000	3.793000	Middle East & North Africa	High income
113	Italy	ITA	15.780000	13.893000	14.854000	15.326000	16.118999	16.238001	16.407000	16.538000	16.273001	14.858000	13.027000	12.604000	11.945000	10.236000	10.055000	8.779000	7.842000	8.497000	9.223000	9.598000	9.545000	11.827000	13.067000	13.798000	12.694000	12.775000	12.388000	11.775000	10.838000	10.855000	Europe & Central Asia	High income
114	Jamaica	JAM	22.877001	22.858000	22.447001	21.760000	22.434999	23.063000	23.457001	22.225000	22.893999	22.323000	20.914000	19.656000	16.754999	15.651000	15.808000	14.465000	14.304000	13.900000	14.874000	16.250999	16.797001	18.101999	20.091999	18.158001	17.875000	17.431000	15.392000	11.909000	10.807000	10.627000	Latin America & Caribbean	Upper middle income
115	Jordan	JOR	29.752001	30.943001	30.079000	30.580000	30.077000	24.337000	23.702999	22.618999	21.903999	21.219000	20.562000	22.076000	20.893999	23.683001	26.104000	25.266001	25.851000	24.681999	24.385000	21.843000	21.285000	20.035000	22.169001	20.610001	22.662001	24.184999	23.958000	23.693001	23.344999	23.004999	Middle East & North Africa	Upper middle income
116	Japan	JPN	2.220000	2.261000	2.620000	3.020000	3.261000	3.340000	3.400000	3.982000	4.520000	4.463000	4.703000	5.159000	4.941000	4.404000	4.166000	3.867000	3.780000	3.823000	4.808000	4.631000	4.139000	3.955000	3.658000	3.426000	3.113000	2.814000	2.635000	2.178000	2.145000	2.228000	East Asia & Pacific	High income
117	Kazakhstan	KAZ	1.205000	1.363000	1.439000	8.669000	12.475000	14.672000	14.693000	14.768000	15.217000	14.512000	12.044000	11.203000	10.402000	9.838000	9.618000	9.310000	8.740000	7.965000	7.546000	6.640000	6.214000	6.491000	5.865000	5.675000	5.500000	5.509000	5.435000	5.348000	5.222000	5.263000	Europe & Central Asia	Upper middle income
118	Kenya	KEN	2.810000	2.802000	2.915000	2.962000	2.983000	2.969000	2.904000	2.958000	2.984000	2.951000	3.023000	2.984000	3.051000	3.031000	2.993000	2.886000	2.819000	2.678000	2.964000	3.091000	3.035000	3.033000	3.072000	3.022000	2.996000	2.940000	2.859000	2.822000	2.800000	2.795000	Sub-Saharan Africa	Lower middle income
119	Kyrgyz Republic	KGZ	1.260000	1.365000	3.970000	5.341000	6.530000	8.566000	8.763000	10.190000	9.693000	8.759000	9.077000	14.257000	11.064000	9.267000	9.053000	9.032000	9.101000	9.403000	9.770000	9.940000	9.851000	9.474000	9.660000	9.492000	9.024000	8.738000	8.907000	6.445000	7.467000	7.859000	Europe & Central Asia	Lower middle income
120	Cambodia	KHM	1.279000	1.305000	1.357000	1.390000	1.385000	1.396000	1.416000	1.442000	1.554000	1.528000	1.537000	1.584000	1.615000	1.558000	1.503000	1.326000	1.191000	0.824000	0.557000	0.794000	0.477000	0.506000	0.517000	0.695000	0.457000	0.851000	0.813000	0.780000	0.824000	0.855000	East Asia & Pacific	Lower middle income
123	Korea, Rep.	KOR	1.987000	2.134000	2.252000	1.926000	1.685000	1.582000	2.303000	5.655000	5.087000	3.572000	3.290000	2.771000	3.302000	3.403000	3.351000	2.959000	2.562000	2.609000	2.958000	3.288000	3.105000	2.967000	2.867000	3.442000	3.543000	3.584000	3.584000	3.726000	3.898000	4.267000	East Asia & Pacific	High income
124	Kuwait	KWT	1.434000	1.847000	2.156000	1.962000	1.973000	1.826000	1.840000	1.891000	1.832000	1.952000	1.985000	2.407000	2.843000	3.523000	3.276000	2.874000	3.053000	3.685000	3.350000	2.856000	3.636000	4.477000	5.041000	5.231000	4.684000	5.619000	4.849000	5.446000	5.524000	5.618000	Middle East & North Africa	High income
126	Lao PDR	LAO	2.350000	2.520000	2.548000	2.656000	2.600000	2.475000	2.325000	2.113000	2.034000	1.963000	1.776000	1.757000	1.616000	1.507000	1.355000	1.168000	0.945000	0.810000	0.750000	0.661000	0.659000	0.661000	0.669000	0.649000	0.639000	0.625000	0.605000	0.585000	0.569000	0.547000	East Asia & Pacific	Lower middle income
127	Lebanon	LBN	10.614000	9.927000	10.248000	10.275000	10.264000	10.257000	10.330000	10.090000	9.965000	9.877000	9.820000	9.776000	9.651000	9.560000	9.737000	9.896000	10.178000	10.324000	10.505000	10.550000	10.463000	10.553000	10.573000	10.478000	10.393000	10.304000	10.133000	9.997000	9.880000	9.815000	Middle East & North Africa	Upper middle income
128	Liberia	LBR	2.270000	2.290000	2.333000	2.310000	2.289000	2.241000	2.202000	2.185000	2.232000	2.233000	2.234000	2.323000	2.329000	2.319000	2.248000	2.146000	2.068000	2.034000	2.230000	2.290000	2.194000	2.108000	2.025000	1.894000	1.783000	2.333000	2.275000	2.211000	2.224000	2.219000	Sub-Saharan Africa	Low income
129	Libya	LBY	25.527000	25.094000	25.420000	25.524000	24.948000	25.368999	25.062000	24.999001	25.062000	25.068001	25.485001	25.688000	25.757999	25.167000	25.127001	24.561001	24.396000	24.417999	25.047001	25.181999	24.496000	25.082001	24.749001	25.107000	25.327000	25.215000	25.003000	24.599001	24.570000	24.663000	Middle East & North Africa	Upper middle income
130	St. Lucia	LCA	22.704000	23.167999	22.933001	23.040001	21.061001	19.622000	24.061001	26.308001	20.629000	21.268999	22.533001	24.167999	28.382999	25.188000	23.139999	20.395000	18.784000	14.776000	16.525000	18.427999	20.311001	21.344000	24.027000	25.745001	27.462999	23.513000	23.263000	22.978001	23.256001	22.921000	Latin America & Caribbean	Upper middle income
135	Sri Lanka	LKA	22.743999	21.930000	21.931999	20.323999	18.995001	17.747999	16.330999	13.493000	13.382000	11.469000	11.417000	13.014000	12.860000	12.913000	11.886000	9.897000	9.133000	8.201000	8.739000	7.502000	7.012000	6.178000	6.492000	6.339000	7.453000	6.837000	6.737000	6.647000	6.687000	6.635000	South Asia	Lower middle income
138	Lesotho	LSO	45.993999	46.556000	46.983002	47.308998	47.395000	47.648998	47.235001	45.625999	44.424000	43.273998	41.800999	40.647999	39.724998	38.264999	37.047001	35.696999	34.383999	33.324001	32.827000	32.233002	31.186001	30.202999	29.127001	28.987000	28.684999	28.478001	27.777000	27.764000	27.118000	26.143999	Sub-Saharan Africa	Lower middle income
140	Lithuania	LTU	0.950000	0.941000	12.832000	12.998000	16.980000	16.101000	13.935000	11.860000	12.061000	13.604000	14.091000	12.783000	13.201000	11.066000	8.535000	5.611000	4.329000	5.628000	10.527000	14.519000	12.912000	11.576000	10.481000	9.223000	8.161000	6.650000	5.673000	5.410000	6.305000	6.060000	Europe & Central Asia	High income
141	Luxembourg	LUX	2.122000	2.784000	3.131000	4.294000	4.385000	4.663000	3.649000	4.169000	3.337000	3.141000	2.169000	3.635000	4.711000	7.094000	5.827000	6.270000	4.723000	6.029000	6.127000	5.097000	6.259000	5.909000	6.387000	5.766000	7.349000	6.632000	5.499000	5.884000	5.890000	5.841000	Europe & Central Asia	High income
142	Latvia	LVA	2.524000	6.003000	15.548000	18.073999	18.535000	20.242001	14.905000	13.498000	13.359000	13.375000	12.461000	12.755000	12.021000	12.273000	9.970000	6.735000	5.565000	7.075000	14.127000	16.284000	13.782000	13.941000	11.139000	9.834000	8.637000	8.400000	7.674000	6.417000	5.631000	5.961000	Europe & Central Asia	High income
143	Macao SAR, China	MAC	3.763000	2.376000	1.984000	2.616000	2.971000	3.578000	2.583000	3.307000	4.338000	4.614000	4.401000	4.493000	4.709000	4.039000	3.785000	3.800000	2.891000	2.794000	2.844000	2.080000	2.155000	1.651000	1.397000	1.437000	1.601000	1.494000	1.592000	1.585000	1.799000	1.981000	East Asia & Pacific	High income
145	Morocco	MAR	13.109000	12.840000	13.166000	13.601000	12.899000	13.743000	13.041000	13.532000	13.294000	13.018000	12.505000	12.543000	12.984000	11.358000	11.552000	9.670000	9.428000	9.783000	9.284000	9.488000	10.229000	9.947000	9.571000	10.290000	10.424000	10.701000	10.718000	10.538000	10.415000	10.419000	Middle East & North Africa	Lower middle income
147	Moldova	MDA	1.077000	2.494000	2.612000	4.176000	3.905000	6.098000	6.144000	7.952000	8.998000	7.237000	5.926000	5.555000	6.380000	6.325000	5.967000	5.797000	3.877000	3.369000	4.885000	5.729000	5.587000	4.286000	4.140000	3.060000	2.920000	2.880000	3.339000	2.472000	4.155000	4.136000	Europe & Central Asia	Lower middle income
148	Madagascar	MDG	5.530000	5.675000	5.779000	5.793000	5.821000	5.841000	5.831000	5.833000	5.904000	5.876000	5.401000	5.756000	6.238000	4.783000	3.513000	3.730000	3.975000	4.297000	4.887000	5.325000	2.328000	0.627000	0.970000	1.362000	1.801000	1.790000	1.752000	1.721000	1.846000	1.968000	Sub-Saharan Africa	Low income
149	Maldives	MDV	1.364000	1.333000	1.337000	1.334000	1.311000	1.583000	1.857000	2.140000	2.424000	2.727000	2.941000	3.230000	3.610000	3.610000	3.682000	4.160000	4.018000	4.258000	4.622000	5.120000	5.348000	5.549000	5.897000	6.044000	5.779000	5.521000	5.376000	5.258000	5.866000	6.544000	South Asia	Upper middle income
151	Mexico	MEX	4.246000	4.049000	3.956000	4.843000	8.621000	6.886000	6.169000	4.943000	3.557000	3.280000	3.292000	3.834000	4.472000	5.274000	3.909000	3.890000	4.001000	4.037000	5.353000	5.183000	5.129000	4.877000	4.956000	4.847000	4.453000	3.909000	3.601000	3.427000	3.712000	4.073000	Latin America & Caribbean	Upper middle income
154	North Macedonia	MKD	39.773998	40.058998	40.325001	40.691002	40.675999	40.834999	40.771000	37.634998	33.262001	34.898998	32.056000	32.304001	36.279999	37.812000	38.439999	37.201000	35.542000	34.157001	32.816002	32.240002	30.790001	30.333000	29.020000	28.618999	25.063000	22.725000	21.820999	19.868000	17.049000	15.426000	Europe & Central Asia	Upper middle income
155	Mali	MLI	3.183000	3.186000	3.315000	3.335000	3.336000	3.329000	3.300000	4.032000	4.923000	5.819000	7.021000	8.067000	9.567000	10.848000	11.784000	12.359000	12.967000	12.160000	11.784000	10.894000	8.636000	8.234000	7.884000	6.292000	8.793000	8.535000	8.204000	7.862000	7.954000	8.046000	Sub-Saharan Africa	Low income
156	Malta	MLT	5.803000	6.362000	6.468000	6.478000	6.458000	6.564000	6.654000	6.769000	6.821000	6.531000	8.068000	8.302000	9.901000	8.255000	8.368000	8.317000	7.894000	6.775000	7.628000	7.132000	7.129000	7.210000	6.139000	5.102000	5.387000	5.164000	4.252000	3.556000	3.774000	3.616000	Middle East & North Africa	High income
157	Myanmar	MMR	0.749000	0.839000	0.825000	0.849000	0.845000	0.848000	0.840000	0.842000	0.877000	0.886000	0.877000	0.891000	0.905000	0.891000	0.871000	0.829000	0.793000	0.755000	0.832000	0.878000	0.896000	0.917000	0.935000	0.917000	0.903000	1.394000	2.023000	1.962000	2.074000	2.262000	East Asia & Pacific	Lower middle income
159	Montenegro	MNE	34.702999	34.770000	34.985001	35.716999	35.790001	36.004002	35.588001	35.641998	35.622002	35.820000	35.756001	35.914001	35.903000	35.771999	35.550999	27.614000	20.966999	18.330000	20.429001	20.739000	20.138000	20.247000	18.816999	18.221001	17.306000	17.080000	16.947001	15.097000	15.730000	14.966000	Europe & Central Asia	Upper middle income
160	Mongolia	MNG	5.766000	5.856000	6.079000	6.124000	6.119000	6.082000	6.094000	6.086000	6.140000	6.089000	6.160000	6.200000	6.419000	6.665000	6.902000	7.020000	7.146000	5.173000	6.175000	5.888000	4.421000	3.516000	4.469000	4.399000	4.246000	5.873000	5.641000	5.591000	5.630000	5.664000	East Asia & Pacific	Lower middle income
162	Mozambique	MOZ	1.522000	1.533000	1.615000	1.585000	1.623000	1.627000	1.563000	1.694000	1.847000	1.934000	2.023000	2.151000	2.244000	2.285000	2.345000	2.351000	2.361000	2.447000	2.777000	2.971000	3.128000	3.350000	3.530000	3.613000	3.673000	3.597000	3.522000	3.474000	3.453000	3.424000	Sub-Saharan Africa	Low income
163	Mauritania	MRT	11.693000	11.827000	11.984000	12.173000	12.214000	12.008000	11.759000	11.972000	12.153000	11.924000	12.080000	12.022000	12.313000	12.444000	12.594000	12.237000	11.624000	11.765000	12.077000	12.388000	12.401000	12.630000	12.633000	12.492000	12.169000	12.186000	12.046000	11.950000	12.112000	12.184000	Sub-Saharan Africa	Lower middle income
164	Mauritius	MUS	13.335000	13.619000	13.621000	13.626000	13.625000	13.387000	13.056000	12.750000	12.276000	12.392000	11.542000	11.583000	12.772000	13.362000	16.319000	15.494000	14.382000	12.624000	12.301000	12.939000	11.719000	11.720000	11.160000	10.840000	10.702000	10.422000	10.053000	9.904000	9.999000	10.130000	Sub-Saharan Africa	High income
165	Malawi	MWI	6.633000	6.659000	6.891000	6.829000	6.949000	6.826000	6.812000	6.811000	6.907000	6.865000	6.871000	7.006000	7.002000	6.928000	6.866000	6.732000	6.615000	6.546000	6.890000	6.959000	6.909000	6.931000	6.984000	6.895000	6.838000	6.780000	6.699000	6.575000	6.617000	6.650000	Sub-Saharan Africa	Low income
166	Malaysia	MYS	4.281000	4.321000	4.844000	4.274000	3.763000	2.604000	2.835000	3.309000	3.295000	3.058000	3.788000	3.778000	3.636000	3.805000	3.705000	3.391000	3.445000	3.673000	3.794000	3.433000	3.314000	3.162000	3.436000	3.222000	3.392000	3.929000	3.927000	3.826000	3.740000	3.764000	East Asia & Pacific	Upper middle income
168	Namibia	NAM	18.612000	19.608999	19.924999	21.113001	22.671000	24.250999	25.864000	24.403999	23.128000	21.646999	22.423000	23.531000	24.177999	25.108999	24.639999	24.169001	23.733999	23.511000	23.770000	24.023001	21.341999	18.997000	20.697001	20.372000	22.799999	25.010000	21.569000	18.589001	19.686001	20.759001	Sub-Saharan Africa	Upper middle income
169	New Caledonia	NCL	20.775999	21.368999	21.910000	22.121000	22.455000	22.107000	21.013000	20.274000	20.233000	20.142000	20.087000	19.974001	19.936001	19.312000	18.176001	16.753000	15.365000	14.861000	16.275000	16.785999	16.629000	16.348000	16.108000	15.597000	15.274000	14.827000	14.349000	13.954000	14.080000	14.234000	East Asia & Pacific	High income
170	Niger	NER	0.895000	0.904000	0.971000	0.973000	0.959000	0.998000	0.968000	0.982000	0.974000	0.988000	1.001000	1.363000	1.791000	2.194000	2.709000	2.056000	1.514000	1.112000	0.827000	0.539000	0.218000	0.291000	0.355000	0.428000	0.419000	0.406000	0.387000	0.369000	0.360000	0.363000	Sub-Saharan Africa	Low income
171	Nigeria	NGA	3.384000	3.504000	3.555000	3.545000	3.581000	3.617000	3.596000	3.609000	3.632000	3.674000	3.677000	3.836000	3.741000	3.739000	3.651000	3.578000	3.524000	3.487000	3.702000	3.773000	3.687000	3.518000	3.359000	5.403000	5.106000	8.114000	9.260000	9.101000	8.908000	8.717000	Sub-Saharan Africa	Lower middle income
172	Nicaragua	NIC	7.375000	7.446000	7.550000	7.765000	7.771000	7.822000	7.761000	7.767000	7.995000	7.806000	7.776000	7.776000	7.849000	7.974000	5.777000	4.972000	5.040000	6.955000	8.846000	8.722000	7.201000	5.802000	5.472000	5.616000	5.444000	4.274000	3.488000	5.261000	6.696000	7.367000	Latin America & Caribbean	Lower middle income
173	Netherlands	NLD	9.925000	7.809000	7.695000	8.057000	8.668000	8.118000	7.148000	5.813000	4.856000	3.462000	2.516000	2.887000	3.795000	5.041000	6.918000	6.160000	5.208000	4.476000	4.921000	5.548000	5.417000	6.231000	7.302000	7.747000	7.276000	6.510000	5.258000	3.957000	3.362000	3.143000	Europe & Central Asia	High income
174	Norway	NOR	4.985000	5.084000	5.166000	4.705000	6.328000	5.341000	5.026000	4.024000	3.234000	3.276000	3.613000	4.094000	3.956000	3.923000	4.192000	3.344000	2.423000	2.364000	2.604000	2.944000	2.991000	2.634000	3.232000	3.235000	3.999000	3.913000	3.691000	3.538000	3.244000	3.184000	Europe & Central Asia	High income
175	Nepal	NPL	1.492000	1.507000	1.585000	1.609000	1.583000	1.638000	1.618000	1.616000	1.659000	1.601000	1.539000	1.511000	1.515000	1.424000	1.339000	1.226000	1.124000	1.070000	1.204000	1.272000	1.273000	1.289000	1.303000	1.292000	1.253000	1.227000	1.233000	1.159000	1.210000	1.266000	South Asia	Lower middle income
177	New Zealand	NZL	9.769000	9.732000	8.960000	7.809000	6.488000	6.248000	6.861000	7.613000	6.692000	5.978000	5.382000	5.469000	5.112000	4.521000	4.147000	4.199000	3.966000	4.264000	6.143000	6.932000	6.761000	7.393000	6.970000	6.542000	5.865000	5.474000	5.198000	4.468000	4.449000	4.397000	East Asia & Pacific	High income
179	Oman	OMN	9.281000	9.691000	10.310000	10.132000	9.950000	9.644000	9.511000	9.510000	9.061000	8.878000	9.083000	8.858000	8.580000	8.522000	8.369000	8.130000	7.926000	7.953000	10.201000	10.691000	10.997000	12.284000	13.312000	12.964000	13.210000	13.299000	12.424000	11.823000	11.910000	12.428000	Middle East & North Africa	High income
181	Pakistan	PAK	0.313000	0.343000	0.301000	0.338000	0.334000	0.352000	0.301000	0.331000	0.336000	0.333000	0.315000	0.338000	0.353000	0.370000	0.374000	0.323000	0.244000	0.534000	0.357000	0.639000	0.669000	1.697000	3.427000	2.014000	6.182000	5.789000	5.226000	4.560000	5.596000	5.309000	South Asia	Lower middle income
182	Panama	PAN	4.833000	4.900000	5.005000	4.982000	4.987000	5.126000	5.043000	5.080000	5.051000	5.010000	4.910000	5.000000	5.057000	5.056000	4.941000	4.845000	4.845000	4.766000	4.836000	5.099000	2.696000	3.058000	3.081000	3.524000	3.929000	3.937000	5.116000	4.994000	5.055000	4.973000	Latin America & Caribbean	High income
183	Peru	PER	5.543000	5.506000	5.772000	5.925000	5.803000	5.750000	5.843000	5.688000	5.811000	5.800000	5.748000	5.919000	4.643000	5.369000	4.868000	4.751000	4.561000	4.543000	3.950000	3.804000	3.618000	3.497000	3.539000	3.073000	2.962000	3.572000	3.501000	3.462000	3.409000	3.399000	Latin America & Caribbean	Upper middle income
184	Philippines	PHL	3.789000	3.886000	4.025000	4.083000	4.067000	4.115000	4.084000	3.916000	4.064000	4.048000	4.020000	3.964000	3.843000	3.889000	4.199000	4.330000	3.716000	4.007000	4.132000	3.790000	3.829000	3.733000	3.823000	3.724000	3.219000	2.870000	2.702000	2.676000	2.442000	2.442000	East Asia & Pacific	Lower middle income
186	Papua New Guinea	PNG	1.975000	2.020000	2.113000	2.032000	1.985000	2.085000	1.959000	2.062000	2.049000	2.009000	1.945000	1.901000	1.834000	1.691000	1.602000	1.433000	1.361000	1.194000	1.306000	1.277000	1.448000	1.493000	1.488000	1.533000	1.431000	1.398000	1.362000	1.306000	1.378000	1.413000	East Asia & Pacific	Lower middle income
187	Poland	POL	13.182000	14.708000	15.613000	16.010000	14.755000	13.951000	13.006000	11.823000	13.173000	18.334000	20.028000	20.681999	19.930000	19.768999	19.143000	14.924000	10.343000	7.958000	8.664000	10.021000	10.397000	10.906000	11.112000	9.593000	7.710000	6.226000	4.908000	3.847000	3.509000	3.094000	Europe & Central Asia	High income
189	Puerto Rico	PRI	12.971000	13.268000	13.182000	11.266000	10.690000	11.577000	12.146000	11.977000	9.439000	7.401000	9.179000	10.308000	10.647000	8.607000	10.092000	10.548000	9.525000	9.647000	12.413000	12.554000	12.620000	12.059000	11.827000	11.376000	9.647000	9.475000	8.578000	7.186000	6.214000	6.428000	Latin America & Caribbean	High income
190	Korea, Dem. People’s Rep.	PRK	2.367000	2.408000	2.503000	2.514000	2.504000	2.529000	2.511000	2.529000	2.552000	2.497000	2.526000	2.557000	2.575000	2.527000	2.475000	2.349000	2.281000	2.255000	2.447000	2.526000	2.528000	2.538000	2.550000	2.507000	2.467000	2.440000	2.342000	2.310000	2.331000	2.349000	East Asia & Pacific	Low income
191	Portugal	PRT	5.637000	4.778000	6.283000	7.786000	7.793000	8.454000	7.503000	5.696000	5.065000	4.737000	4.952000	5.314000	7.260000	7.183000	8.641000	8.912000	9.497000	8.753000	10.092000	11.862000	13.043000	15.497000	16.367001	14.326000	12.746000	11.166000	9.349000	7.437000	6.946000	6.580000	Europe & Central Asia	High income
192	Paraguay	PRY	11.309000	11.360000	11.685000	11.747000	11.792000	11.656000	11.789000	11.608000	11.704000	11.647000	11.716000	11.848000	8.361000	8.398000	6.279000	6.689000	6.693000	5.994000	6.668000	6.071000	6.423000	5.470000	4.975000	6.829000	4.949000	6.747000	5.443000	5.321000	5.784000	5.859000	Latin America & Caribbean	Upper middle income
193	West Bank and Gaza	PSE	7.827000	8.324000	8.137000	8.296000	8.161000	8.131000	8.320000	8.283000	8.241000	8.032000	10.071000	12.827000	15.178000	14.838000	16.674000	12.998000	14.434000	19.539000	20.113001	21.562000	22.437000	26.025999	27.420000	26.155001	34.298000	37.929001	42.769001	41.849998	40.945000	40.616001	Middle East & North Africa	Lower middle income
196	French Polynesia	PYF	12.830000	13.180000	13.509000	13.509000	13.499000	13.348000	13.213000	13.317000	13.714000	13.519000	13.330000	13.361000	13.872000	13.901000	13.723000	13.231000	12.799000	12.796000	14.532000	14.794000	14.868000	15.185000	15.581000	15.171000	14.681000	14.248000	13.818000	13.432000	13.227000	13.096000	East Asia & Pacific	High income
197	Qatar	QAT	3.614000	4.724000	6.069000	5.798000	5.525000	5.235000	5.076000	5.286000	4.950000	3.988000	4.065000	4.202000	4.192000	4.122000	3.502000	3.132000	2.476000	1.565000	1.708000	2.771000	3.402000	2.879000	1.595000	1.050000	0.867000	0.717000	0.639000	0.479000	0.432000	0.385000	Middle East & North Africa	High income
198	Romania	ROU	7.670000	7.851000	8.429000	8.744000	8.587000	7.319000	5.891000	5.470000	5.527000	6.370000	6.003000	7.591000	6.375000	6.147000	6.427000	6.109000	5.408000	4.670000	5.839000	6.177000	6.465000	6.078000	6.308000	6.095000	5.852000	4.983000	4.039000	3.488000	3.423000	3.350000	Europe & Central Asia	High income
199	Russian Federation	RUS	5.195000	5.167000	5.823000	7.950000	9.197000	9.297000	11.455000	12.955000	12.896000	10.368000	8.597000	7.599000	7.952000	7.517000	6.934000	6.686000	5.642000	5.943000	7.670000	6.835000	6.086000	5.069000	5.135000	4.821000	5.309000	5.348000	5.051000	4.777000	4.462000	4.313000	Europe & Central Asia	Upper middle income
200	Rwanda	RWA	0.149000	0.197000	0.248000	0.287000	0.347000	0.337000	0.369000	0.403000	0.470000	0.526000	0.583000	0.665000	0.710000	0.745000	0.756000	0.737000	0.729000	0.762000	0.937000	1.031000	1.088000	1.158000	1.229000	1.245000	1.215000	1.175000	1.113000	1.061000	1.070000	1.074000	Sub-Saharan Africa	Low income
202	Saudi Arabia	SAU	5.140000	4.948000	5.343000	5.961000	6.554000	7.118000	7.604000	7.927000	8.180000	9.367000	9.187000	11.637000	12.651000	13.506000	14.227000	14.784000	14.350000	13.955000	16.225000	17.521999	19.089001	21.013000	20.834999	21.723000	21.657000	21.174000	20.332001	22.488001	22.114000	22.454000	Middle East & North Africa	High income
203	Sudan	SDN	20.309999	20.263000	20.337999	20.386000	20.440001	20.246000	20.249001	20.299999	20.240000	20.355000	20.379999	20.315001	20.399000	20.283001	20.187000	19.933001	19.697001	19.587999	22.969999	26.809999	30.299999	29.940001	30.624001	30.298000	30.025000	29.875000	29.513000	29.125999	27.768999	27.171000	Sub-Saharan Africa	Low income
204	Senegal	SEN	7.677000	7.724000	7.841000	7.864000	7.931000	7.852000	7.889000	7.887000	7.881000	7.843000	7.853000	7.854000	9.376000	10.758000	12.319000	13.757000	13.554000	13.406000	13.853000	13.967000	13.895000	12.064000	10.289000	8.734000	7.304000	7.251000	7.185000	7.072000	7.446000	7.774000	Sub-Saharan Africa	Lower middle income
205	Singapore	SGP	2.186000	3.183000	3.404000	3.443000	3.448000	3.871000	2.605000	3.696000	5.201000	3.457000	3.888000	5.784000	6.201000	6.212000	6.074000	4.960000	4.329000	4.421000	6.547000	4.420000	4.331000	4.234000	4.407000	4.112000	3.978000	4.488000	4.436000	4.255000	4.314000	4.536000	East Asia & Pacific	High income
206	Solomon Islands	SLB	1.690000	1.757000	1.773000	1.824000	1.829000	1.788000	1.782000	1.798000	1.822000	1.732000	1.794000	1.853000	1.921000	1.873000	1.867000	1.716000	1.671000	1.647000	1.768000	1.661000	1.329000	0.967000	0.722000	0.693000	0.690000	0.684000	0.659000	0.638000	0.595000	0.561000	East Asia & Pacific	Lower middle income
207	Sierra Leone	SLE	2.168000	2.233000	2.286000	2.295000	2.279000	2.315000	2.299000	2.295000	2.302000	2.312000	2.292000	2.307000	2.310000	2.283000	2.390000	2.441000	2.499000	2.600000	2.964000	3.264000	3.468000	3.601000	3.704000	3.830000	3.807000	3.797000	3.701000	3.624000	3.628000	3.642000	Sub-Saharan Africa	Low income
208	El Salvador	SLV	6.431000	7.083000	6.819000	6.417000	5.874000	6.474000	5.325000	5.777000	4.438000	3.744000	5.218000	3.206000	3.204000	3.425000	4.794000	3.884000	3.713000	3.622000	4.921000	4.161000	3.414000	3.304000	3.332000	3.141000	3.474000	3.852000	4.006000	3.751000	3.578000	3.561000	Latin America & Caribbean	Lower middle income
210	Somalia	SOM	10.930000	11.001000	11.306000	11.169000	11.417000	11.260000	11.172000	11.235000	11.273000	11.242000	11.259000	11.374000	11.402000	11.318000	11.226000	10.999000	10.769000	10.724000	11.217000	11.381000	11.418000	11.456000	11.503000	11.446000	11.374000	11.262000	11.113000	11.008000	11.079000	11.135000	Sub-Saharan Africa	Low income
211	Serbia	SRB	15.657000	15.600000	15.788000	16.208000	16.146000	15.662000	16.011999	16.160000	16.275000	15.030000	15.028000	15.732000	16.337999	22.969000	26.240000	24.711000	21.011000	15.995000	17.870001	20.219999	23.688000	24.966999	23.832001	20.372000	18.757999	16.099001	14.273000	13.698000	13.780000	14.056000	Europe & Central Asia	Upper middle income
213	South Sudan	SSD	13.546000	13.649000	13.870000	13.875000	13.892000	13.835000	13.809000	13.759000	13.867000	13.835000	13.862000	13.952000	13.948000	13.884000	13.748000	13.495000	13.291000	13.210000	13.705000	13.823000	13.788000	13.933000	14.058000	13.872000	13.792000	13.740000	13.550000	13.394000	13.406000	13.417000	Sub-Saharan Africa	Low income
216	Sao Tome and Principe	STP	17.622999	17.624001	17.808001	17.886000	17.861000	17.858000	17.813999	17.882000	17.927999	17.794001	25.796000	26.299999	26.132999	24.316000	25.681000	25.660999	24.396999	24.191000	23.525999	23.483999	22.434000	21.601999	21.650000	21.555000	21.417000	21.305000	21.091000	20.795000	20.919001	21.330999	Sub-Saharan Africa	Lower middle income
217	Suriname	SUR	22.827999	23.584999	23.188000	15.134000	11.006000	16.905001	16.349001	17.256001	20.660000	19.663000	18.759001	17.823999	16.862000	15.734000	15.444000	15.091000	14.738000	14.540000	14.872000	12.840000	12.723000	12.703000	11.206000	12.192000	11.665000	11.628000	11.607000	11.441000	11.923000	12.098000	Latin America & Caribbean	Upper middle income
218	Slovak Republic	SVK	10.327000	10.957000	11.822000	14.100000	13.789000	12.680000	12.843000	12.543000	15.907000	18.612000	18.549999	18.781000	17.284000	19.628000	17.232000	14.735000	12.679000	10.927000	12.827000	14.598000	13.627000	14.521000	14.525000	13.631000	12.885000	10.753000	8.407000	7.025000	6.182000	5.684000	Europe & Central Asia	High income
219	Slovenia	SVN	5.944000	6.022000	6.407000	6.946000	6.843000	6.648000	6.953000	7.473000	7.473000	7.064000	6.016000	6.271000	7.017000	6.403000	7.034000	7.188000	5.831000	4.819000	5.804000	7.019000	8.173000	9.369000	10.911000	10.550000	10.038000	8.588000	7.479000	5.697000	4.660000	3.935000	Europe & Central Asia	High income
220	Sweden	SWE	2.947000	4.614000	7.642000	8.136000	7.860000	8.575000	9.685000	8.035000	6.859000	4.999000	4.397000	4.622000	5.009000	6.182000	7.373000	7.255000	6.466000	6.565000	8.011000	8.521000	7.763000	7.711000	7.888000	7.694000	7.290000	6.586000	6.442000	6.292000	6.220000	6.250000	Europe & Central Asia	High income
221	Eswatini	SWZ	22.652000	22.971001	23.091000	23.087000	23.179001	24.454000	25.746000	26.458000	27.069000	27.497999	28.093000	29.025000	29.556999	30.052999	30.622000	30.830000	31.030001	29.906000	29.924999	29.372999	28.431999	27.823999	27.135000	25.979000	25.131001	24.462999	24.162001	23.915001	23.660999	23.615000	Sub-Saharan Africa	Lower middle income
224	Syrian Arab Republic	SYR	13.343000	9.921000	6.455000	11.649000	13.803000	14.228000	14.607000	15.270000	15.048000	19.594000	25.556000	23.298000	20.906000	22.076000	22.877001	23.438000	25.636000	24.159000	22.129000	21.945999	21.924000	21.518999	21.549999	21.355000	21.180000	21.105000	20.995001	20.879000	20.837000	20.544001	Middle East & North Africa	Low income
226	Chad	TCD	0.269000	0.270000	0.285000	0.342000	0.364000	0.412000	0.448000	0.484000	0.541000	0.597000	0.676000	0.779000	0.831000	0.811000	0.801000	0.834000	0.849000	0.883000	1.108000	1.232000	1.274000	1.398000	1.466000	1.522000	1.561000	1.582000	1.625000	1.642000	1.706000	1.784000	Sub-Saharan Africa	Low income
229	Togo	TGO	4.045000	4.127000	4.222000	4.354000	4.285000	4.260000	4.317000	4.298000	4.369000	4.387000	4.402000	4.483000	4.492000	4.415000	4.330000	4.164000	3.389000	2.821000	2.522000	2.080000	1.654000	1.672000	1.716000	1.696000	1.700000	1.661000	1.622000	1.586000	1.547000	1.516000	Sub-Saharan Africa	Low income
230	Thailand	THA	3.430000	1.488000	1.829000	1.621000	1.342000	1.115000	0.903000	3.397000	2.942000	2.335000	2.491000	1.634000	1.442000	1.379000	1.209000	1.122000	1.065000	1.039000	0.833000	0.622000	0.676000	0.531000	0.476000	0.564000	0.606000	0.714000	0.845000	0.743000	0.742000	0.791000	East Asia & Pacific	Upper middle income
231	Tajikistan	TJK	1.307000	1.388000	5.271000	7.741000	9.189000	12.139000	12.657000	15.249000	14.093000	13.829000	13.431000	13.142000	12.790000	12.338000	11.802000	11.290000	10.774000	10.312000	10.348000	10.495000	10.540000	10.538000	10.551000	10.461000	10.417000	10.381000	10.260000	10.083000	9.935000	9.833000	Europe & Central Asia	Low income
232	Turkmenistan	TKM	0.373000	0.443000	0.442000	2.732000	5.724000	7.499000	8.161000	9.349000	9.862000	8.923000	8.087000	7.313000	6.573000	5.756000	4.990000	4.215000	3.552000	3.032000	2.658000	2.269000	2.298000	2.279000	2.291000	2.264000	2.222000	2.195000	2.149000	2.104000	2.179000	2.281000	Europe & Central Asia	Upper middle income
234	Timor-Leste	TLS	3.947000	3.977000	4.056000	4.069000	4.070000	4.088000	4.039000	4.013000	3.887000	4.188000	4.131000	4.053000	4.092000	3.999000	3.942000	3.787000	3.785000	3.746000	3.991000	4.023000	4.368000	4.706000	5.085000	5.460000	5.828000	6.237000	6.017000	6.032000	6.169000	6.278000	East Asia & Pacific	Lower middle income
236	Tonga	TON	2.425000	2.261000	2.366000	2.726000	3.169000	3.534000	3.949000	4.591000	5.075000	5.574000	6.200000	6.833000	7.429000	5.275000	3.449000	1.960000	1.936000	1.948000	2.068000	2.102000	2.084000	2.063000	2.089000	2.091000	2.059000	2.032000	1.965000	1.923000	2.027000	2.081000	East Asia & Pacific	Upper middle income
239	Trinidad and Tobago	TTO	3.848000	4.328000	4.646000	4.448000	4.511000	4.471000	4.434000	4.338000	4.545000	4.375000	4.341000	4.491000	4.509000	4.238000	3.811000	3.458000	3.145000	2.967000	4.089000	4.295000	3.991000	4.268000	2.942000	2.561000	2.609000	2.953000	2.652000	2.544000	2.825000	3.044000	Latin America & Caribbean	High income
240	Tunisia	TUN	16.879999	17.354000	17.070999	17.253000	17.171000	17.511999	17.344000	16.862000	16.577999	16.108000	15.564000	15.740000	16.204000	16.399000	15.166000	15.148000	15.161000	15.946000	18.797001	18.968000	27.422001	25.664000	23.047001	21.493999	22.416000	23.443001	23.121000	23.131001	23.410000	22.745001	Middle East & North Africa	Lower middle income
241	Turkey	TUR	7.137000	7.779000	9.350000	8.095000	7.329000	5.974000	7.789000	6.808000	7.588000	6.272000	7.484000	9.466000	10.108000	10.989000	11.180000	9.097000	9.146000	9.980000	12.609000	11.383000	10.060000	9.364000	10.532000	11.810000	12.562000	13.596000	13.854000	13.735000	16.422001	15.429000	Europe & Central Asia	Upper middle income
243	Tanzania	TZA	4.376000	4.300000	4.239000	4.086000	3.908000	3.737000	3.551000	3.415000	3.290000	3.121000	2.963000	3.373000	3.688000	3.985000	4.275000	4.475000	3.755000	3.104000	2.759000	3.507000	4.331000	4.037000	3.804000	2.714000	2.670000	2.645000	2.596000	2.548000	2.462000	2.418000	Sub-Saharan Africa	Lower middle income
244	Uganda	UGA	0.573000	0.655000	0.843000	1.059000	1.320000	1.609000	1.936000	2.326000	2.782000	3.229000	3.791000	4.379000	4.555000	3.180000	2.111000	2.427000	2.856000	3.326000	4.176000	4.271000	4.279000	4.277000	2.464000	2.404000	2.381000	2.329000	2.304000	2.245000	2.296000	2.331000	Sub-Saharan Africa	Low income
245	Ukraine	UKR	1.434000	1.430000	1.487000	1.436000	4.937000	7.273000	8.390000	10.762000	11.365000	11.312000	10.684000	9.764000	8.736000	8.292000	6.822000	6.602000	5.995000	6.088000	7.299000	6.815000	6.679000	6.442000	6.216000	7.516000	8.077000	7.693000	7.747000	7.424000	7.796000	7.872000	Europe & Central Asia	Lower middle income
247	Uruguay	URY	11.752000	12.265000	12.827000	12.853000	12.671000	12.532000	12.595000	12.312000	13.676000	15.873000	19.825001	21.002001	20.806000	16.612000	15.261000	14.233000	12.761000	10.945000	10.462000	9.466000	8.099000	8.311000	8.228000	8.360000	8.856000	9.451000	9.482000	10.089000	10.658000	10.872000	Latin America & Caribbean	High income
248	United States	USA	6.358000	7.001000	6.570000	6.052000	5.672000	5.470000	5.075000	4.622000	4.341000	4.100000	4.653000	5.610000	5.661000	5.397000	5.097000	4.627000	4.501000	5.418000	8.059000	8.612000	8.459000	7.890000	7.071000	6.059000	5.177000	4.788000	4.308000	3.837000	3.616000	3.836000	North America	High income
249	Uzbekistan	UZB	1.627000	2.497000	4.448000	6.776000	7.406000	10.361000	10.587000	13.020000	12.995000	11.754000	10.621000	9.618000	8.587000	7.612000	6.612000	5.618000	4.748000	4.647000	4.790000	5.102000	4.701000	4.636000	4.618000	4.848000	4.886000	4.902000	5.611000	5.517000	5.688000	5.823000	Europe & Central Asia	Lower middle income
250	St. Vincent and the Grenadines	VCT	22.114000	22.143000	21.684000	21.125000	21.254999	20.448999	20.267000	19.862000	19.427000	19.016001	18.615999	18.753000	18.565001	18.118999	17.761000	17.493000	16.891001	16.486000	16.947001	17.065001	17.205000	17.253000	17.337999	17.118999	17.120001	17.044001	16.902000	16.715000	16.679001	16.665001	Latin America & Caribbean	Upper middle income
251	Venezuela, RB	VEN	9.317000	6.907000	5.661000	9.578000	12.644000	14.396000	13.656000	13.336000	16.502001	14.606000	14.114000	18.917000	20.485001	17.945999	11.642000	9.382000	7.644000	6.218000	5.965000	6.952000	7.207000	7.110000	8.605000	8.488000	8.483000	8.114000	8.029000	8.000000	9.320000	9.619000	Latin America & Caribbean	Upper middle income
253	Virgin Islands (U.S.)	VIR	9.441000	9.910000	10.531000	10.585000	10.575000	10.456000	10.315000	10.133000	10.361000	10.312000	10.297000	10.389000	10.413000	10.173000	9.932000	9.331000	8.805000	8.789000	10.313000	10.605000	10.505000	10.321000	10.554000	10.495000	10.237000	9.905000	9.454000	9.088000	9.221000	9.228000	Latin America & Caribbean	High income
254	Vietnam	VNM	1.483000	1.583000	1.635000	1.671000	1.693000	1.694000	2.541000	2.156000	2.408000	2.141000	3.274000	2.332000	2.639000	2.440000	2.293000	2.089000	1.933000	1.636000	1.573000	1.076000	2.269000	1.910000	1.965000	1.816000	1.994000	1.927000	1.882000	1.831000	1.904000	1.953000	East Asia & Pacific	Lower middle income
255	Vanuatu	VUT	4.840000	4.931000	5.004000	5.182000	4.998000	5.082000	5.109000	5.025000	5.084000	5.171000	4.974000	5.052000	5.239000	5.169000	5.134000	5.069000	4.901000	4.947000	5.153000	5.177000	5.142000	5.142000	5.178000	5.128000	5.091000	5.064000	4.990000	4.898000	4.914000	4.912000	East Asia & Pacific	Lower middle income
257	Samoa	WSM	2.667000	3.014000	3.278000	3.656000	3.952000	4.296000	4.505000	4.889000	5.232000	5.700000	6.095000	6.167000	6.235000	6.207000	6.279000	6.088000	6.026000	6.120000	6.236000	6.508000	6.691000	10.345000	10.355000	10.350000	10.265000	10.293000	9.970000	9.806000	9.837000	9.797000	East Asia & Pacific	Upper middle income
259	Yemen, Rep.	YEM	3.728000	3.832000	3.833000	3.877000	4.561000	5.341000	6.207000	7.146000	8.157000	8.936000	9.696000	10.629000	11.572000	12.460000	13.378000	14.102000	15.009000	16.077999	17.962999	19.591999	20.389999	22.830000	24.614000	26.115000	25.295000	25.466999	25.469000	25.306999	24.879999	24.462000	Middle East & North Africa	Low income
260	South Africa	ZAF	35.853001	35.848000	36.375999	36.492001	36.417000	36.494999	36.243000	36.069000	36.351002	36.330002	36.787998	41.546001	39.480000	36.366001	36.550999	35.971001	33.179001	25.836000	25.590000	27.032000	27.247000	27.087000	26.547001	27.045000	27.542999	29.006001	29.297001	29.059999	30.334999	30.809999	Sub-Saharan Africa	Upper middle income
261	Zambia	ZMB	22.099001	21.347000	20.601000	18.608000	17.459999	15.826000	13.489000	11.494000	11.612000	11.432000	12.078000	12.867000	13.739000	14.416000	15.054000	12.345000	9.842000	7.705000	10.413000	13.275000	10.429000	8.007000	8.864000	9.731000	10.687000	11.629000	12.570000	12.448000	12.237000	11.981000	Sub-Saharan Africa	Lower middle income
262	Zimbabwe	ZWE	2.791000	2.819000	2.940000	2.955000	3.542000	4.286000	5.000000	4.791000	4.596000	4.633000	4.664000	4.679000	4.629000	4.525000	4.639000	4.711000	4.781000	4.919000	5.508000	5.793000	5.990000	5.984000	5.984000	5.947000	5.895000	5.841000	5.739000	5.598000	5.458000	5.522000	Sub-Saharan Africa	Lower middle income
In [73]:
#how many partipant countries
CC_countries = merged_data_clean['Country Code'].unique()
print(CC_countries)
print(len(CC_countries))
['AFG' 'AGO' 'ALB' 'ARE' 'ARG' 'ARM' 'AUS' 'AUT' 'AZE' 'BDI' 'BEL' 'BEN'
 'BFA' 'BGD' 'BGR' 'BHR' 'BHS' 'BIH' 'BLR' 'BLZ' 'BOL' 'BRA' 'BRB' 'BRN'
 'BTN' 'BWA' 'CAF' 'CAN' 'CHE' 'CHI' 'CHL' 'CHN' 'CIV' 'CMR' 'COD' 'COG'
 'COL' 'COM' 'CPV' 'CRI' 'CUB' 'CYP' 'CZE' 'DEU' 'DJI' 'DNK' 'DOM' 'DZA'
 'ECU' 'EGY' 'ERI' 'ESP' 'EST' 'ETH' 'FIN' 'FJI' 'FRA' 'GAB' 'GBR' 'GEO'
 'GHA' 'GIN' 'GMB' 'GNB' 'GNQ' 'GRC' 'GTM' 'GUM' 'GUY' 'HKG' 'HND' 'HRV'
 'HTI' 'HUN' 'IDN' 'IND' 'IRL' 'IRN' 'IRQ' 'ISL' 'ISR' 'ITA' 'JAM' 'JOR'
 'JPN' 'KAZ' 'KEN' 'KGZ' 'KHM' 'KOR' 'KWT' 'LAO' 'LBN' 'LBR' 'LBY' 'LCA'
 'LKA' 'LSO' 'LTU' 'LUX' 'LVA' 'MAC' 'MAR' 'MDA' 'MDG' 'MDV' 'MEX' 'MKD'
 'MLI' 'MLT' 'MMR' 'MNE' 'MNG' 'MOZ' 'MRT' 'MUS' 'MWI' 'MYS' 'NAM' 'NCL'
 'NER' 'NGA' 'NIC' 'NLD' 'NOR' 'NPL' 'NZL' 'OMN' 'PAK' 'PAN' 'PER' 'PHL'
 'PNG' 'POL' 'PRI' 'PRK' 'PRT' 'PRY' 'PSE' 'PYF' 'QAT' 'ROU' 'RUS' 'RWA'
 'SAU' 'SDN' 'SEN' 'SGP' 'SLB' 'SLE' 'SLV' 'SOM' 'SRB' 'SSD' 'STP' 'SUR'
 'SVK' 'SVN' 'SWE' 'SWZ' 'SYR' 'TCD' 'TGO' 'THA' 'TJK' 'TKM' 'TLS' 'TON'
 'TTO' 'TUN' 'TUR' 'TZA' 'UGA' 'UKR' 'URY' 'USA' 'UZB' 'VCT' 'VEN' 'VIR'
 'VNM' 'VUT' 'WSM' 'YEM' 'ZAF' 'ZMB' 'ZWE']
187
In [74]:
CC_IG = merged_data_clean[['Country Code','IncomeGroup']]
print(CC_IG)
print(len(CC_IG))
    Country Code          IncomeGroup
1            AFG           Low income
2            AGO  Lower middle income
3            ALB  Upper middle income
6            ARE          High income
7            ARG  Upper middle income
..           ...                  ...
257          WSM  Upper middle income
259          YEM           Low income
260          ZAF  Upper middle income
261          ZMB  Lower middle income
262          ZWE  Lower middle income

[187 rows x 2 columns]
187
In [75]:
CC_IG_GP= CC_IG.groupby(['Country Code']).describe()
print(CC_IG_GP)
print(len(CC_IG_GP))
             IncomeGroup                                 
                   count unique                  top freq
Country Code                                             
AFG                    1      1           Low income    1
AGO                    1      1  Lower middle income    1
ALB                    1      1  Upper middle income    1
ARE                    1      1          High income    1
ARG                    1      1  Upper middle income    1
...                  ...    ...                  ...  ...
WSM                    1      1  Upper middle income    1
YEM                    1      1           Low income    1
ZAF                    1      1  Upper middle income    1
ZMB                    1      1  Lower middle income    1
ZWE                    1      1  Lower middle income    1

[187 rows x 4 columns]
187
In [76]:
IG_IG_GP= CC_IG.groupby(['IncomeGroup']).describe()
print(IG_IG_GP)
print(len(IG_IG_GP))
                    Country Code                 
                           count unique  top freq
IncomeGroup                                      
High income                   60     60  TTO    1
Low income                    29     29  GNB    1
Lower middle income           48     48  BTN    1
Upper middle income           50     50  BLZ    1
4
In [77]:
CC_IG_2019 = merged_data_clean[['Country Code','2019','IncomeGroup']]
print(CC_IG_2019)
print(len(CC_IG_2019))
    Country Code       2019          IncomeGroup
1            AFG  14.004000           Low income
2            AGO   6.942000  Lower middle income
3            ALB  11.604000  Upper middle income
6            ARE   6.046000          High income
7            ARG  10.922000  Upper middle income
..           ...        ...                  ...
257          WSM   9.837000  Upper middle income
259          YEM  24.879999           Low income
260          ZAF  30.334999  Upper middle income
261          ZMB  12.237000  Lower middle income
262          ZWE   5.458000  Lower middle income

[187 rows x 3 columns]
187
In [78]:
import statistics
grouped_data_income = merged_data_clean.groupby(['IncomeGroup']).mean()
grouped_data_income
#grouped_data_income_2019= grouped_data_income(['2019'])
#Data2019=merged_data_clean['2019']
#mode = statistics.mode(transpose_income_group.groupby([]))
#print(mode)
Out[78]:
1991	1992	1993	1994	1995	1996	1997	1998	1999	2000	2001	2002	2003	2004	2005	2006	2007	2008	2009	2010	2011	2012	2013	2014	2015	2016	2017	2018	2019	2020
IncomeGroup																														
High income	7.736467	8.250783	9.243850	9.437617	9.237233	9.212950	8.843950	8.693183	8.703783	8.479150	8.252700	8.419200	8.672367	8.563100	8.345683	7.703300	7.011633	6.799433	8.279067	8.769983	8.888267	9.132533	9.244683	8.827083	8.370100	7.900950	7.301283	6.753733	6.569350	6.594800
Low income	5.191034	5.122586	5.241103	5.521621	5.722138	5.862035	5.939793	6.142310	6.236034	6.476931	6.796276	6.910207	6.998379	7.046828	7.092172	7.077345	7.154931	7.056069	7.421069	7.645621	7.609897	7.646276	7.719931	7.709862	7.734690	7.701448	7.599138	7.491828	7.458897	7.440862
Lower middle income	7.770750	7.832417	8.115313	8.255917	8.423167	8.522042	8.436583	8.552500	8.596146	8.522167	8.632187	8.800479	8.815729	8.629063	8.552938	8.074896	7.795771	7.745271	8.212021	8.366792	8.326208	8.061479	8.041271	7.941438	8.267479	8.405146	8.481042	8.283375	8.333917	8.284333
Upper middle income	11.855680	11.943880	12.369380	12.909100	13.475140	13.965980	14.108920	14.097900	14.203700	13.907960	13.776640	14.130820	13.996420	13.734020	13.419940	12.613340	11.910920	11.348980	12.007120	12.239020	12.157240	12.124720	12.233620	12.294740	12.246720	12.158600	11.795040	11.403020	11.610500	11.690940
In [79]:
income_groups
print(income_groups)
income_groups[0]
for income_group in income_groups:
  print(income_group)
['Low income' 'Lower middle income' 'Upper middle income' 'High income']
Low income
Lower middle income
Upper middle income
High income
In [80]:
income_group_data=[]
for i in range (len(income_groups)):
  income_group_data.append(merged_data_clean['2019'][merged_data_clean['IncomeGroup']==income_groups[i]])
In [81]:
income_group_data
Out[81]:
[1      14.004000
 14      1.009000
 17      9.223000
 32      3.565000
 41      3.404000
 67      4.953000
 70      2.756000
 83      3.316000
 84     12.237000
 85      2.210000
 98     16.693001
 128     2.224000
 148     1.846000
 155     7.954000
 162     3.453000
 165     6.617000
 170     0.360000
 190     2.331000
 200     1.070000
 203    27.768999
 207     3.628000
 210    11.079000
 213    13.406000
 224    20.837000
 226     1.706000
 229     1.547000
 231     9.935000
 244     2.296000
 259    24.879999
 Name: 2019, dtype: float64, 2       6.942000
 16      2.336000
 18      6.204000
 26      3.809000
 30      3.200000
 39      3.772000
 40      3.872000
 42     10.105000
 44      4.702000
 45     11.491000
 54     10.402000
 58     21.080000
 65     22.150000
 81      4.464000
 95      7.003000
 107     5.233000
 118     2.800000
 119     7.467000
 120     0.824000
 126     0.569000
 135     6.687000
 138    27.118000
 145    10.415000
 147     4.155000
 157     2.074000
 160     5.630000
 163    12.112000
 171     8.908000
 172     6.696000
 175     1.210000
 181     5.596000
 184     2.442000
 186     1.378000
 193    40.945000
 204     7.446000
 206     0.595000
 208     3.578000
 216    20.919001
 221    23.660999
 234     6.169000
 240    23.410000
 243     2.462000
 245     7.796000
 249     5.688000
 254     1.904000
 255     4.914000
 261    12.237000
 262     5.458000
 Name: 2019, dtype: float64, 3      11.604000
 7      10.922000
 8      17.295000
 13      6.335000
 19      3.969000
 22     21.002001
 23      3.350000
 24      9.653000
 27     14.139000
 31     21.222000
 38      3.727000
 43     12.713000
 46     14.846000
 48      1.776000
 57      8.279000
 64      4.958000
 74      5.320000
 78     28.507000
 80     12.880000
 86      6.184000
 90      3.412000
 92     15.189000
 104     4.510000
 109    18.566999
 110    30.403999
 114    10.807000
 115    23.344999
 117     5.222000
 127     9.880000
 129    24.570000
 130    23.256001
 149     5.866000
 151     3.712000
 154    17.049000
 159    15.730000
 166     3.740000
 168    19.686001
 183     3.409000
 192     5.784000
 199     4.462000
 211    13.780000
 217    11.923000
 230     0.742000
 232     2.179000
 236     2.027000
 241    16.422001
 250    16.679001
 251     9.320000
 257     9.837000
 260    30.334999
 Name: 2019, dtype: float64, 6       6.046000
 11      5.328000
 12      4.547000
 15      5.136000
 20      2.963000
 21     10.801000
 28     10.692000
 29      9.907000
 33      5.263000
 35      4.768000
 36      7.695000
 37      7.702000
 51      8.122000
 52      2.384000
 53      2.694000
 56      5.194000
 68     16.204000
 69      4.966000
 73      6.296000
 75      8.364000
 79      3.575000
 87     22.016001
 91      5.305000
 94      3.198000
 97      8.644000
 99      3.480000
 108     4.674000
 111     2.835000
 112     3.898000
 113    10.838000
 116     2.145000
 123     3.898000
 124     5.524000
 140     6.305000
 141     5.890000
 142     5.631000
 143     1.799000
 156     3.774000
 164     9.999000
 169    14.080000
 173     3.362000
 174     3.244000
 177     4.449000
 179    11.910000
 182     5.055000
 187     3.509000
 189     6.214000
 191     6.946000
 196    13.227000
 197     0.432000
 198     3.423000
 202    22.114000
 205     4.314000
 218     6.182000
 219     4.660000
 220     6.220000
 239     2.825000
 247    10.658000
 248     3.616000
 253     9.221000
 Name: 2019, dtype: float64]
In [82]:
statistic, pvalue = stats.f_oneway(income_group_data[0],
                                   income_group_data[1],
                                   income_group_data[2],
                                   income_group_data[3])
print("statistic: %s pvalue %s" %(statistic,pvalue))
statistic: 5.052886221006664 pvalue 0.0021894356280740147
In [83]:
regions = merged_data_clean['Region'].unique()
print(regions)


regions_data=[]
for i in range (len(regions)):
  regions_data.append(merged_data_clean['2019'][merged_data_clean['Region']==regions[i]])



statistic, pvalue = stats.f_oneway(regions_data[0],
                                   regions_data[1],
                                   regions_data[2],
                                   regions_data[3],regions_data[4],
                                   regions_data[5],regions_data[6])
print("statistic: %s pvalue %s" %(statistic,pvalue))
['South Asia' 'Sub-Saharan Africa' 'Europe & Central Asia'
 'Middle East & North Africa' 'Latin America & Caribbean'
 'East Asia & Pacific' 'North America']
statistic: 7.031185985789595 pvalue 9.75436578876421e-07
In [84]:
fig = plt.figure(figsize=(20,10))
ax = fig.add_subplot(111)
ax.set_title("Boxplot of % Female Unemployment by Income  Group")
ax.set
ax.boxplot(income_group_data, labels = income_groups, showmeans =True)
plt.xlabel("Country Income Group")
plt.ylabel("% Female Unemployment")
plt.show()
/usr/local/lib/python3.6/dist-packages/numpy/core/_asarray.py:83: VisibleDeprecationWarning: Creating an ndarray from ragged nested sequences (which is a list-or-tuple of lists-or-tuples-or ndarrays with different lengths or shapes) is deprecated. If you meant to do this, you must specify 'dtype=object' when creating the ndarray
  return array(a, dtype, copy=False, order=order)

Median is the orange line in graph above
The mean for each group is the green triangle
ANOVA Step by Step
Defining ANOVA
Analysis of variance (ANOVA) is a statistical technique that is used to check if the means of two or more groups are significantly different from each other. ANOVA checks the impact of one or more factors by comparing the means of different samples. Analytics Vidhya%20is,the%20means%20of%20different%20samples.&text=Another%20measure%20to%20compare%20the%20samples%20is%20called%20a%20t%2Dtest)

Objective
In our case, the impact of the factor Income Group to the different means of the % Female Unemployment will be analysed.

Hypotesis testing in ANOVA:
Hypothesis Testing - Analysis of Variance (ANOVA)

The null hypothesis in ANOVA is always that there is no difference in means.

H0: mu1=mu2=mu3=mu4

The alternative hypothesis is always that the means are not all equal

H1: means are not all equal

Test Statistic for ANOVA
The test statistic for testing H0: μ1 = μ2 = ... = μk is:

equation_image35.gif

And follows the table of calculations:

ANOVACalculations1.jpg

ANOVA Step By Step
Assumptions while calculating test statisitic F:

Equal variability in the 4 income groups (e.g population variances are equal s1^2 = s2^2 = s3^2= s4^2
Sample Data
Groups are the income groups: IG1, IG2, IG3, IG4

Sample Size for each group: n1 =60, n2=29, n3=48,n4=50

Sample mean: mu1, mu2, mu3, mu4

Sample standard deviation: s1,s2,s3,s4

Preparing all sample data (K= number of samples, N=population, degrees of freedom 1 and 2
In [127]:
k=len(pd.unique(merged_data_clean.IncomeGroup)) 
N=len(merged_data_clean.values) 
#Degrees of Freedom Between Treatments
df1= k-1
#Degrees of Freedom within Treatments
df2 = N-k
#Total Degrees of Freedom
dfT=N-1
print(k,N, df1, df2, dfT)
4 187 3 183 186
Another way to find the Income Groups
Calculating the populations of each income group and N
In [87]:
n0=merged_data_clean.groupby('IncomeGroup').size()[0]  
n1=merged_data_clean.groupby('IncomeGroup').size()[1]  
n2=merged_data_clean.groupby('IncomeGroup').size()[2] 
n3=merged_data_clean.groupby('IncomeGroup').size()[3] 
N=n0+n1+n2+n3
print(n0,n1,n2,n3,n0+n1+n2+n3)
60 29 48 50 187
In [148]:
grandmu=(merged_data_clean['2019'].sum()/N)
print(grandmu)
8.50813905424613
Sum of Squares Between Samples
We start by calculating the Sum of Squares between. Sum of Squares Between is the variability due to interaction between the groups. Sometimes known as the Sum of Squares of the Model.

In [149]:
print(n0,n1,n2,n3)
merged_data_clean_IG_2019=merged_data_clean[['2019','IncomeGroup']]
IG0=merged_data_clean_IG_2019[merged_data_clean_IG_2019['IncomeGroup']=='High income']
IG1=merged_data_clean_IG_2019[merged_data_clean_IG_2019['IncomeGroup']=='Low income']
IG2=merged_data_clean_IG_2019[merged_data_clean_IG_2019['IncomeGroup']=='Lower middle income']
IG3=merged_data_clean_IG_2019[merged_data_clean_IG_2019['IncomeGroup']=='Upper middle income']

IG0mu_2=((sum(IG0['2019']))/n0)
IG1mu_2=((sum(IG1['2019']))/n1)
IG2mu_2=((sum(IG2['2019']))/n2)
IG3mu_2=((sum(IG3['2019']))/n3)

SSB= n0*(IG0mu_2 -grandmu )**2 + n1*(IG1mu_2-grandmu)**2 + n2*(IG2mu_2-grandmu)**2 + n3*(IG3mu_2-grandmu)**2
SSB
60 29 48 50
Out[149]:
740.1497027860728
Mean Square for SSB
In [150]:
MSSB= SSB/df1
MSSB
Out[150]:
246.7165675953576
How to Calculate the Sum of Squares Within
The variability in the data due to differences within each group.

In [177]:
import statistics
IG0_ss=statistics.pvariance(IG0['2019'])
IG1_ss=statistics.pvariance(IG1['2019'])
IG2_ss=statistics.pvariance(IG2['2019'])
IG3_ss=statistics.pvariance(IG3['2019'])
#this calculation was not included in SSW. it didnt result in an accurate
In [178]:
def variance(datav, ddof=1):
      n = len(datav)
      mean = sum(datav) / n
      return sum((x - mean) ** 2 for x in datav) / (n - ddof)
IG0_sss=variance(IG0['2019'])
IG1_sss=variance(IG1['2019'])
IG2_sss=variance(IG2['2019'])
IG3_sss=variance(IG3['2019'])
In [180]:
SSW=(n0-1)*IG0_sss +(n1-1)*IG1_sss +(n2-1)*IG2_sss +(n3-1)*IG3_sss 
SSW
Out[180]:
8935.315361396688
Mean Square for SSW
In [181]:
MSSW = SSW/df2
MSSW
Out[181]:
48.826859898342555
Calculating the F-value
In [182]:
F=MSSB/MSSW
F
Out[182]:
5.0528862210066565
Calculating p
In [188]:
from scipy import stats
p= stats.f.sf(F,dfT, df2)
p
Out[188]:
8.001863810066076e-26
Interpretation of F
One rejects the the null hypothesis, H0 , if the computed F-static is greater than the critical F-statistic. The critical F-statistic is determined by the degrees of freedom and alpha value. In our case, 1-tailed , alpha= 0,05, dof = 186 so critical F = 2.347

Reject H0 if calulated F-statistics > critical F-statistic: 5.05 > 2.347

We reject the null hypothesis H0 because p<= 0.05

The % of Female Unemployment rate was measured across different income groups.

The purpose of calculating ANOVA was to see if averages of the values of % of Female Unemployment across the different Income Groups were statistically different.

We can now report that Income Group factor greatly alters the average of the % Female Unployment rate for the year 2019.

In [185]:
!pip install researchpy
Requirement already satisfied: researchpy in /usr/local/lib/python3.6/dist-packages (0.2.3)
Requirement already satisfied: scipy in /usr/local/lib/python3.6/dist-packages (from researchpy) (1.4.1)
Requirement already satisfied: numpy in /usr/local/lib/python3.6/dist-packages (from researchpy) (1.19.4)
Requirement already satisfied: statsmodels in /usr/local/lib/python3.6/dist-packages (from researchpy) (0.10.2)
Requirement already satisfied: pandas in /usr/local/lib/python3.6/dist-packages (from researchpy) (1.1.5)
Requirement already satisfied: patsy>=0.4.0 in /usr/local/lib/python3.6/dist-packages (from statsmodels->researchpy) (0.5.1)
Requirement already satisfied: python-dateutil>=2.7.3 in /usr/local/lib/python3.6/dist-packages (from pandas->researchpy) (2.8.1)
Requirement already satisfied: pytz>=2017.2 in /usr/local/lib/python3.6/dist-packages (from pandas->researchpy) (2018.9)
Requirement already satisfied: six in /usr/local/lib/python3.6/dist-packages (from patsy>=0.4.0->statsmodels->researchpy) (1.15.0)
In [184]:
import researchpy as rp
rp.summary_cont(merged_data_clean['2019'])

Out[184]:
Variable	N	Mean	SD	SE	95% Conf.	Interval
0	2019	187.0	8.5081	7.2124	0.5274	7.4676	9.5486
In [186]:
rp.summary_cont(merged_data_clean_IG_2019.groupby(merged_data_clean_IG_2019['IncomeGroup']))

Out[186]:
2019
N	Mean	SD	SE	95% Conf.	Interval
IncomeGroup						
High income	60	6.5694	4.3386	0.5601	5.4486	7.6901
Low income	29	7.4589	7.4468	1.3828	4.6263	10.2915
Lower middle income	48	8.3339	8.2012	1.1837	5.9525	10.7153
Upper middle income	50	11.6105	7.9678	1.1268	9.3461	13.8749
Calculation of Sum of Squares Total
Sum of Squares Total will be needed to calculate eta-squared later. This is the total variability in the data:

In [139]:
SStotal = SSB+SSW
SStotal
Out[139]:
9475.338487574169
ANOVA with Pingouin
Install the library

In [128]:
#One-Way ANOVA 
!pip install pingouin
import pingouin as pg
Collecting pingouin
  Downloading https://files.pythonhosted.org/packages/e6/5f/4618f878765a8b7037b8831f19105c5c2764b26e5e9afa4a29c58fc11d26/pingouin-0.3.8.tar.gz (223kB)
     |████████████████████████████████| 225kB 6.3MB/s 
Requirement already satisfied: numpy>=1.15 in /usr/local/lib/python3.6/dist-packages (from pingouin) (1.19.4)
Requirement already satisfied: scipy>=1.3 in /usr/local/lib/python3.6/dist-packages (from pingouin) (1.4.1)
Requirement already satisfied: pandas>=0.24 in /usr/local/lib/python3.6/dist-packages (from pingouin) (1.1.5)
Requirement already satisfied: matplotlib>=3.0.2 in /usr/local/lib/python3.6/dist-packages (from pingouin) (3.2.2)
Requirement already satisfied: seaborn>=0.9.0 in /usr/local/lib/python3.6/dist-packages (from pingouin) (0.11.0)
Requirement already satisfied: statsmodels>=0.10.0 in /usr/local/lib/python3.6/dist-packages (from pingouin) (0.10.2)
Requirement already satisfied: scikit-learn in /usr/local/lib/python3.6/dist-packages (from pingouin) (0.22.2.post1)
Collecting pandas_flavor>=0.1.2
  Downloading https://files.pythonhosted.org/packages/9a/57/7fbcff4c0961ed190ac5fcb0bd8194152ee1ee6487edf64fdbae16e2bc4b/pandas_flavor-0.2.0-py2.py3-none-any.whl
Collecting outdated
  Downloading https://files.pythonhosted.org/packages/86/70/2f166266438a30e94140f00c99c0eac1c45807981052a1d4c123660e1323/outdated-0.2.0.tar.gz
Requirement already satisfied: tabulate in /usr/local/lib/python3.6/dist-packages (from pingouin) (0.8.7)
Requirement already satisfied: pytz>=2017.2 in /usr/local/lib/python3.6/dist-packages (from pandas>=0.24->pingouin) (2018.9)
Requirement already satisfied: python-dateutil>=2.7.3 in /usr/local/lib/python3.6/dist-packages (from pandas>=0.24->pingouin) (2.8.1)
Requirement already satisfied: cycler>=0.10 in /usr/local/lib/python3.6/dist-packages (from matplotlib>=3.0.2->pingouin) (0.10.0)
Requirement already satisfied: pyparsing!=2.0.4,!=2.1.2,!=2.1.6,>=2.0.1 in /usr/local/lib/python3.6/dist-packages (from matplotlib>=3.0.2->pingouin) (2.4.7)
Requirement already satisfied: kiwisolver>=1.0.1 in /usr/local/lib/python3.6/dist-packages (from matplotlib>=3.0.2->pingouin) (1.3.1)
Requirement already satisfied: patsy>=0.4.0 in /usr/local/lib/python3.6/dist-packages (from statsmodels>=0.10.0->pingouin) (0.5.1)
Requirement already satisfied: joblib>=0.11 in /usr/local/lib/python3.6/dist-packages (from scikit-learn->pingouin) (1.0.0)
Requirement already satisfied: xarray in /usr/local/lib/python3.6/dist-packages (from pandas_flavor>=0.1.2->pingouin) (0.15.1)
Collecting littleutils
  Downloading https://files.pythonhosted.org/packages/4e/b1/bb4e06f010947d67349f863b6a2ad71577f85590180a935f60543f622652/littleutils-0.2.2.tar.gz
Requirement already satisfied: requests in /usr/local/lib/python3.6/dist-packages (from outdated->pingouin) (2.23.0)
Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.6/dist-packages (from python-dateutil>=2.7.3->pandas>=0.24->pingouin) (1.15.0)
Requirement already satisfied: setuptools>=41.2 in /usr/local/lib/python3.6/dist-packages (from xarray->pandas_flavor>=0.1.2->pingouin) (51.0.0)
Requirement already satisfied: idna<3,>=2.5 in /usr/local/lib/python3.6/dist-packages (from requests->outdated->pingouin) (2.10)
Requirement already satisfied: chardet<4,>=3.0.2 in /usr/local/lib/python3.6/dist-packages (from requests->outdated->pingouin) (3.0.4)
Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /usr/local/lib/python3.6/dist-packages (from requests->outdated->pingouin) (1.24.3)
Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.6/dist-packages (from requests->outdated->pingouin) (2020.12.5)
Building wheels for collected packages: pingouin, outdated, littleutils
  Building wheel for pingouin (setup.py) ... done
  Created wheel for pingouin: filename=pingouin-0.3.8-cp36-none-any.whl size=221687 sha256=a2dd24e2b28c1437c23ab0c0ac5e4c4eb1c1f12572b3cd680cb99c9f29310981
  Stored in directory: /root/.cache/pip/wheels/d6/9e/53/f885f73f29cf7c8cac3d8f4b1532bbfef2f5eb543946ac9055
  Building wheel for outdated (setup.py) ... done
  Created wheel for outdated: filename=outdated-0.2.0-cp36-none-any.whl size=4961 sha256=3ca93b7c5eefbfc150f00006672a0f0ec253ecf3be7c4df0d0580d76cdaebff3
  Stored in directory: /root/.cache/pip/wheels/fd/7c/ef/814f514d31197310872b5abf353feb8fef9d67ee658e1e7e39
  Building wheel for littleutils (setup.py) ... done
  Created wheel for littleutils: filename=littleutils-0.2.2-cp36-none-any.whl size=7051 sha256=a7df2035d271e6a23b0b62b419974a2d7c287e545eabfb517d55fab82530fa33
  Stored in directory: /root/.cache/pip/wheels/53/16/9f/ac67d15c40243754fd73f620e1b9b6dedc20492ecc19a2bae1
Successfully built pingouin outdated littleutils
Installing collected packages: pandas-flavor, littleutils, outdated, pingouin
Successfully installed littleutils-0.2.2 outdated-0.2.0 pandas-flavor-0.2.0 pingouin-0.3.8
ANOVA Table to study variability of the data (between and within samples)
In [129]:
aov= pg.anova(dv='2019', between='IncomeGroup',data= merged_data_clean,detailed=True)
aov
Out[129]:
Source	SS	DF	MS	F	p-unc	np2
0	IncomeGroup	740.149703	3	246.716568	5.052886	0.002189	0.076498
1	Within	8935.315361	183	48.826860	NaN	NaN	NaN
the impact of the factor Region to the different means of the % Female Unemployment

In [142]:
aov= pg.anova(dv='2019', between='Region',data= merged_data_clean,detailed=True)
aov
Out[142]:
Source	SS	DF	MS	F	p-unc	np2
0	Region	1837.100070	6	306.183345	7.031186	9.754366e-07	0.189872
1	Within	7838.364994	180	43.546472	NaN	NaN	NaN
Creating a Worldmap with Folium
What follows next is unpivoting the main.data from wide to long format, optionally leaving identifiers set using melt function. One column has all the identifiers that later on, we will be selecting only one identifier % Female Unemployement and only one year 2019 of data from the column 'Year'.

In [189]:
main_data_m=main_data.melt(id_vars=['Country Code', 'Indicator Name'],value_vars=['1991', '1992', '1993', '1994', '1995',
       '1996', '1997', '1998', '1999', '2000', '2001', '2002', '2003', '2004',
       '2005', '2006', '2007', '2008', '2009', '2010', '2011', '2012', '2013',
       '2014', '2015', '2016', '2017', '2018', '2019', '2020'])
main_data_m.head()

main_data_m= main_data_m.rename(columns={'variable': 'Year'})

main_data_m.head()
main_data_m_clean=main_data_m.dropna()
main_data_m_clean.head()
Out[189]:
Country Code	Indicator Name	Year	value
0	ABW	Population ages 15-64 (% of total population)	1991	68.523104
1	ABW	Population ages 0-14 (% of total population)	1991	24.084677
78	ABW	Secondary education, duration (years)	1991	5.000000
82	ABW	Educational attainment, at least completed pos...	1991	7.186510
83	ABW	Educational attainment, at least completed pos...	1991	9.047520
In [190]:
import pandas as pd
import folium
import csv
import json
In [191]:
stage = main_data_m_clean
stage
Out[191]:
Country Code	Indicator Name	Year	value
0	ABW	Population ages 15-64 (% of total population)	1991	68.523104
1	ABW	Population ages 0-14 (% of total population)	1991	24.084677
78	ABW	Secondary education, duration (years)	1991	5.000000
82	ABW	Educational attainment, at least completed pos...	1991	7.186510
83	ABW	Educational attainment, at least completed pos...	1991	9.047520
...	...	...	...	...
1282884	ZWE	Labor force, female (% of total labor force)	2020	50.859818
1282956	ZWE	Secondary education, duration (years)	2020	6.000000
1282969	ZWE	Lower secondary school starting age (years)	2020	13.000000
1283012	ZWE	Primary education, duration (years)	2020	7.000000
1283019	ZWE	Primary school starting age (years)	2020	6.000000
546123 rows × 4 columns

In [192]:
main_data_m_clean_year=main_data_m_clean[main_data_m_clean['Year']=='2019']
main_data_m_clean_year_ind=main_data_m_clean_year[main_data_m_clean_year['Indicator Name']=='Unemployment, female (% of female labor force) (modeled ILO estimate)']
main_data_m_clean_year_ind
Out[192]:
Country Code	Indicator Name	Year	value
1197670	AFG	Unemployment, female (% of female labor force)...	2019	14.004000
1197832	AGO	Unemployment, female (% of female labor force)...	2019	6.942000
1197994	ALB	Unemployment, female (% of female labor force)...	2019	11.604000
1198318	ARB	Unemployment, female (% of female labor force)...	2019	19.954200
1198480	ARE	Unemployment, female (% of female labor force)...	2019	6.046000
...	...	...	...	...
1239304	WSM	Unemployment, female (% of female labor force)...	2019	9.837000
1239628	YEM	Unemployment, female (% of female labor force)...	2019	24.879999
1239790	ZAF	Unemployment, female (% of female labor force)...	2019	30.334999
1239952	ZMB	Unemployment, female (% of female labor force)...	2019	12.237000
1240114	ZWE	Unemployment, female (% of female labor force)...	2019	5.458000
233 rows × 4 columns

In [ ]:
data_to_plot = main_data_m_clean_year_ind[['Country Code','value']]
data_to_plot['Country Code'].unique()
In [194]:
data_to_plot
Out[194]:
Country Code	value
1197670	AFG	14.004000
1197832	AGO	6.942000
1197994	ALB	11.604000
1198318	ARB	19.954200
1198480	ARE	6.046000
...	...	...
1239304	WSM	9.837000
1239628	YEM	24.879999
1239790	ZAF	30.334999
1239952	ZMB	12.237000
1240114	ZWE	5.458000
233 rows × 2 columns

In [195]:
hist_indicator = main_data_m_clean_year_ind.iloc[0]['Indicator Name']
hist_indicator
Out[195]:
'Unemployment, female (% of female labor force) (modeled ILO estimate)'
In [196]:
!wget --quiet https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DV0101EN/labs/Data_Files/world_countries.json
print('GeoJSON file downloaded!')
GeoJSON file downloaded!
In [197]:
wc=r'world_countries.json'
In [198]:
world = folium.Map(location=[0, 0], zoom_start=2)
In [199]:
# choropleth maps bind Pandas Data Frames and json geometries.
world.choropleth(geo_data =wc ,
                data = data_to_plot,
                columns = 
['Country Code', 'value'],

key_on='feature.id',
fill_color = 'YlOrRd',
                  fill_opacity =0.8 ,
                  line_opacity = 0.1,
legend_name ='%Female Unemployment')
world
/usr/local/lib/python3.6/dist-packages/folium/folium.py:426: FutureWarning: The choropleth  method has been deprecated. Instead use the new Choropleth class, which has the same arguments. See the example notebook 'GeoJSON_and_choropleth' for how to do this.
  FutureWarning
Out[199]:
Make this Notebook Trusted to load map: File -> Trust Notebook
