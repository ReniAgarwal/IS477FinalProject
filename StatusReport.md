## Task Update  

### Overview of the Plan  
In our original project plan we laid out six main steps for studying the relationship between the world happiness data and the mental health data. We planned to explore both datasets, clean and align them, merge them by country and year, build models to look for predictors, interpret the results, and then turn everything into a final report and presentation.

### Data Cleaning and Integration  
So far we have completed the first three steps and started some early exploratory analysis. In the Project-Check notebook in our repository, we load the mental health disorder data and filter it so we keep only the prevalence percentages for conditions like depression, anxiety, drug use disorders, and alcohol use disorders. We then standardize country names and years so they can match the World Happiness data.

For the World Happiness dataset, we combine the separate files for 2015 through 2019, rename the key columns to a consistent set of features such as happiness score, GDP per capita, social support, healthy life expectancy, freedom, generosity, and perceptions of corruption, and align country names and years.

After cleaning both sources, we merge them into a single dataset based on country and year. The merged table currently has a little over four hundred rows and includes both the happiness indicators and the mental health prevalence percentages. This merged dataset is now the main artifact we will use for the rest of the project and it is created and stored through the Project-Check notebook.

### Current Visualizations and Findings  
We have also begun using the merged data for visual exploration. In the same notebook we generate world maps of happiness scores for 2015, 2016, and 2017, which are saved in the repository as image files such as WorldHappiness2015.png, WorldHappiness2016.png, and WorldHappiness2017.png. These maps give us a quick sense of which regions tend to report higher or lower happiness scores.

In addition, we created a scatter plot for 2017 that compares the happiness score of each country with the percentages of people experiencing anxiety, drug use disorders, and depression. This figure, saved as MentalHealthVHappinessScore2017.png, starts to show how mental health outcomes may relate to reported happiness across countries.

### Next Steps  
Going forward, we will use this merged dataset and the insights from these first visualizations to move into step four of our plan. Our next focus will be building and testing simple predictive models that use happiness indicators to predict the prevalence of depression or anxiety. We will then refine our visualizations and narrative based on those results and begin drafting the final report and presentation.

---

## Timeline Update  

At the start of the semester we planned our work in six steps: initial exploration, cleaning and preparation, merging, modeling, interpretation, and final presentation. Our current progress is close to what we originally planned, but the cleaning and merging stage took more time than we first expected.

During the first few weeks we focused on understanding both datasets and confirming that they could be linked by country and year. Over the last couple of weeks we concentrated on cleaning and aligning the data, writing the Project-Check notebook, and generating the first set of figures using the merged dataset. This covers steps one through three from the project plan.

Over the next one to two weeks we plan to work on step four by building simple regression or classification models that try to predict mental health disorder prevalence from happiness indicators such as GDP per capita, social support, and healthy life expectancy. We will start with a small number of target variables, most likely depression and anxiety, and evaluate how well the models perform.

In the final phase of the project we will move into steps five and six. We will summarize the main patterns we find, refine our charts so they are clear to a general audience, and write the final report. We will also prepare a short presentation that walks through the data, the methods we used, and what we learned. At this point we feel that we are on track to complete the remaining work if we continue to build on the merged dataset and the current notebook.


## Changes to Project Plan and Reflection  

The core idea of the project and the main research questions have stayed the same. We still want to understand how happiness and well being relate to the prevalence of mental health disorders across countries and over time. However, there have been some adjustments to the details of our plan as we worked with the data and looked at the overlap between the two sources.

One change is that we decided to focus our first round of visualizations on the years 2015 through 2017, where we have strong overlap between the World Happiness data and the mental health prevalence data. The original plan was more open ended about the time period, but after looking at the data we realized that trying to cover every possible year would stretch the project and make it harder to tell a clear story. Narrowing the time frame helps us keep the analysis manageable and still answer our questions.

Another change is that data cleaning became a larger part of the project than we expected. The two datasets use different country name conventions and slightly different structures across years, so we wrote more detailed cleaning code, including a function to normalize country names and a step that filters the mental health file to keep only percentage based rows. This extra work was not spelled out in the original plan, but it turned out to be important so that the merged dataset would be reliable.

The feedback we received on the earlier milestone emphasized being clear about how we link data sources and documenting our choices. In response we added comments in the notebook, kept the file names of our figures descriptive, and wrote this status report with specific references to artifacts in the repository. Overall, the project is still aligned with the original goals, but we now have a better sense of where the time goes and which parts of the workflow require the most care.


## Team Member Contributions  

### Dhruval  
Dhruval has focused mainly on the data engineering and analysis side of the project. For this milestone he wrote the code that loads the mental health dataset, filters it to keep the prevalence percentages, and standardizes the column names. He also set up the cleaning and standardization of the World Happiness files from 2015 through 2019, including the logic for renaming the columns and creating a consistent year variable.  

Dhruval implemented the function that cleans country names so they can be matched across both datasets and then used it to merge the two sources into a single table. He also created the first version of the merged dataset that the visualizations are based on. In addition, he drafted most of the technical parts of this status report and checked that the written description matches what the code currently does.

### Reni  
Reni has focused on visualizations and reporting. For this milestone he used the merged dataset that Dhruval prepared to build clear figures that show how happiness and mental health vary across countries. He produced the world maps of happiness scores for 2015, 2016, and 2017 and saved them as image files in the repository. He also created and refined the scatter plot that compares happiness scores with the percentages of people experiencing anxiety, drug use disorders, and depression.

On the reporting side, Reni helped shape the narrative of the project by working on the written explanation of the figures and by editing sections of the status report so they are easier to read. He has focused on making sure that someone who looks at the graphs and the report can understand the main ideas without needing to dig into the raw code.
