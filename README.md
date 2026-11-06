    Project Objective
within this project i wanted to analyse how accurately and consistently a monte carlo simulation can price a european call option when u have set option parameters,
and also how the number of simulations that are ran can affect its consistancy. and to check its accuracy we compared it to a black scholes theoretical price 
which we got from the option parameters. this project had the aim of answering 3 questions: 
1. how does increasing the number of simulations affect the accuracy and consistency of the estimate we get from the monte carlo simulation?
2. how closely can the estimate from the monte carlo simulation reach the black scholes theoretical option price of £10.4506
3. how and why the simulation acts the way it does. i explored how the geometric brownian motion generates possible stock option price path and how
 randomness can affect the final estimate

        1.the mathematical background and methodology
       1.1 modelling stock prices using the Geometric Brownian Motion
       how stock prices are modelled as random processes and why , talk abt gbm and random shocks sigmasqrtdeltat times Z the gbm differential equation, whole thing where the gbm is in my code,  explain Z
   stock prices within this project have been modelled as random processes to simulate a simplified mathematical representation of stock market behaviour
   as stocks prices can fluctuate randomly. we do this via the use of the exact GBM solution:
\[
S_{t+\Delta t}=S_t e^{\left(r-\frac{1}{2}\sigma^2\right)\Delta t+\sigma\sqrt{\Delta t}Z}

\]  which is derived from the differential equation: 
\[
dS_t = rS_t\,dt+\sigma S_t\,dW_t
\]  
this model where s_t is the current asset price at time t, and $S_{t+\Delta t}$ is the asset price at the next time 
step. r is the risk free interest rate of 0.05 sigma is the volatiliy of 0.2 and delta t is the length of the 
time step which is 1/252 in this model (this will be covered in the next subsection) and z is a standard normal
random variable. z (mean of 0 SD of 1) is particularly importan as it is the source of randomness inside of this model, since z is
a standard normal random variable there is a higher probability of Z being a small value (around 0) and a 
lower probability of being a larger value(greater than 2 and less than negative 2) this means that the model 
will largely contain small positive or negative shocks dependant on the value of z also the simulation will be largely comprised of continous movement since the 
gbm assumes stock prices to evolve overtime meaning if the stock was to go from £10 to £10.01 it would go through every intermediate value which 
is not representative of a real market where we know that stock prices move in random ticks. furthermore the gbm model assumes eery shock to be independant of 
each other meaning a shock at the current time step doesnt affect the size of the shock at the next step which is a simplified version of the stock marker where 
behaviours suchas volatility clustering, small movements being followed by small and large shocks being followed by large shocks are observed. the role of 
volatility riskfree interest rate on the size 
the shock  i will cover in section 1.2

       1.2 Discretising time and simulating stock paths EXPLAIN WHY n=252 and why we do t/n  and how the code finds the stock price at the next time step using the current tiime step stock price and why we usedelta t and sqrrt delta t,
whereas z determines the direction of the shock volatility dictates the size of the shock. Stock volatility measures how much and how quickly a stock’s price fluctuates over time, reflecting the level of risk and uncertainty in the market therefore a large volatility greatly ampplifies the size of the shock whereas a 
small volatility would do the opposite. a large volatility within stock option generally causes the price of the premium to be higher since the stock
option has higher time value meaning the payoff can be very large since the stock has a larger possibility for a big payoff by increasing the range of random outcomes . similarly the time step can have a great affect on the intrinsic value of the stock by increasing the time value, since the 
larger the time to maturity of the stock the randomly simulated paths have a chance to go very high giving the asset a higher potential payoff, and as the expiry date gets closer the time value continues to decrease until it reaches 0 meaning it can have 
a very large payoff. it is important to notice that stocks with very high intrinsic values generally have a larger premium meaning a larger intrinsic value may 
not necesarily mean the profit is much larger unless the payoff is very large and in the money. and a stock with a lower time to maturity or smaller time step 
will have a lower time value since it has a smaller chance to be able 
to have a very large surge in value.

in this model we use t=1 and n=252 since were taking the time to be a year long and a year roughly has 252 trading days.
and by doing t/n we get delta t=0.00396825 which is the length of each individual step, by deviding the total time to expiry into 252 steps were able to form an 
entire 
stock price path to the expiry date of the stock option, the exact gbm solution uses this to simulate the stock price at the next time step although u could 
technicall just calculate the value at expiry splitting each path into 252 steps allows you to generate stock prices throughout the year, visualise how stock 
prices can change over the course of a trading year and examine the effect of a large shock. and sincce the end result is already set increasing the number of 
trading steps just oncreases how detailed the path seems instead of increasing the accuracy of the estimated end price.
the model can be split into to parts part A which is the predictable movement which predicts the expected movement of the stock at the next time step and part b 
which is the random component which introduces a random shock via the use of Z when combining these 2 together within the gbm model to produce a simulated value of the 
stock price at the next time step and will continue doing this until it hasnt reached the final estimated price. since part a \[
S_{t+\Delta t}=S_t e^{(r-\frac12\sigma^2)\Delta t+\sigma\sqrt{\Delta t}Z}
\] 

consists of the predictable movement it contains dealta T (sigma and r will be covered in the next subsection) meaning if the time step is quadrupled the expected 
affect is also quadrupled. within our model since r=0.05 and sigma=0.2 the predictable component is 0.03(deltat) which is strictlly positive meaning theres a 
positive drift so it pushes the stock value up however the reason the stock price isnt always necesarily increasing is due to the random component which can 
cause the stock price to go up or down this is the reason we can have a lower value than we started with because the random component can outweigh the affect of 
the positive predictable drift. the random movements can partially cancel each other and as u add more random movements the uncertainty (the difficulty to predict the stock price due to random movement)
keeps increasing , but since the random movements partly cancel each other out uncertainty does not increase in a straight line. so if we increase the time step 4 
the predictable affect would also increase 4 fold since it uses delta t. however uncertainty and the  random affect would increase 2 fold since they scale with standard deviation which is the sqrt of delta t. so the predictable affect increases proportionally to the time step, whereas the random affect and the uncertainty increase to the square root of the time step so uncertainty keeps increasing at a decreasing rate. its important to note that this is what
is expected of the stock and the stock may not follow this pattern, uncertainty just means there is a wider variety of possible paths for them to take



       1.3 european call options and risk neutral pricing what the models tryna price and why future payoffs are discountwd, talk about the option parameters, talk abt volatility, risk neauteral pricing, and why we use r
the goal of this model is to price a european call option, a european call option is a financial contract which gives the buyer the right but not the obligation 
to purchase an underlying asset at a predetermined price. in our model we have a european call option with a strike price of 100 and time to expiry being 1 year
meaning 1 year from now we can buy that stock for 100 pounds meaning that if the stock price at expiry is 120 pounds the buyer purchases the option at 100 
and gets a 20 pound payoff and if the buyer does not exercisee their right bc the stock falls short they would get no payoff 

\[
C_T=\max(S_T-K,0)
\]
this is the formula for calculating payoff at expiry, C_T is payoff at expiry, S_T is the stock price at expiry and K is the strike pricee further important note is that the paayoff is not the premium since a stock could have a large payoff buy if the premium is large enough the buyer may not gain any profit. the premium is the price of the financcial contract. and were calculating how to price an option premium today we do this via 
simulating the option payoff at expiry of a stock we have simulated 100000 times getting the option values of these simulated paths and then each payoff to 
how much its value would be today then we take the average of all the discounted payoffs to find a suitable option premium price today, since we assumed r to be 0.05 if we had a stock worth 95.12 pounds today the value of that stock 1 year from now would be 100 pounds meaning that 95.12 pounds compounded continously from today would equate to 100 pounds from one year from 
now we discount future payoffs using our risk free rate of 5% to find out what their value would be today so its equivalence can be found today 
giving us a consistent no arbitrage price so going with our example an option with a payoff of £100 would have a worth of 95.12 pounds in equivalence with today
since this is risk neuteral pricing we discount the future payoff using r and after averaging the discounted payoffs we can estimate the options
price today. this whole method is known as risk neuteral option pricing risk-neutral pricing lets us price an option by using the risk-free rate as the expected return in our mathematical model, rather than trying to predict the stock's actual return.

another factor which can affect the options premium is volatility, a large volatility means a stock can have a wider range of possible simulated paths to take 
there fore meaning that the stock option increases potential for both very large upwards or downwards movements. however since this is a european call option you 
cannot have a payoff below 0 pounds since
if after the year the stock price falls below the strike price the buyer can choose not to exercise his right since the contract gives u the right but not the obligation to buy the stock at the predetermined price meaning then the only loss you would be making is that of the premium. 

these are the option parameters

Parameter	Value	Meaning
\(S_0\)	£100	Initial stock price
\(K\)	£100	Strike price
\(T\)	1 year	Time until expiry
\(r\)	5%	Risk-free rate
\(\sigma\)	20%	Annual volatility
\(N\)	252	Time steps
\(M\)	100,000	Simulations

it is important to not that the stock is initially at the money since the initial stock price and strike price are the same, and these option parameters give a 
monte carlo simulation premium of £10.4273 and a black scholes theoretical premium of £10.4506 where the absolute difference is £0.0233. furthermore it is 
important that we use the absolute difference by using the mod function. doing this prevents 2 values one positive and one negative across 2 different 
simulations to candcel each other out, e.g if you had one estimatre from a run as £11 and another as £9.9 the signed error is -0.0006 which makes it look like 
the results are perfect and really close to the benchmark which we know is not the case which we can see is wrong as both vakues are very far from the benchmark 
where as in reality the absolute error would be £0.55 since both estimnates are each 0.55 pounds away from the benchmark.

       1.4 monte carlo estimation and sampling error Explain how the individual discounted payoffs are combined to produce one estimated option price.
        
        Include:
        - The sample mean of the discounted payoffs.
        - Why more simulations generally improve precision.
        - Standard error and its \(1/\sqrt{M}\) relationship.
        - The difference between standard error and absolute error.
        This provides the mathematical foundation for Graphs 2 and 3.
to find the option price today we dicount the future payoffs and average them across all the run simulations to get the option price today to discount them we
use the risk free interest rate of 0.05 and times is by the time to expiry however since the time to expiry is one year we would just get \[
e^{-rT}=e^{-0.05}=0.951229, and to
average all the ran simulations we just find the mean of them shown in the code by np.mean(payoffs) (which represents this formula \[
\bar X=\frac{1}{M}\sum_{i=1}^{M}X_i
\])
and by multiplying this by the discount factor \[
e^{-rT}
\]  where r = 0.05 and t =1, we get the discounted mean option price of today. this has been done acriss all the different simulations ran from 10-100000 this gives us a 
reliable option price for today. we got the monte carlo estimate of £10.4245 via this method, the £10.4245 (not to be confused with the value of £10.4273 which we got from the set seed in the code) represents a mean discounted option price given to us 
from a simulation of 3 different programs running 100000 simulation paths, the more simulation paths that are run generally the higher the precision of your
results, this is due to the law of large numbers where in the context of this project the more simulations u run the more the results woule converge to one value
the sample mean converges towards the theoretical expected discounted payoff under the risk-neutral model. Under the same assumptions, this is the Black–Scholes price of £10.4506.  In probability theory, the law of large numbers is a mathematical law 
which states that the average of the results obtained from a large number of independent random samples converges to the true value, if it exists. More formally, 
the law of large numbers states that given a sample of independent and identically distributed values, the sample mean converges to the true mean. although this 
doesnt guarantee that every increase causes the runs to get more accurate due to fluctuations caused by randomness . random sampling may cause absolute error to increase 
between simulation runs. this is also why in simulation runs with less simulations the 
absolute error can be so high since 1 outlier can greatly affect the absolute error this will also be seen in graph 3 later within this subsection.

standard error measures the sampling uncertainty of the monte carlo estimate wherewas the absolute arror measures how far my montecarlo estimate is from the black scholes benchmark both ignoring whether the affect is poisitive or negative. standard error decreases as the number of simulations increases (\(1/\sqrt M\))
because averaging a larger number of independent simulated payoffs reduces the sampling variability of the estimated mean.

furthermore graph 2 is a visual representation of thew monte carlo simulation we see, as the number of simulations increases the presicion increases as sampling variability decreases The estimate generally converges towards the benchmark, improving its accuracy in the long run. and consistency of the 
theoretical price in comparison to the benchmark increases however there are periods where the random shock causes the results to move further away from the 
black scholes benchmark however the overall trend of the graph suggests convergence to the black scholes theoretical price. as mentioned previously its due to 
the law of large numbers which states, with fewer simulation counts individual random movements have a greater influence pn the average payoff therefore as the 
number of simulation increases the influence of those singular random movements decreases and the average estimate becomes a more clear representation of
the true mean. and convergence is not always linear or smooth as i mentioned before as an estimate with 100 simulations could be closer to the benchmark than an
estimate with 1000 simulations but the one with 1000 simulations would still have lower standard error it would just be affected by random variation. convergence 
refers to approaching a limit closer as the number of simulations increases which is what we generally see in the graph and thats due to the law of large 
numbers, which explains that increasing the number of simulations increases the stability as there is a lower chance for individual results to greatly influence 
the payoff.

graph 3 on the other hand shows absolute difference which is the difference between the black scholes benchmark and the average monte carlo payoff that we get this graph gives a visual representation of how absolute error decreases as more simulations are run. the formula for absolutre error is:

\[
\text{Absolute Error}=|\text{MC estimate}-\text{BS price}|
\] 

which is the modulus of the monte carlo estimate minus the black scholes benchmark and the reason for using absolute error and by extention modulus, is  because
we dont want any positives and negatives cancelling each other out and Absolute error generally tends to decrease as the number of simulations increases because the sample mean converges towards the theoretical expected value. However, individual absolute errors can fluctuate due to random sampling. absolute error measures the difference between the mc estimate and the bc price and then averages this across all simulation runs within the same simulation, graph 2 shows convergence which graph 3 also partiallly shows but graph 3 mainly emphasises the acuracy of individual simulation runs incomparison with the bc benchmark

        black scholes formula 
the black scholes model formula is: 

\[
C=S_0N(d_1)-Ke^{-rT}N(d_2)
\]

where by using the option parameters (found below) the model calculates a fair theoretical option price today producing the benchmark we used throughout this project of 
£10.4506. the formula which is given can be split into 2 parts the first term and the second term

Component	Meaning
C	The theoretical option price today
S_0	Current stock price (£100)
K	Strike price (£100)
r	Risk-free interest rate (0.05)
T	Time until expiry (1 year)
e^{-rT}	Discount factor
N(d_1)	Cumulative standard normal probability evaluated at \(d_1\)
N(d_2)	Cumulative standard normal probability evaluated at \(d_2\)
                                                        

        model aassumptions and limitations 

limitations of this model are that the geometric brownian motion assumes the random shocks to be normally distributed therefore a real market is not accurately
replicated since in a normal distribution most values are around 0 however in a real market large price gaps can occur more frequently than a normal distribution
can predict therefore the shocks may not be representative of something that would occur in a real market. and another limitation of the GBM being the every new 
shock is independant of previous shocks, whereas real markets show behaviours such as volatility clustering where an large movement one day can be followed with 
another large movement and small movements are often followed by other small movements thus the GBM is not able to show large jumps. within real markets 
certain anouncements from a company or events can show large temporary jumps in option price but since the GBM generates continous stock paths meaning the stock 
path 
changes without sudden jumps, this is an important limitation of this model since 
its a model that simplifies what an actual market would run like. it is important to note that in this project i did not take into account the 
possibility of the volatility and the risk free interest rate changing as time progresses, whereas in reality the risk free interest rate
could change in a real market due to decisions made by the bank of england and volatility can also change dependant on economic uncertainty
and company news suchas the company gaining bad reputation for something they have done.

    Why did i use the black scholes as the benchmark?

it is to check whether the simulation works, the black scholes model factors in all of the stock option parameters and it gives an analytical theoretical price
using the black scholes call pricing formula it is not a guess but a theoretical price which is derived using a mathematical formula under the option parameters. 
this price is not guaranteed if ran in reality, instead it is a theoretical estimate made using the model 
assumptions. and it is used to compare against the monte carlo simulation and check for the models accuracy, if the 2 show a large absolute error even after 
greatly increasing the simulation count it may be indicative of a mathematical/coding/calculation error.
 
    Monte carlo convergence.

if the generated outcomes converge to a completely different value and the absolute difference does not show an overall trend of decrease in terms of absolute 
differences the values could converge to another theoretical price showing consistency however if the estimated value is consistently not near the benchmark of 
10.4506 there may be an indication that a maths/code error is present, however since their model assumptions are the same the generated outcomes should converge 
to the theoretical option price.
it is also important to notice that randomness plays a part within the monte carlo simulation, you can run the same program with the same option parameters and same number or simulations ran
but get a different value and that is simply because the monte carlo simulation generates the estimate using randomly generated outcomes increasing the number of
simulations generally reduces sampling error however running more simulations cause individual random outcomes to have less of an affect on the estimate
therefore a run with a thousand simulations may get lucky and get less absolute error than a run with 10000 simulations simply because were working with random
outcomes The mathematical idea is that the sampling variability generally decreases as the number of simulations increases. 
Under the usual assumptions, the standard error decreases approximately in proportion to
1 over sqrt m
where M is the number of simulations.
This means that multiplying the simulation count by 100 reduces the standard error by a factor of approximately 10, assuming the underlying payoff variance 
remains the same. It does not mean the absolute error must decrease by a factor of 10, or even decrease at all in every individual run. and furthermore to
further investigate the consistency we have ran each simulation at each count multiple times, i.e. i have ran the simulations for 1000, 10K..... multiple times check it now, are we missing any vital information is there anything else we can add, or anything we can change to this to link it back to the original questions or is there no need to do that

     Conclusion
the main result i came to was that, the monte carlo simulation can accurately predict the option price when compared to its black scholes benchmark of £10.4506 under the models assumptions which where

S0 = 100 # Current stock price (£)

K = 100 # Strike price (£)

T = 1.0 # Time to expiry (years)

r = 0.05 # Risk-free interest rate (5%)

sigma = 0.20 # Annual volatility (20%)

N = 252 # Trading-time steps per year

M = 100_000 # Number of simulated paths

generally the model found that increasing the number of simulations ran decreased the sampling error and variability making the results more stable and
consistent since there was a lower chance for individual random results to greatly affect the overall trend. however increasing the number of simulations 
does not always decrease the absolute error since the averages can still show difference from the benchmark due to randomness. ps the model simulates stock 
prices to estimate the option price, it does not predict the price of the stock itself
