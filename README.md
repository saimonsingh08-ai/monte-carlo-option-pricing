                                                     Project Objective
within this project i wanted to analyse how accurately and consistently a monte carlo simulation can price a european call option when u have set option parameters,
and also how the number of simulations that are ran can affect its consistancy. and to check its accuracy we compared it to a black scholes theoretical price 
which we got from the option parameters. this project had the aim of answering 3 questions: 
1. how does increasing the number of simulations affect the accuracy of the theoretical price we get from the monte carlo simulation?
2. how accurately can a monte carlo simulation estimate an option price? compared against a black scholes theoretical price of £10.4506
3. how and why the simulation acts the way it does. i explored how the geometric brownian motion generates possible stock option price path and how
 randomness can affect the final estimate.

                                                        Conclusion
the main result i came to was that, the monte carlo simulation can accurately predict the price of a stock option when compared to its black scholes benchmark
generally the model found that increasing the number of simulations ran decreased the sampling error and variability making the results more stable and
consistent. . 

    model aassumptions and limitations 

limitations of this model are that the geometric brownian motion assumes the system to be normally distributed therefore a real market is not accurately
replicated since in a normal distribution most values are around 0 however in a real market large price gaps can occur more frequently than a normal distribution
can predict. and another limitation of the GBM being the every new shock is independant of previous shocks, meaning if the market is on the rise today it does 
not guarantee a rise tomorrow whereas real markets show behaviours such as volatility clustering where an increase one day can be followed with an upwards trend.
furthermore the GBM is not able to show large jumps. within real markets certain anouncements from a company or events can show large temporary jumps but since 
the GBM generates continous stock paths meaning the stock path changes without sudden jumps, this is an important limitation of this model since 
its a model that simplifies what an actual market would run like. however it is important to note that in this project i did not take into account the possibility of the volatility and the risk free interest 
rate changing as time progresses
Why did i use the black scholes as the benchmark?
it is to check whether the simulation works, the black scholes model factors in all of the stock option parameters and it gives a reasonable estimate
using the black scholes call pricing formula. this price is not guaranteed if ran in reality, instead it is a theoretical estimate made using the model 
assumptions. and it is used to compare against the monte carlo simulation and check for the models accuracy. 
 
    Monte carlo convergence.

if the generated outcomes converge to a completely different value and the absolute difference does not show an overall trend of decrease in terms of absolute 
differences. the values could converge to another theoretical price showing consistency however if the value is not around 10.4506 we would know that the model
lacks accuracy. both the black scholes benchmark and the monte carlo simulation ran under the same assumptions. it is also important to notice
that randomness plays a part within the monte carlo simulation, you can run the same program with the same option parameters and same number or simulations ran
but get a different value and that is simply because the monte carlo simulation generates the estimate using randomly generated outcomes. increasing the number of
simulations reduces sampling error however there is a possibility that since the simulations outcomes are random the prive converges onto another value thats 
therefore a run with a thousand simulations may get lucky and get less absolute error than a run with 10000 simulations simply because were working with random
outcomes 
The mathematical idea is that the sampling variability generally decreases as the number of simulations increases. Under the usual assumptions, 
the standard error decreases approximately in proportion to
1 over sqrt m
where M is the number of simulations.
This means that multiplying the simulation count by 100 reduces the standard error by a factor of approximately 10, assuming the underlying payoff variance 
remains the same. It does not mean the absolute error must decrease by a factor of 10, or even decrease at all in every individual run. and furthermore to
reduce the chance of random error we have ran each simulation at each count multiple times, i.e. i have ran the simulations for 1000, 10K..... multiple times check it now
