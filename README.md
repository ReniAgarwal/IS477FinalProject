
## Mental Health and Happiness: Integrating Country-Level Indicators from Our World in Data and the World Happiness Report
## Made by: Dhruval Patel and Reni Agarwal



**Summary:**

This project explores how national mental health outcomes relate to country level happiness scores. People often assume that happier countries must have better mental health, but it is not obvious how strong that relationship is or whether it is consistent across different disorders. Our main goal is to take two widely used public datasets, clean and integrate them into a single reusable table, and then use that table to examine simple but meaningful patterns between mental health prevalence and happiness. By focusing on a small number of clear questions and documenting each data curation step, we aim to create a project that is not only interesting but also easy for others to reproduce, extend, or critique.

We focus on three guiding questions. First, how are country level prevalence rates of common mental and substance use disorders associated with national happiness scores from the World Happiness Report. Second, do these associations look similar across different disorders such as depression, anxiety, alcohol use, drug use, eating disorders, bipolar disorder, and schizophrenia, or do some conditions appear more strongly related to happiness than others. Third, how stable are these relationships across the years where both sources provide overlapping data, specifically 2015 through 2019. Together these questions let us explore whether the simple story that “happier countries have better mental health” holds up when we look more closely at the numbers.

To answer these questions we combined an Our World in Data mental health prevalence table with World Happiness Report data from 2015 to 2019. The mental health dataset reports the percentage of the population with several disorders by country and year. The happiness dataset provides country level happiness scores and related factors such as gross domestic product per capita, social support, healthy life expectancy, freedom, generosity, and perceived corruption. We cleaned both sources by trimming whitespace, converting percentage columns to numeric types, and filtering impossible values. We then standardized country names using a set of explicit replacements so that both tables shared a common cleaned country key. Finally, we merged the datasets on this key and on year to create a long merged table that can be reused for other analyses.

After integration we generated descriptive statistics and several visualizations. These include scatter plots of depression prevalence versus happiness scores for different years, similar plots for other disorders, and basic summaries that show how happiness and mental health prevalence vary across countries and over time. We also examined distributions and simple correlations to get a sense of the overall patterns without committing to a complex statistical model. The figures help us visually inspect whether higher happiness scores tend to coincide with lower reported prevalence and whether any clear clusters or outliers emerge.

In general we see a weak negative relationship between reported depression or anxiety prevalence and happiness scores. Countries with higher reported prevalence tend to have somewhat lower happiness scores, but there is a lot of variation and many exceptions. Some countries with high happiness scores still show notable prevalence of certain disorders, while some lower ranked countries show moderate or low reported prevalence. The patterns also differ by disorder and are not perfectly stable from year to year, which suggests that measurement and context matter as much as any simple link between happiness and mental health.

Overall, our results show that simple public data can be combined to support cross national exploration of mental health and happiness, but they also highlight the limits of this approach. Differences in diagnosis standards, reporting practices, and health system capacity likely influence observed prevalence as much as the true burden of illness. Despite these limitations, the integrated dataset and documented workflow in this repository provide a starting point for students or researchers who want to build more advanced models, add new years or variables, or focus on specific regions. At the same time, the project serves as a concrete example of a small but fully reproducible data curation and analysis pipeline.

**Data profile:**

**Mental health prevalence**

The first dataset comes from Our World in Data and reports prevalence estimates for several mental and substance use disorders by country and year. The original CSV contains many columns (including DALYs, YLDs, and region codes), but for this project we focus on the columns that can be interpreted directly as percentages of the population. The key raw columns are

Entity, Code, Year – country name, country code when available, and calendar year

Schizophrenia (%)

Bipolar disorder (%)

Eating disorders (%)

Anxiety disorders (%)

Drug use disorders (%)

Depression (%)

Alcohol use disorders (%)

In our cleaned table, which we call mental, we:

Convert all disorder columns to numeric types using a coercion step so non-numeric strings become missing values.

Keep only rows where all selected disorder columns contain values that look like valid percentages (for example, less than or equal to 100).

Rename the disorder columns to shorter, consistent names such as schizophrenia_pct, bipolar_pct, eating_disorders_pct, anxiety_pct, drug_use_pct, depression_pct, and alcohol_use_pct.

Convert Year into an integer year column.

Drop rows with missing country or year, since these fields are part of the key we use later when merging with the happiness data.

The result is a country–year panel where each row contains prevalence information for multiple disorders for a given country and year. This table is small enough to keep in CSV form and cleanly join with the World Happiness data using a shared country key and year.

**World Happiness Report 2015–2019**

The second dataset is the World Happiness Report data for years 2015 through 2019. Each year is distributed as a separate CSV file, and the column names and ordering change slightly from one year to the next. We load each file, trim whitespace from the column names, infer the year from the filename, and then map the columns into a common schema.

The cleaned happiness table includes the following variables:

country – country name

year – calendar year (2015–2019)

happiness_score – overall life evaluation score used in the report

gdp_per_capita – GDP per capita (log scale in the original report)

social_support – measure of perceived social support

healthy_life_expectancy – healthy life expectancy at birth

freedom – measure of perceived freedom to make life choices

generosity – measure related to charitable giving

perceptions_of_corruption – perceptions of corruption in government and business

Because different yearly files use labels such as “Happiness Score,” “Happiness.Score,” “Score,” or “Ladder” for similar concepts, we use a small mapping based on lower-cased column names and keywords to standardize them. A similar mapping handles variations in the GDP, social support, and corruption columns. After mapping, we keep only the target columns, drop rows with missing country or year, and cast year to an integer.

Finally, we vertically concatenate the yearly tables into a single happiness table that covers 2015–2019. This unified table allows us to analyze trends over time and makes the merge with the mental health table straightforward.

**Ethical and legal considerations**

Both datasets are aggregate, country-level sources with no individual-level identifiers or microdata. This greatly reduces privacy risks because no single person can be re-identified from our tables. Our World in Data generally publishes material under permissive Creative Commons–style licenses, and the World Happiness Report data are explicitly provided for research and educational use. In this project we use the data only for coursework, credit the original providers in the references, and do not attempt to link them with any sensitive individual-level datasets.

We store a subset of cleaned CSV files and output tables in a Box folder to support reproducibility without pushing large data files directly to GitHub. The README explains exactly where to place these files inside the project structure after downloading them (for example, data/ for input CSVs and images/ for output figures). The Box folder is shared with view access for course staff only. This design respects the terms of use of the original data, keeps the GitHub repository lightweight, and still makes our curated version reproducible for grading and for any future extensions of the project.



**Data Quality:**

This data quality assessment examines one longitudinal mental-health dataset and a collection of World Happiness Index datasets spanning the years 2015 through 2019. Together these datasets offer a rich opportunity for cross-national and cross-temporal analysis of wellbeing, but they differ significantly in structure, consistency, and completeness, which requires careful evaluation before meaningful analytical work can be done.
The mental-health dataset is organized as a clean, long-format panel containing one observation per country per year. Each row includes a country name, a three-letter ISO code, a year, and several prevalence measures, such as depression, anxiety disorders, eating disorders, drug use disorders, and alcohol use disorders. These values appear in plausible epidemiological ranges, and the presence of an ISO code is particularly beneficial for cross-dataset merging, since it provides a standardized identifier that remains stable across years. In contrast, the World Happiness Index files are structured as annual cross-sections. Each file contains one row per country for a specific year and includes a variety of economic and social indicators used to compute a happiness score. The files are generally tidy on their own, but they are not entirely consistent across years: variable names differ slightly, some additional fields appear or disappear from year to year, and column counts vary. As a result, creating a unified dataset for trend analysis requires harmonizing these structural differences.
In terms of completeness, the happiness data tends to be highly populated, with most countries represented and most fields filled. However, during import, at least one file produced a warning due to mixed data types in certain columns, which typically indicates the presence of empty strings or irregular values. These inconsistencies must be resolved because they often mask missing data or hinder statistical modeling. The mental-health dataset is also largely complete but, as is common with global epidemiological time series, some countries have missing years or incomplete reporting. While the missingness does not appear severe, it should be formally assessed using visualization or summary statistics to identify countries whose records are too fragmented to be reliable for year-to-year comparisons.
Data-type integrity is generally strong, with most variables loading as numeric and falling within reasonable ranges. For the happiness datasets, fields such as happiness score, GDP per capita, family or social support metrics, life expectancy contributions, and indicators of freedom, trust, and generosity all appear plausible. The same is true for the mental-health prevalence metrics, which remain within expected small-percentage values. The key issue concerns the mixed-type columns in certain happiness files, which must be cleaned to avoid corrupting quantitative analyses. Once cleaned, all relevant fields should be coerced to numeric types and validated to ensure that no negative values, impossible percentages, or out-of-range scores remain.
Consistency across datasets presents a more significant challenge. The mental-health dataset includes ISO codes, but the happiness datasets rely solely on country names, which vary across years and often appear in multiple legitimate but inconsistent forms. Countries with alternate or politically influenced names, such as “Congo (Brazzaville)” versus “Republic of the Congo”, can easily become mismatched. Furthermore, some entities change over time, such as the separation of Sudan and South Sudan or political shifts in Eastern Europe, meaning that a consistent lookup table will be required. Harmonizing country names across the five years of happiness data is itself necessary before attempting to merge the two collections. Adding a standard identifier to all happiness datasets will reduce ambiguity and ensure that temporal and cross-dataset joins behave as expected.
Assessing accuracy without external verification is difficult, but internal plausibility checks indicate that the data is reasonable overall. Happiness scores follow the patterns reported in published World Happiness Reports for these years, and mental-health prevalences resemble common estimates from global burden of disease studies. No impossible values, duplicates, or contradictory trends are immediately apparent in the previewed data, though a more detailed review of the full files would be advisable.
Several steps will be necessary to prepare these datasets for integrated analysis. Mixed-type and non-numeric fields must be cleaned and converted to uniform numeric formats. Country names in the happiness files should be standardized and mapped to ISO codes so that they match the identifiers in the mental-health dataset. Columns with similar meanings across years should be renamed to a consistent schema to avoid fragmentation of variables. Missing entries should be documented and handled transparently, either through imputation or by filtering out incomplete cases depending on the planned analysis. Finally, because the happiness files contain one row per country per year while the mental-health dataset spans many years, all datasets should be aligned on a consistent year field before merging into a single panel.
Overall, the datasets possess high analytical value and, after careful cleaning, will support robust examinations of how national wellbeing indicators relate to trends in mental-health prevalence. Their primary limitations involve cross-year inconsistencies, data-type irregularities in certain files, and the need for harmonized country identifiers. Addressing these issues through thoughtful preprocessing will allow both the happiness and mental-health data to be combined into a coherent, longitudinal dataset suited for statistical modeling, visualization, and policy analysis.

**Findings:**

The relationship between national happiness and mental health disorders reveals complex patterns across global populations, suggesting that the connection between collective wellbeing and individual psychological challenges is multifaceted and varies considerably by condition type. The 2017 scatter plot examining mental health versus country happiness scores presents a striking observation: there appears to be a negative correlation between reported happiness levels and certain mental health conditions, particularly drug use disorders.
Countries with the highest happiness scores, typically ranging from 7.0 to 7.5, consistently show the lowest percentages of people suffering from drug use issues, typically below 1%. This cluster of high-happiness, low-drug-use countries likely includes Nordic nations and other developed economies that consistently rank at the top of global happiness indices. Conversely, countries with happiness scores in the 3.0-5.0 range show dramatically higher rates of drug use disorders, sometimes exceeding 7% of the population. This suggests that societal factors contributing to lower happiness, such as economic instability, weak social support systems, conflict, or poor governance, may also create conditions conducive to substance abuse.
The relationship between happiness and depression or anxiety disorders presents a more nuanced picture. While depression rates, shown in green on the scatter plot, do trend slightly lower in happier countries, the correlation is weaker than with drug use disorders. Countries across the entire happiness spectrum show depression rates ranging from approximately 3% to 7%, with significant overlap. This suggests that depression affects populations relatively uniformly regardless of national happiness levels, indicating that individual-level factors may play a stronger role than societal conditions.
Anxiety disorders, represented in blue, demonstrate an even more scattered distribution, appearing across all happiness levels from 3.0 to 7.5. Countries with identical happiness scores can have vastly different anxiety prevalence rates, ranging from 4% to over 8%. This wide variation implies that anxiety may be influenced by factors not captured by happiness metrics, such as cultural attitudes toward uncertainty, work-life balance norms, or social expectations.
The world maps from 2015 and 2016 reveal consistent geographic patterns in happiness distribution. North America, Scandinavia, Australia, and parts of South America consistently show the highest happiness scores, represented in yellow-green tones indicating values of 6.5-7.5, while much of Africa and parts of Asia display lower scores in dark blue, ranging from 3.0 to 4.5. Western Europe maintains moderate to high happiness levels, while Eastern Europe and Russia show more moderate scores. The remarkable consistency between 2015 and 2016 suggests that national happiness levels are relatively stable over short time periods, likely reflecting deeply rooted structural factors like economic development, political stability, social safety nets, and cultural values. The few visible changes between years appear minimal, indicating that major shifts in national wellbeing require sustained efforts over extended periods.
These findings suggest that while national happiness correlates strongly with certain mental health outcomes, particularly substance use disorders, it is not a universal predictor of all psychological challenges. The strong inverse relationship with drug use disorders may reflect the protective effects of strong communities, economic opportunity, and effective healthcare systems found in happier nations. However, the more scattered patterns for depression and anxiety indicate these conditions have complex etiologies involving biological, psychological, and social factors that transcend national happiness levels. This complexity underscores the importance of addressing mental health through multifaceted approaches that consider both societal wellbeing and individual-level interventions, recognizing that improving national happiness alone may not sufficiently address the full spectrum of mental health challenges facing populations worldwide.

**Future Work:**

The analysis of mental health data and happiness trends across countries in the mid-2010s has revealed some intriguing patterns that warrant further investigation. While the current study has established a somewhat positive relationship between national happiness scores and various mental health disorders, numerous questions remain unanswered, and several promising avenues for future research have emerged that could deepen our understanding of the complex interplay between societal wellbeing and individual psychological health.
One critical direction for future work involves longitudinal analysis that extends beyond the snapshot approach of examining single years. While the 2015, 2016, and 2017 happiness maps showed remarkable stability, a comprehensive study tracking mental health indicators and happiness scores across the entire decade of the 2010s could reveal important temporal dynamics. Such research could identify whether countries experiencing rapid economic development or political transitions show corresponding shifts in mental health prevalence rates. Additionally, examining whether happiness improvements precede mental health improvements, or vice versa, could help establish causal relationships rather than mere correlations. This temporal dimension is particularly important for understanding whether interventions aimed at improving national happiness actually translate into measurable mental health benefits over time.
Future research should also disaggregate the data to examine within-country variations in mental health and happiness. National averages, while useful for broad comparisons, obscure significant regional, urban-rural, and socioeconomic disparities that exist within countries. A study examining subnational data could reveal whether happiness and mental health patterns observed at the country level hold true across different demographic groups. For instance, do wealthier regions within lower-happiness countries show mental health profiles similar to higher-happiness nations? Are urban populations in developing countries experiencing different mental health challenges than rural populations? Understanding these internal variations could provide more targeted insights for policy interventions and help identify which population segments are most vulnerable to specific mental health conditions.
The current study's finding that anxiety and depression show weaker correlations with national happiness than drug use disorders suggests that different mental health conditions may be influenced by different factors. Future work should investigate the specific societal, cultural, and economic variables that best predict each type of mental health disorder independently. This could involve incorporating additional country-level data such as healthcare spending, unemployment rates, income inequality measures, social safety net strength, cultural individualism versus collectivism scores, and measures of social cohesion. Multivariate regression analyses could help disentangle which factors are most strongly associated with each mental health condition, potentially revealing that anxiety is more closely tied to work culture and job insecurity, while depression might be more strongly linked to social isolation or income inequality.
Another important area for future investigation involves examining the quality and comparability of mental health data across countries. The current analysis assumes that mental health disorders are diagnosed and reported consistently across nations, but significant variations in healthcare infrastructure, diagnostic practices, cultural stigma around mental illness, and data collection methods may affect prevalence estimates. Future work should include a critical assessment of data quality and consider developing adjustment factors or sensitivity analyses that account for these cross-national differences in measurement. Additionally, research could explore alternative data sources such as social media sentiment analysis, pharmaceutical consumption patterns, or workplace disability claims to triangulate mental health trends and validate official statistics.
The role of specific policy interventions deserves dedicated attention in future research. Countries implement vastly different approaches to mental health care, from fully integrated public health systems to fragmented private care models. Comparative policy analysis examining which healthcare models, social support programs, or public health campaigns are most effective at reducing mental health disorder prevalence could provide actionable insights for policymakers. This research could identify best practices from high-happiness countries and assess whether these approaches are transferable to different cultural and economic contexts.
Cultural factors represent another underexplored dimension in the relationship between happiness and mental health. Future studies should investigate how cultural attitudes toward mental illness, help-seeking behaviors, and the cultural construction of happiness itself might influence both the reporting and actual prevalence of mental health disorders. Some cultures may normalize certain psychological experiences that others pathologize, or may experience entirely different manifestations of distress. Qualitative research incorporating interviews and ethnographic methods alongside quantitative data could enrich our understanding of these cultural dynamics.
Climate change and environmental factors present an increasingly relevant area for future investigation. As environmental stressors intensify, research should examine whether countries facing severe climate impacts show changing patterns in mental health disorders, and whether national happiness scores adequately capture the psychological burden of environmental degradation. The mental health implications of climate-related displacement, natural disasters, and resource scarcity deserve systematic study across affected nations.
Finally, future work should explore the potential for developing more sophisticated composite measures that integrate happiness scores with mental health indicators to create holistic wellbeing indices. Such indices could better capture the full spectrum of population psychological health and provide more nuanced country rankings than happiness scores alone. These integrated measures could guide international development priorities and help track progress toward sustainable development goals related to health and wellbeing.
The mid-2010s data on mental health and happiness represents a valuable foundation, but realizing its full potential requires expanded research efforts that examine temporal trends, within-country variations, causal mechanisms, data quality, policy effectiveness, cultural contexts, environmental factors, and integrated measurement approaches. Such comprehensive future research could transform our understanding of population mental health and inform evidence-based strategies for improving psychological wellbeing across diverse national contexts.

**Reproducing:**

**1: Clone the repository**


Open a terminal and run:
 git clone https://github.com/ReniAgarwal/IS477FinalProject.git
cd IS477FinalProject


**2: Check the project structure**


The repo should contain these folders in the root:


data – input CSV files


images – output PNG visualizations


notebooks – Jupyter notebook and markdown reports


**3: Download data from Box**


Go to our shared Box folder: https://uofi.box.com/s/2mu56tzmc8tj76aqjhsafvis7x3cwnpp


Download the contents.


Place the files in the repo as follows:


All CSV files → data/


All PNG image files → images/


Note: the data/ folder is in .gitignore, so these files are not stored in GitHub and must be downloaded from Box.


**4: Create and activate a Python environment**


Create a new environment using conda or python -m venv and activate it.


**5: Install dependencies**


From the root of the repository, run:
 pip install -r requirements.txt


**6: Run the analysis notebook**


Start Jupyter Lab or VS Code.


Open notebooks/Project-Check.ipynb.


Run all cells from top to bottom.


This notebook loads and cleans the mental health and happiness data, integrates them, and generates the analysis and plots used in our report.


**7: Verify the outputs**


After running the notebook you should see:


A merged table linking mental health prevalence and happiness scores by country and year.


Scatter plots of mental health vs. happiness.


Year-specific happiness visualizations saved into the images/ folder.


**References**

**Data sources**

Ritchie, H., Ortiz-Ospina, E., and Roser, M. “Mental Health.” Our World in Data. Accessed December 2025. https://ourworldindata.org/mental-health Our World in Data


Helliwell, J. F., Layard, R., Sachs, J., and De Neve, J. E. World Happiness Report 2015–2019. Sustainable Development Solutions Network. Original reports available at https://worldhappiness.report. Data accessed via Kaggle dataset “World Happiness” (unsdsn/world-happiness): https://www.kaggle.com/datasets/unsdsn/world-happiness Observable


**Software and tools**

Python Software Foundation. Python Language Reference, Version 3.11. https://www.python.org


The pandas development team. pandas: Python Data Analysis Library. https://pandas.pydata.org


Harris, C. R., Millman, K. J., van der Walt, S. J., et al. “Array programming with NumPy.” Nature 585, 357–362 (2020). https://numpy.org


Hunter, J. D. “Matplotlib: A 2D graphics environment.” Computing in Science & Engineering 9(3), 90–95 (2007). https://matplotlib.org


Plotly Technologies Inc. Plotly Python Graphing Library. https://plotly.com/python


Kluyver, T., Ragan-Kelley, B., Pérez, F., et al. “Jupyter Notebooks – a publishing format for reproducible computational workflows.” In Positioning and Power in Academic Publishing (2016). https://jupyter.org

