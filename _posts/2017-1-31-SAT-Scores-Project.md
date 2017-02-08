This is the first project for my Data Science course at General Assembly. For this project we were given a data set containing the participation rate, mean verbal score, and mean math score for all 50 states as well as the District of Columbia. The work was done in Jupyter Notebook using Python.

The first thing I did was examine the data to determine if it was complete and if there were any obvious issues with the observations. The data did look complete, however, potential issues with just looking at the data are that it isn't made explicitly clear that the participation rate is listed as a percentage. Also, someone not familiar with the grading scale may not be able to tell whether the average scores are good or bad.

The next step was creating a dictionary for the data set. This was done using the following code:

```python
import csv
reader=csv.DictReader(open("/Users/ct/DSI-NYC-4/projects/project-1-sat-scores/assets/sat_scores.csv"))
datadict=[]
for x in reader:
    datadict.append(x)
print datadict
```

The data was also loaded into a list of lists and printed:

```python
import csv
with open('/Users/ct/DSI-NYC-4/projects/project-1-sat-scores/assets/sat_scores.csv', 'r') as f:
    reader = csv.reader(f)
    datalist = list(reader)
```

```python
print datalist
```

I extracted a list of the labels from the data:

```python
datalist.pop(0) #removes the list of header names and displays it
```

I also created a list of state names extracted from the data with the help of a for loop:

```python
statenames=[]
for x in datalist:
    statenames.append(x[0])
print statenames
```

Next, I took a look at the data types for each column and found out that they were all strings. This was a problem since I needed all of the values to be integers, so I reassigned the datatypes for the participation rate, math score, and verbal score columns using the help of a list comprehension.

```python
print type(datalist[0][0]) #Prints the data type for the state column
print type(datalist[0][1]) #Prints the data type for the participation rate column
print type(datalist[0][2]) #Prints the data type for the verbal score column
print type(datalist[0][3]) #Prints the data type for the math score column
```

```python
datalist2 = [[x[0], int(x[1]), int(x[2]), int(x[3])] for x in datalist]
print datalist2
#The data types for the values of the the participation rates, and math and verbal scores were reassigned to integers
```

I was then tasked with creating a dictionary for each column mapping the state to its respective value for that column, followed by creating a dictionary with the values for each of the columns.

```python
prate={x[0]:x[1] for x in datalist2} #Creates a dictionary for the participation rates of each state
verbal={x[0]:x[2] for x in datalist2} #Creates a dictionary for the verbal scores for each state
math={x[0]:x[3] for x in datalist2} #Creates a dictionary for the math scores for each state
print prate
print verbal
print math
```

```python
prate_col={'rate':[x[1] for x in datalist2]} #Creates a dictionary for the participation rate values
verbal_col={'verbal':[x[2] for x in datalist2]} #Creates a dictionary for the verbal score values
math_col={'math':[x[3] for x in datalist2]} #Creates a dictionary for the math score values
print prate_col
print verbal_col
print math_col
```

The next section of the project required us to describe the data. I started with printing minimum and maximum values for each column and then finding the stadard deviations for each using list comprehensions.

```python
print "The minimum participation rate is", min(prate_col['rate']), "percent"
print "The maximum participation rate is", max(prate_col['rate']), "percent"
print "The minimum verbal score is", min(verbal_col['verbal'])
print "The maximum verbal score is", max(verbal_col['verbal'])
print "The minimum math score is", min(math_col['math'])
print "The maximum math score is", max(math_col['math'])
```

```python
from math import sqrt
def std(lst):
    num=len(lst)                    # number of values
    avg=sum(lst)/num                # average of all values
    diff=[x - avg for x in lst]     # creates a list containing the difference from the mean for each value
    sqdiff=[y ** 2 for y in diff]   # creates a list containing the squared difference for each value
    ssd=sum(sqdiff)                 # ssd is the sum of the squared differences
    variance=ssd/num                # variance is the ssd divided by the number of values
    sd=sqrt(variance)               # standard deviation is the square root of the variance
    print sd
    
print "The standard deviation of the participation rates is", std(prate_col['rate'])
print "The standard deviation of the verbal scores is", std(verbal_col['verbal'])
print "The standard deviation of the math scores is", std(math_col['math'])
```

For the final section of the project, I made visualizations of the data using MatPlotLib and PyPlot, as well as Tableau. I started by making histograms of the participation rates, math scores, and verbal scores.

```python
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline

x=prate_col['rate']

plt.hist(x, bins=[0,10,20,30,40,50,60,70,80,90,100], facecolor='blue') # creates the histogram
plt.title('Histogram of SAT Participation Rates')                      # adds the chart title
plt.ylabel('Frequency')                                                # adds y axis label
plt.xlabel('SAT Participation Rate')                                   # adds x axis label
plt.axis([0, 100, 0, 15])                                              # sets the scales for the x and y axes
plt.show()
```

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/histrate.png?raw=true">

```python
y=math_col['math']

plt.hist(y, bins=[0,25,50,75,100,125,150,175,200,225,250,275,300,325,350,375,400,425,450,475,500,525,550,575,600,625,650,675,700,725,750,775,800], facecolor='green')
plt.title('Histogram of Math scores')
plt.ylabel('Frequency')
plt.xlabel('Math Scores')
plt.axis([0, 800, 0, 18])
plt.show()
```

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/histmath.png?raw=true">

```python
z=verbal_col['verbal']

plt.hist(z, bins=[0,25,50,75,100,125,150,175,200,225,250,275,300,325,350,375,400,425,450,475,500,525,550,575,600,625,650,675,700,725,750,775,800], facecolor='red')
plt.title('Histogram of Verbal scores')
plt.ylabel('Frequency')
plt.xlabel('Verbal Scores')
plt.axis([0, 800, 0, 16])
plt.show()
```

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/histverbal.png?raw=true">

The typical assumption for data distribution is that the data of a sample follows a normal distribution. A graph of the data should show a bell curve. When it comes to this dataset, that generally holds true for the math and verbal scores distribution. However the participation rate distribution does not.

After the histograms, I made three scatterplots for the data set. The first was a plot of the math and verbal scores, and the last two were of the math and verbal scores compared to the participation rates.

```python
x1=math_col['math']
y1=verbal_col['verbal']

plt.scatter(x1,y1)                                 # creates scatterplot
plt.title('Scatterplot of Math and Verbal Scores') # adds chart title
plt.ylabel('Verbal Score')                         # sets y axis label
plt.xlabel('Math Score')                           # sets x axis label
plt.show()
```

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/scatscores.png?raw=true">

```python
x2=prate_col['rate']
y2=math_col['math']
plt.scatter(x2,y2)
plt.title('Scatterplot of Participation Rate and Math Scores')
plt.ylabel('Math Score')
plt.xlabel('Participation Rate')
plt.show()
```

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/scatmath.png?raw=true">

```python
x2=prate_col['rate']
y2=verbal_col['verbal']
plt.scatter(x2,y2)
plt.title('Scatterplot of Participation Rate and Verbal Scores')
plt.ylabel('Verbal Score')
plt.xlabel('Participation Rate')
plt.show()
```

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/scatverbal.png?raw=true">

Examining the scatterplots, there were some interesting relationships to note. Generally, it looks like the higher that students scored in math, the better they scored in verbal as well. Students generally scored similarly in both section. Also, states with lower participation rates had higher average scores.

The next type of visualization I used for the data was the box plot. I created one for each variable.

```python
x3=prate_col['rate']

plt.boxplot(x3)                              # creates box plot
plt.title('Box Plot for Participation Rate') # adds chart title
plt.ylabel('Participation Rate')             # sets y axis label
plt.show()
```

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/boxrate.png?raw=true">

```python
y3=math_col['math']

plt.boxplot(y3)
plt.title('Box Plot for Math Scores')
plt.ylabel('Math Score')
plt.show()
```

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/boxmath.png?raw=true">

```python
z3=verbal_col['verbal']

plt.boxplot(z3)
plt.title('Box Plot for Verbal Scores')
plt.ylabel('Verbal Score')
plt.show()
```

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/boxverbal.png?raw=true">

Finally I used Tableau to create heat maps for each variable. The heat maps show a map of the United states and higher values make each state appear darker for each variable.

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/rateheatmap.png?raw=true">

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/mathheatmap.png?raw=true">

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/verbalheatmap.png?raw=true">

It appears as though the SAT participation rate is higher in the coastal states but the average scores for both the verbal and math sections are higher in the inland states. This seems to indicate that although more students take the SAT on the coasts of the country, the few that do participate in the SAT from the inland states are, for whatever reason, better prepared to do well in the exams.