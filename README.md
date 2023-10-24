<b><i>Author: </i></b>Tejaswini Sundar

<b><i>Date: </i></b>October 24, 2023

# Financial Modeling Projects
This repository contains projects that cover the basics of Financial Modeling, using the Financial Modeling course available on [GitHub](https://nickderobertis.github.io/fin-model-course/), taught by Nick DeRobertis, with lecture videos and tutorials available on [YouTube](https://www.youtube.com/playlist?list=PLACKX9tziAQJSk4YSGN0N2II985HTIuHD). This course helped me gain a better understanding and implementation of Financial Modeling by using tools like Excel and Python covering basics of Financial Modeling, such as TVM Models, Probabilistic Loan Pricing, etc.

This file will cover all projects listed in the course, along with Excel Worksheets and Jupyter Lab Notebooks for each project. The Project Titles are listed here: TVM Model, Probabilistic Loan Pricing, Monte Carlo Cost of Capital, and Full DCF Valuation. 

## 📋 TVM Model
In this project, I am tasked with building a model which will help determine how many machines the startup I work for must invest in and how much should the company spend on marketing its phones. There are several conditions to be aware of, all of which are listed below:
* The company will purchase a total of <i>n<sub>machines</sub></i> over a maximum period of years, denoted by the variable ```max_years```
  * The machine's operating lifetime is <i>n<sub>life</sub></i> years, after which the machine will be scrapped for $<i>p<sub>scrap</sub></i>, as the machine can no longer produce any output.
  * The machine, post its working lifetime, will not be replaced.
* Each purchased machine will produce <i>n<sub>output</sub></i> phones per year, with each phone selling for $<i>p<sub>phone</sub></i> with $<i>c<sub>phone</sub></i> being the variable cost of the phone's production.
* The equity investment of the company is also limited, which means that the company must either use its investment, denoted by <i>c<sub>machine</sub></i>, to buy machines or advertise its phones.
  * However, in the first year, the company must purchase a machine, with subsequent machine purchases being made one after the other. Only after all machine-related purchases are over will the advertising of the company's phones begin.
* The demand for phones increases as the advertising begins. The initial demand is <i>d<sub>1</sub></i>, with the demand's growth rate at <i>g<sub>d</sub>%</i>.
* The project occurs when the market interest rate is at <i>r</i>.

Before we begin the project, there are a few concepts we have to look over.

### Theoretical Concepts and Related Functions:
A concept in microeconomics, known as the law of supply and demand, plays an important role in the determination of price in the project. The law combines two fundamental economic principles by describing how prices in the market for a specific commodity fluctuate depending on its supply or demand. As the prices of a product increase, its demand decreases, and when the prices of the product decrease, it's demand increases. The intersection of supply and demand is called the **equilibrium price**, for which the product is sold. 

These curves can be plotted using a graph, with Quantity on the x-axis, Price on the y-axis, and S and D being the Supply and Demand curve respectively. If the supply curve shifts to the left, we will have a decrease in supply, meaning that the quantity would decrease, with the pricing of the product increasing, dropping the product's demand. The converse is true for the case of demand.

![Screenshot of the supply and demand curve](/Images/Supply_and_Demand_Curve_with_Equilibrium.webp?raw=true "Supply and Demand Graph")

Applying the above concepts to the given project, we know that the formula for Revenue is $`Revenue = Price * Quantity`$. In this instance, the price of the phone is already given, but the factor that changes with the dynamic demand for the phones is quantity. It is important to note that everything produced will not be sold, as it has to match the required demand for the year. Therefore, we take quantity as the ```min(Demand, Supply)```, because:
* If there is an excess supply of phones into the market, the price of the phones would increase, dropping its demand, thereby dropping the actual quantity purchased.
* Similarly, if there is a great requirement (demand) of phones in the market, and if suppliers are not able to produce as much, the supply will be the constraint for the above equation.

