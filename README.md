<b><i>Author: </i></b>Tejaswini Sundar

<b><i>Date: </i></b>October 24, 2023

# Financial Modeling Projects
This repository contains projects that cover the basics of Financial Modeling, using the Financial Modeling course available on [GitHub](https://nickderobertis.github.io/fin-model-course/), taught by Nick DeRobertis, with lecture videos and tutorials available on [YouTube](https://www.youtube.com/playlist?list=PLACKX9tziAQJSk4YSGN0N2II985HTIuHD). This course helped me gain a better understanding and implementation of Financial Modeling by using tools like Excel and Python covering basics of Financial Modeling, such as TVM Models, Probabilistic Loan Pricing, etc.

This file will cover all projects listed in the course, along with Excel Worksheets and Jupyter Lab Notebooks for each project. The Project Titles are listed here: [TVM Model](#-tvm-model), Probabilistic Loan Pricing, Monte Carlo Cost of Capital, and Full DCF Valuation. 

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

![Screenshot of the supply and demand curve](/Project-1-TVM-Model/Images/Supply_and_Demand_Curve_with_Equilibrium.webp?raw=true "Supply and Demand Graph")

Applying the above concepts to the given project, we know that the formula for Revenue is $`Revenue = Price * Quantity`$. In this instance, the price of the phone is already given, but the factor that changes with the dynamic demand for the phones is quantity. It is important to note that everything produced will not be sold, as it has to match the required demand for the year. Therefore, we take quantity as the ```min(Demand, Supply)```, because:
* If there is an excess supply of phones into the market, the price of the phones would increase, dropping its demand, thereby dropping the actual quantity purchased.
* Similarly, if there is a great requirement (demand) of phones in the market, and if suppliers are not able to produce as much, the supply will be the constraint for the above equation.

The functions we will be using for the calculation of the Time Value of Money problem is Excel's built-in function called ```NPV(rate, values)``` and ```numpy_financial```'s ```npv(interest_rate, cash_flow)``` function. NPV, or Net Present Value, is the difference between the present values of the cash flow and the initial investment of a project, and this is calculated to predict the profitability of a project, and is a measure widely used in capital budgeting and investment planning.

### Solutions Overview - Excel and Python:
* To begin, I first created a column/array called Demand, where I calculated the demand per year based on the above conditions for machine purchases.
  * As the company had to purchase it's first machine in the first year, and then complete all machine purchases before beginning to advertise, I used a condition to check whether the given year had exceeded the planned number of machines the company wished to buy.
  * If it did, the demand would increase by 2% using the formula ```val = d_1 * (1 + g_d) ** (i - n_machines)```. In this case,
    * ```val``` is the value being inputted into the demand array/column
    * ```d_1``` is the initial demand for the phones produced by the company
    * ```g_d``` is the growth in demand after the company begins advertising
    * ```i``` is the loop counter than runs for the forecasted number of years for which the company is calculating its NPV.
    * ```n_machines``` is the number of machines the company wishes to purchase.
* I then created a column to calculate the Revenue of phones manufactured by the company. To do this, I multiplied the price of the phone with the quantity, which, as mentioned above, changed with the changing demands.
  * To track the changes to the quantity, I evaluated the inputs through varying conditions, including checking whether the current year indicated changes to quantity supplied. For instance, if the current year was less than the lifespan of a single machine:
   
   ```
   elif i < n_life and j < n_life:
    supply = n_phones * j
    quantity = min(demand[j], supply)
   ```
  
  * Or if the current year stood in the period of when the company's machines would begin deprecating:
   
   ```
   elif i%n_machines != 0 and i%n_life < n_machines:
    supply = n_phones * (n_machines - (i%n_life))
    quantity = min(demand[j], supply)
   ```
  * From the above, we find that:
    * ```n_life``` is the expected lifespan of the machine,
    * ```i, j``` are the loop counters that range over the maximum mentioned number of years and the length of the demand array
    * ```supply``` is the calculated supplied value by the company, and
    * ```quantity``` follows the above condition of taking the minimum of demand and supply to calculate the expected revenue for that year.
  * Similar calculations were made for finding the variable cost of the phones produced
* I then used conditions to check when a machine would operate beyond its lifespan, thereby becoming scrap. To do this, I checked whether the year was greater than the expected lifespan of a single machine, and its difference with the single machine was less than or equal to the number of machines. A snippet is shown here:
  ```
  if i > n_life and i - n_life <= n_machines:
   scrap_array.insert(i, price_scrap)
  ```
  * ```scrap_array``` is the array created to check and calculate whether the machine for that year has passed its working lifetime.
  * ```price_scrap``` is the variable used to denote the $<i>p<sub>scrap</sub></i>
* To calculate the cash flow for the year, I used the Revenue, variable cost of producing the phone, the equity investment for the year, and the calculated scrap array using the following formula:
  $`Cash flow = Revenue - Variable Cost - Investment + P`$
* Lastly, I calculated the NPV on Excel and Python using the built-in functions to obtain the NPV of the project. Since the NPV is positive, we can say that the project is profitable, and can be picked up by the company for future growth.

That concludes the first project - my version and understanding of the TVM Model, both in Excel and Python!
