---
layout: post
title:  "Predicting abstention rate using open data"
subtitle: "how Open Data can help politics"
authors:
  - name:   Alexandre Vallette
    twitter:    https://twitter.com/vallettea
    github_image: "https://avatars3.githubusercontent.com/u/371303?"
    bio: Data-scientist, founder of ANTS, his passion for machine-learning applied to geographical data and networks comes from his Phd in chaos theory. Open-data enthusiast, he is committed to show how open-innovation can lead to a better governance and economy.
date:   2014-02-18 14:34:25
categories: post
tags: 
  - open data
  - politics
  - predictive analytics
image: /assets/article_images/predicting-abstention-rate-using-open-data/cover.jpg
credits: Geoff Gallice from Gainesville, FL, USA - Army ant bivouac, CC BY 2.0
---

The purpose of this post is to guide beginners in data-science by illustrating how open data can be used to predict the [voting abstention rate at the 2008 mayoral elections in France](http://www.data.gouv.fr/fr/dataset/elections-municipales-2008-resultats-572150).

At ANTS, we believe open data is the key to solving many problems we experience on a daily basis. Being able to access high quality data about how our society works is a first step towards a more transparent, better democracy. 
We are hoping that hacking with data will be seen by the next generation as a way to express and prove powerful ideas. This is why [we are open sourcing](https://github.com/vallettea/politics-open-data) projects like this one. 

This post will cover three aspects of datascience:
 
 - how to clean the source datasets
 - how to merge heterogeneous datasets in one contextualized dataset
 - how data science can be used to predict the abstention rate with high accuracy

<!--more-->


## Background

Every six years in France, local municipalities elect a council that in turn elects a mayor. The [french open data portal](http://www.data.gouv.fr/) published the [2008 election results](http://www.data.gouv.fr/fr/dataset/elections-municipales-2008-resultats-572150), including the abstention rate.

In France, the municipality is the smallest administrative subdivision of the commune, of which there are about 36&nbsp;500. To get an idea of how densely packed this administrative division is, we can compare it to the US which has roughly the same number of municipalities, but has a territory 15 times larger.

Our goal is to determine whether the abstention rate can be predicted from a set of characteristics pertaining to each municipality. Hopefully, this will shed new light on the possible reasons behind high abstention rates, and contribute to a better understanding of the current political climate.


## Cleaning, transforming and visualizing the data

Cleaning data is by far the most time consuming task when starting a new data science project, so it is important to master the common data processing techniques to save time.

The election results dataset contains three files ([part1](http://www.data.gouv.fr/fr/dataset/elections-municipales-2008-resultats-572152), [part2](http://www.data.gouv.fr/fr/dataset/elections-municipales-2008-resultats-572154), and [part3](http://www.data.gouv.fr/fr/dataset/elections-municipales-2008-resultats-572150)) which are heterogenous. Some are based on the name of the electoral list, others the name of the mayor.

To clean this dataset, we will be using the largely popular Python language, having a huge community and many powerful data science libraries. One of the most used data-processing library is [pandas](http://pandas.pydata.org), which will be extensively used here.


#### Step 1. Converting from a proprietary XLS format to a CSV format

Open the files in a compatible editor and export them to CSV using UTF-8 as encoding and comma as separator. Each XLS file will produce as many CSV files as it has tables.

#### Step 2. Standardising the data fields' formats

The header in a CSV file (the first optional line) should preferably not contain any accent, special character or white space. The reason is that most tools and libraries (like Pandas) are designed for the English language and do not always support accessing columns by their name if odd characters are present.

#### Step 3. Merging everything into a single dataset

In Pandas, a `DataFrame` is the equivalent of an Excel table, meaning it has an index and columns. Creating a `DataFrame` from a CSV file (even compressed) is very simple:

```python
import pandas

filename = "Tour 1-Table 1.csv.gz"
clean_header(filename, sep=",")
data = pandas.read_csv(filename, sep=",", compression="gzip")
```

Columns can then be accessed by their names using `data[["column_name", "other_column"]]`. Pandas comes with all sorts of powerful data processing functions (see [10 minutes introduction to Pandas](http://pandas.pydata.org/pandas-docs/stable/10min.html)). For instance, in the files "part1" and "part2" from the previous section, each municipality has one line per person on the election list. As we only need to keep one row for each unique pair of `CODE_DEPARTEMENT` and `CODE_COMMUNE`, we have to drop duplicates:

```python
data.drop_duplicates(["CODE_DEPARTEMENT", "CODE_COMMUNE"])
```

We ultimately want to create one big dataframe out of all the CSV files we extracted. In Python, a useful tool is the `glob` module that returns all files whose name match a given pattern. The following one-liner yields all the CSV files we need from the two first Excel files:

```python
import glob
files = glob.glob("election_data/municipales_2008_part_[12]/*.csv")

['election_data/municipales_2008_part_1/49-59-Table 1.csv',
 'election_data/municipales_2008_part_1/60-69-Table 1.csv',
 'election_data/municipales_2008_part_1/70-79-Table 1.csv',
 'election_data/municipales_2008_part_1/80-88-Table 1.csv',
 'election_data/municipales_2008_part_1/89-95-Table 1.csv',
 'election_data/municipales_2008_part_2/16-26-Table 1.csv',
 'election_data/municipales_2008_part_2/35-48-Table 1.csv',
 'election_data/municipales_2008_part_2/37-34-Table 1.csv',
 'election_data/municipales_2008_part_2/OM 01-15-Table 1.csv']
```

We then create an empty `DataFrame` and loop to append each individual `DataFrame` to it:

```python
# An empty dataframe holding all others
all_data =  pandas.DataFrame(columns=[
    "CODE_DEPARTEMENT", 
    "CODE_COMMUNE", 
    "NOMBRE_INSCRITS", 
    "NOMBRE_ABSTENTION"])

for filename in files:
    # replace the headers so it has no accents
    clean_header(filename, sep=",")
    data = pandas.read_csv(filename, sep=",", compression="gzip")
    # we keep only columns we want and we remove duplicates
    data = data[["CODE_DEPARTEMENT", 
                "CODE_COMMUNE", 
                "NOMBRE_INSCRITS",
                "NOMBRE_ABSTENTION"]]
                .drop_duplicates(["CODE_DEPARTEMENT", "CODE_COMMUNE"])
    data["CODE_COMMUNE"] = data.apply(clean_commune_code, axis = 1)
    data["CODE_DPARTEMENT"] = data["CODE_DPARTEMENT"].apply(clean_dept_name)
    all_data = all_data.append(data)
```

We apply `clean_dept_name` and `clean_commune_name` to the original columns to remove the particular cases in the ids of the municipalities. These functions can be found [in the github repository](https://github.com/vallettea/politics-open-data).


#### Step 4. Assigning postcodes to municipalities

Linking municipalities to data from other sources requires the official municipality ID. The two most common ones in France are the postcode and the INSEE code.

Fortunately, there is a [clean file](http://public.opendatasoft.com/explore/dataset/correspondance-code-insee-code-postal/) containing both, as well as the *departement_id* and the *commune_id*. Using this file, we can transform our table `all_data`:

```
final = pandas.merge(
    mapping, 
    all_data, 
    how = "inner", 
    on=["CODE_DEPARTEMENT", "CODE_COMMUNE"])
```

The code can be accessed in the [election_cleaner.py](https://github.com/vallettea/politics-open-data/blob/master/election_cleaner.py) file on [Github](https://github.com/vallettea/politics-open-data).


#### Step 5. Plotting the result

Now that we have a clean structured file, it is straightforward to plot it on a map, such as the one below, where each municipality is coloured according to its abstention rate.

<figure>
    <img src="{{ site.baseurl }}/assets/article_images/predicting-abstention-rate-using-open-data/2014-02-18_communes_abstention.jpg" style="max-width:100%;">
    <figcaption>2008 local election abstention rate for municipalities in France.</figcaption>
</figure>

The code can be found in [plot_communes.py](https://github.com/vallettea/politics-open-data/blob/master/plot_communes.py).


### Open data publishing tips

The less time we spend cleaning data, the more time we can spend doing science. Here are a few tips for open data publishers who want to make our work (life?) easier:

#### Formats, encoding & locale

Excel files are unsuitable for a number of reasons:

- it's a proprietary format with little documentation
- it forces people to have/use an Excel compatible editor
- it doesn't work well with big datasets (it made my 2013 MacBook crash twice when opening the abovementioned files)
- encoding is often messed up, leading to deeply annoying problems when exporting to other formats such as CSV


JSON is a great file format. However, when possible, CSV should be preferred since:

- it can be streamed, and thus is better suited for large datasets
- JSON field names add a lot of redundancy to the dataset, making it 3-10x larger compared to CSV


A good dataset:

- is in a simple format such as CSV
- uses US or UK locale (no accents or weird punctuation)
- uses UTF-8 encoding
- uses ISO formatting for dates and times

#### Give us raw data!

The whole point of open data is to enable data scientists to do their own analysis. The trouble with aggregated datasets is that they often include hidden biases, and won't contain the same information (since it's, well, aggregated). 

Data scientists will always prefer raw, dirty datasets over clean aggregated ones. And they can actually contribute by publishing the data they cleaned so that other people can use it! As a matter of fact, the French government's open data portal has such a feature, where anyone can upload and modify datasets.


## Building the predictive model: the power of context

At ANTS, we use context to make predictions: we don't learn from the past directly but we rather recreate the context in which the phenomenon to predict occurred. Historical modelling tells you *what* happened, but contextual modelling tells you *why* it happened!

<figure>
    <img src="{{ site.baseurl }}/assets/article_images/predicting-abstention-rate-using-open-data/2014-02-22_contextual.png" style="max-width:100%;">
    <figcaption>Two different modelling paradigms: on the left, the model extrapolates from the past, following a given trend. On the right, the context is recreated and used to calibrate the models using the historical data.</figcaption>
</figure>


#### Contextualising French communes

Our data science objective is to try to predict the abstention rate from the characterisation of a municipality. By characterisation, we mean the features that describe the area (surface, unemployment rate, education, infrastructure, latitude, etc.). If successful, it would give insights on what drives abstention, and thus, what could be done to lower it. 

The main institution (and source) in France that describes these areas is the [INSEE](http://en.wikipedia.org/wiki/INSEE), whose role is to measure various socio-economic and demographic indicators. The datasets that can be dowloaded from [this page](http://www.insee.fr/fr/bases-de-donnees/default.asp?page=statistiques-locales/donnees-detaillees_tableau.htm) (in Excel format):

- [retail:](http://www.insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/equip-serv-commerce/equip-serv-commerce-com-12.zip) number of shops per category (malls, groceries, butchers, ...)
- [habitat:](http://www.insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/rp2010/chiffres-cles/base-cc-logement-2010/base-cc-logement-2010.zip) housing indicators (number of houses, secondary houses, ...)
- [entertainement:](http://www.insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/equip-sport-loisir-socio/equip-sport-loisir-socio-com-12.zip) entertainment & sports activities (sport clubs, cinemas, ...)
- [companies:](http://www.insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/base-cc-demo-entreprises/base-cc-demo-entreprises-12.zip) corporate activity indicators (number of young companies, multinationals, ...)
- [population:](http://www.insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/rp2010/chiffres-cles/base-cc-evol-struct-pop-2010/base-cc-evol-struct-pop-2010.zip) population demographic indicators
- [families:](http://www.insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/rp2010/chiffres-cles/base-cc-couples-familles-menages-2010/base-cc-couples-familles-menages-2010.zip) household indicators
- [salaries:](http://www.insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/base-cc-salaire-net-horaire-moyen/base-cc-salaire-net-horaire-moyen-10.zip) individual population earnings indicators
- [revenues:](http://www.insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/base-cc-rev-fisc-loc-menage/base-cc-rev-fisc-loc-menage-10.zip) household earnings indicators
- [medical employments:](http://www.insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/equip-serv-medical-para/equip-serv-medical-para-com.zip) availability of medical aid (doctors, nurses, ...)
- [unemployment:](http://www.insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/base-cc-chomage/base-cc-chomage-t3-2012.zip) employment statistics
- [tourism:](http://www.insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/base-cc-) tourism indicators (hostels, camping, ...)
- [services:](http://www.insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/equip-serv-particuliers/equip-serv-particuliers-com-12.zip) city services (police, banks, ...)

Using the steps described earlier, we cleaned and transformed the files, before adding their content to our previous election result dataset using the `INSEE_CODE` field:

```python
filename = "equip-serv-medical-para-com.csv.gz"
medical = pandas.read_csv(filename, sep=";", compression="gzip")
medical["INSEE_CODE"] = medical["CODGEO"].apply(str)
# we drop all the columns that we have already and 
# the column Unnamed: 32 due to a trailing comma in the file
medical = medical.drop([
            "CODGEO","LIBGEO",
            "REG","DEP","ARR",
            "CV","ZE2010","UU2010",
            "POP_2010", "Unnamed: 32"], axis=1)
# setting partial can decide wether we keep all the informative
# columns or if we choose arbitrarily some of them
if partial: medical = medical[[
            "MEDECIN_OMNIPRATICIEN", 
            "CHIRURGIEN_DENTISTE", 
            "SAGE-FEMME", 
            "INFIRMIER", "INSEE_CODE"]]
data = pandas.merge(data, medical, how="left", on="INSEE_CODE")
```

The code can be found in [contextualize.py](https://github.com/vallettea/politics-open-data/blob/master/contextualize.py).

Let's now add more files from [data.gouv](http://www.data.gouv.fr/):

- [education:](http://www.data.gouv.fr/fr/dataset/effectifs-d-etudiants-inscrits-dans-les-etablissements-et-les-formations-de-l-enseignement-superieur) higher education statistics (engeneering, medical, art, law schools etc)
- [taxes:](http://www.data.gouv.fr/fr/dataset/impot-de-solidarite-sur-la-fortune) wealth tax statistics
- [European help for agriculture:](http://www.data.gouv.fr/fr/dataset/aides-percues-par-les-personnes-morales-au-titre-de-la-politique-agricole-commune) European agricultural subsidies statistics
- [AOC:](http://www.data.gouv.fr/fr/dataset/aires-geographiques-des-aoc-aop) the number of *Appelation d'origine controllée* per municipality. This can be seen as a proxy to characterise municipalities with strong traditions (which often have good stuff to eat or drink!)

Unfortunately the data is not yet in a usable format. This is where Pandas shows its true power.

For agriculture, the dataset is composed of the name of the farmer who received a subsidy, his address *with postcode* and the amount. To get what we want, `groupby` postcodes and sum:

```python
file = "pac.csv.gz"
agriculture = pandas.read_csv(file, sep=";", compression="gzip")
# convert the amount from string to float to be able to sum
agriculture["PAC"] = agriculture["MONTANT_TOTAL"].apply(lambda x: float(x.replace(",", ".")))
# correctly format the postal code on 5 digits 1298 -> 01298
agriculture["POSTAL_CODE"] = 
    agriculture["CODE_POSTAL_DE_LA_COMMUNE_DE_RESIDENCE"]
    .apply(lambda x: "%05d" % int(x))
agriculture = agriculture[["POSTAL_CODE", "PAC"]]
# groupby postal code and sum
aggregated = agriculture.groupby("POSTAL_CODE").sum()
aggregated["POSTAL_CODE"] = aggregated.index
data = pandas.merge(data, aggregated, how="left", on="POSTAL_CODE")
```

For education, it is a bit more complicated because the file has entries for each pair of commune / school type. The solution is to make a `pivot` to get one row per municipality with columns for each school type:

```python
education = education.drop(["REGROUPEMENT","SECTEUR","SEXE"], axis=1)
p = education.pivot(index="GEO_ID", 
                    columns="CATEGORIE", 
                    values="EFFECTIF")
p["INSEE_CODE"] = map(str, p.index)
p = p.fillna(0)
```

What we end up with is a file containing about 36500 rows (one for each municipality) and thousands of columns describing it (its context). To make it human-readable, we added a `partial` parameter corresponding to the columns we wish to keep (picked by hand). Feel free to pick your own and see if you can improve the model! In a future blog post we will show how to automatically select the best features.

The full code can be found in [contextualize.py](https://github.com/vallettea/politics-open-data/blob/master/contextualize.py) in [the Github repo](https://github.com/vallettea/politics-open-data).

#### Visualizing the context

We previously plotted a static map of abstention rates in France. Displaying the context for each municipality is a hard visualization problem that requires powerful frontend libraries such as [d3.js](http://d3js.org/) and [leaflet](http://leafletjs.com/).

With the help of [Tahar Zanouda](https://twitter.com/T_Zano) (doing his master thesis with us at Snips (my former company)) and [David Bruant](https://twitter.com/DavidBruant), we plot this on a map to explore it visually.

<figure>
    <img src="{{ site.baseurl }}/assets/article_images/predicting-abstention-rate-using-open-data/2014-02-22_viz.jpeg" style="max-width:100%;">
    <figcaption>The d3+leaflet map. </figcaption>
</figure>

The dataset to plot is too large to be pushed to the Github repo, but it can be recreated by running the demo locally with `contextualize.py`.


## Predicting the abstention rate

Time to do some machine learning! We need an algorithm that can predict the abstention rate for a municipality (the output) from its context (the inputs). Here, we will be using the [gradient boosted regressor from the scikit-learn library](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingRegressor.html). 

<figure>
    <img src="{{ site.baseurl }}/assets/article_images/predicting-abstention-rate-using-open-data/2014-02-22_mlblackbox.png" style="max-width:100%;">
    <figcaption>Simplification of a machine learning algorithm. The output is a function of the weighted inputs. </figcaption>
</figure>

To predict the abstention rate for the coming 2014 elections, we want to train our model using the data from previous elections and apply it using the context in 2014.

Unfortunately, we don't have contextual data for 2014 (collecting this data is expensive and the INSEE cannot provide it for every year). The best we can do is take a sample from 2008, train the model on it, and test it against the remaning data. This is called "out-sample testing". In our case, we train the model using 90% of the dataset (so about 32k municipalities) and testing it on the remaining 10%.

Building this model with `scikit-learn` is easy:

```python
classifier = ensemble.GradientBoostingRegressor(**params)

size = 9*len(x_norm)/10
regressor.fit(x_norm[:size],y[:size])
expected = y[size:]
predicted = regressor.predict(x_norm[size:])
pearson = pearsonr(expected,predicted)[0]
print "Pearson coefficient: %s" % str(pearson)
print "MSE : %s" % np.sqrt(mean_squared_error(expected, predicted))
```

We created a regressor and trained it using the `fit` function. We then used it to make predictions for municipalities that were not included in the training using the `predict` function. Results are quite good:

- Pearson coefficient: **0.819012803238**
- MSE : **0.0891406116177**

These statistics measure the accuracy of our predictor: the Pearson coefficient measures the correlation between what was predicted and what actually happened. The MSE (Mean Squared Error) is a measure of the amplitude of the error.

But the really interesting part is to see what features are the most informative, i.e. the ones the model relies on the most to make its predictions.

Here are the twenty most important features and their relative importance:

<table>
<tr> <td> number of people in households<td> 100
<tr> <td> population <td> 65
<tr> <td> latitude <td> 56
<tr> <td> unemployment <td> 41
<tr> <td> number of companies created <td> 40
<tr> <td> longitude <td> 29
<tr> <td> second quartile of households revenues <td> 20
<tr> <td> number of households <td> 18
<tr> <td> number of single-parent families <td> 17
<tr> <td> population between 15-19 years old <td> 15
<tr> <td> altitude <td> 15
<tr> <td> households of a single person <td> 13
<tr> <td> area <td> 7
<tr> <td> number of AOC registered <td> 7
<tr> <td> population over 75 years old <td> 6
<tr> <td> population between 45-59 years old <td> 4
<tr> <td> population between 60-74 years old <td> 4
<tr> <td> amount of subsidies given to farmers by the EU <td> 4
<tr> <td> number of vacant housings <td> 2
<tr> <td> birthrate <td> 2
</table>

It seems that abstention rate is correlated to rational problems, giving insights into what are the key drivers. In a future post, we will show how varying these parameters can be used to test various scenarios, and thus be used as a decision support tool.

Interpreting these results is beyond the scope of this post, and is what politics is about. Data science is just a tool that can help make better decisions. Keep in mind though that:

<div class='note'>
<a href="http://en.wikipedia.org/wiki/Correlation_does_not_imply_causation"><strong>Correlation does not imply causation.</strong></a>
The number of sunglasses sold is correlated to the number of ice-cream sold but one cannot explain the other. In this case, a common variable (the presence of the sun) causes both of them, which explains why they occur together.
</div>


