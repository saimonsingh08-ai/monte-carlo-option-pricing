     Project Objective 
 

Within this project, I wanted to analyze how accurately and consistently a monte Carlo simulation can price a European call option when u have set option 
parameters. I also wanted to see how the number of simulations that are run can affect the consistency of the answers. and to check its accuracy, we compared it 
to a black Scholes theoretical 
price which we got from entering the option parameters into the Black Scholes formula . This project had the aim of answering 3 questions: 

How does increasing the number of simulations affect the consistency of the Monte Carlo estimate? 

How accurately can the estimate from the monte Carlo simulation reach the black Scholes theoretical option price of £10.4506?

How does the geometric Brownian motion affect the simulation? How does randomness affect the simulation and why? 

    1.The mathematical background and methodology 
         1.1 Modelling stock prices using the Geometric Brownian Motion 
 

Stock prices within this project have been moddeled as random processes, to simulate a simplified mathematical representation of how the stock market behaves as 
stock 
prices can fluctuate randomly. We do this via the use of the exact GBM solution:

[ S_ {t+\Delta t}=S_t e^ {\left (r-\frac {1}{2} \sigma^2\right) \Delta 
t+\sigma\sqrt {\Delta t} Z} 
] 

Which is derived from the differential equation: 

dS_t = rS_t,dt+\sigma S_t,dW_t  

Within this model where s_t is the current asset price at time t and St+Δt is the asset price at the next time step, r is the risk-free interest rate of 0.05 
 and sigma represents
the volatility of 0.2. deDlta t is the length of the time step which is 1/252 in this model (this will be covered in the next subsection), and z is a standard 
normal random variable. Z has a mean of 0 SD of 1 and is particularly important as it is the source of randomness inside of this model. Since, z is a standard 
normal 
random variable there is a higher probability of Z being within one standard deviation from the mean (68% between -1 and 1) and a lower probability of being more 
than 2 standard deviations out lower than -2 and higher than 2 (5%) this means that the model will largely contain small positive or negative shocks dependent on 
the value of z. Furthermore, the simulation will be largely comprised of continuous movement since the GBM assumes stock prices to evolve overtime meaning if the 
stock price 
was to go from £10 to £10.01 it would go through every intermediate value. However in a real market we know that stock prices move in 
random ticks thus the model is not the most accurate. The GBM model assumes every shock to be independent of each other meaning a shock at the 
current time step doesn't affect the size of 
the shock at the next step however it is important to not get confused as although in the model the shock directions are independent of each other it is 
important to remember that the stock price at the next time step is still dependent on the current stock price. within a real market behaviors such as volatility clustering, small movements being followed by small and large shocks being followed by 
large shocks are observed but the model is not able to replicate these.

     1.2 Discretizing time and simulating stock paths 
 

Stock volatility measures how much and how quickly a stock’s price 
fluctuates over time therefore reflecting the level of risk and uncertainty in the market thus a large volatility amplifies the size of the shock whereas a small 
volatility would do the opposite. A large volatility within a stock option generally causes the price of the premium to be higher since the stock option has 
a higher 
time value meaning the payoff can be very large since there a wider range of possible outcomes for the paths to take. Similarly, 
the time step can have a great effect on the intrinsic value of the stock causing the time value to increase, since the larger the time to maturity of the stock 
the 
randomly simulated estimates a chance to go very high giving the asset a potentially higher payoff. Furthermore, as the expiry date gets closer the time value 
continues to decrease until it reaches 0. It is important to notice that stocks with extremely high time values have a larger 
premium, meaning a larger intrinsic value may not necessarily mean the profit is much larger unless the payoff is exceptionally large. A
stock with a lower time to maturity or smaller time step will have a lower time value since it has a smaller chance to be able to have an exceptionally large 
surge in value. 

Within this model we use t=1 and n=252 since were taking the time to be a yearlong and a year has 252 trading days. and by doing t/n we get delta t=0.00396825 
which 
is the length of each individual step, by dividing the total time to expiry into 252 steps we're able to form an entire stock price path to the expiry date of 
the 
stock option. The exact GBM solution uses this to simulate the stock price at the next time step. Although you could technically just calculate the value at 
expiry 
splitting each path into 252 steps allows you to generate stock prices throughout the year and visualize how stock prices can change over the course of a trading 
year allowing you to examine the effect of a large shock. Since the estimate can be estimate even without splitting the trading year into 252 steps, the number 
of trading steps just allows us to see a more detailed path rather than just calculating the end option price. The exact GBM model solution can be split into two 
parts part A which 
is the predictable movement which 
predicts the expected movement of the stock at the next time step and part b which is the random component which introduces a random shock via the use of Z. When 
combining these 2 together within the GBM model to produce a simulated value of the stock price at the next time step and will continue doing this until it 
hasn't reached the final estimated price. Since part A of the GBM model solution:

{{t+\Delta t}=S_t e^{(r-\frac12\sigma^2) \Delta t+\sigma\sqrt {\Delta t} Z

consists of the predictable movement it contains the constant delta T (sigma and r will be covered in the next subsection) meaning if the time step is quadrupled 
the 
expected return is also quadrupled. However, this comparison is about the scaling of the drift and random terms in the exponent it is  not a guarantee that the 
stock price itself moves by those 
exact amounts. Within our model since r=0.05 which is the expected stock growth rate and sigma=0.2 the predictable component is 
0.03(deltat) which is strictly positive meaning there's a positive drift so it pushes the stock value up. However the reason the stock price isn't always 
necessarily increasing is due to the random component which can cause the stock price to go up or down randomly this is the reason we can have a lower value than 
we 
started with because the random component can outweigh the effect of the positive predictable drift and cause the stock price to decrease greatly. The random 
movements can partially cancel each other and as 
the number of random movements increases the uncertainty (the difficulty to predict the stock price due to random movement) keeps increasing, but since the random movements 
partly cancel each other out uncertainty does not increase in a straight line instead it increases at a decreasing rate. so, if we increase the time step 4 the predictable affect would also increase 4 
fold since it uses delta t. However uncertainty and the random affect would increase 2 fold since they scale with standard deviation which is the sqrt of delta 
t. so the predictable affect increases proportionally to the time step, whereas the random affect and the uncertainty increase to the square root of the time 
step so uncertainty keeps increasing at a decreasing rate. it is important to note that this is what is expected of the stock and the stock may not follow this 
pattern, uncertainty just means there is a wider variety of paths for the stock price to take.

     1.3 European call options and risk neutral pricing 
 

The goal of this model is to price a European call option; a European call option is a financial contract which gives the buyer the right but not the obligation 
to purchase an underlying asset at a predetermined price. In our model we have a European call option with a strike price of 100 and time to expiry being 1 year 
meaning 1 year from now we can buy that stock for 100 pounds, so if the stock price at expiry is 120 pounds the buyer purchases the option at 100 and 
gets a 20 pound payoff and if the buyer does not exercise their right because the stock falls short, they would get no payoff.

C_T=\max(S_T-K,0) this is the formula for calculating payoff at expiry, C_T represents the payoff at expiry, S_T is the stock price at expiry and K is the strike 
price 
further important note is that the payoff does not account the premium since a stock could have a large payoff buy if the premium is large enough the buyer may 
not gain 
much profit. The premium is the price of the financial contract and we're calculating how to price an option premium today. We do this via simulating the option 
payoff at expiry of a by simulating 100000 paths and we gett the option values of these simulated paths and then calculate the payoff estimate and discount it to 
find out how much its value would be 
today. Since we assumed r to be 0.05 if we had a stock worth 
95.12 pounds today the value of that stock 1 year from now would be 100 pounds meaning that 95.12 pounds compounded continuously from today would equate to 100 
pounds the risk free rate is the expected return of the stock not the actual return that the stock will return from one year. Within the project we discount 
future payoffs 
using our risk free rate of 5% to find out what their value would be today giving us a consistent no arbitrage price. 
This method of discounting the estimated future payoff is known as risk neutral 
option pricing. Risk-neutral pricing lets us price an option by using the risk-free rate as the expected return in our mathematical model, rather than trying to 
predict the stock's actual return. 

Another factor which can affect the options premium is volatility, a large volatility means a stock can have a wider range of possible simulated paths to take 
therefore meaning that the stock option increases potential for both very large upwards or downwards movements and generally a larger volatility dictates a 
larger premium. However, since this is a European call option you cannot have a payoff below 0 pounds since if after the year to expiration has passed the stock price falls below the 
strike price the buyer can choose not to exercise his right meaning then the only loss the buyer would be making is that of the premium. 

these are the option parameters;

Parameter Value Meaning (S_0) £100 Initial stock price (K) £100 Strike price (T) 1 year Time until expiry (r) 5% Risk-free rate (sigma) 20% Annual volatility (n) 
252 Time steps (M) 100,000 Simulations 

it is important to note that the stock is initially at the money since the initial stock price and strike price are the same, and these option parameters give a 
monte Carlo simulation premium of £10.4273 after running the simulation and a black Scholes theoretical premium of £10.4506 where the absolute difference is £0.0233. Furthermore, it is 
important that we use the absolute difference by using the modulus function doing this prevents 2 values one positive and one negative across 2 different 
simulations to cancel each other out. E.g. if you had one estimate from a run as £11 and another as £9.9 the averaged signed error is -0.0006 which makes it look 
like the results are perfect and really close to the benchmark which we know is not the case which we can see is wrong as both values are very far from the 
benchmark whereas in reality the absolute error would be £0.55 since both estimates are each about £0.55 away from the benchmark. 

     1.4 Monte Carlo estimation and sampling error 
     
 
 

To find the option price today we discount the future payoffs and average them across all the simulation run to get the option price toda. To discount them we 
use the risk free interest rate of 0.05 and times is by the time to expiry but since the time to expiry is one year we would just get [ e^{-
rT}=e^{-0.05}=0.951229, and to average all the ran simulations we just find the mean of them shown in the code by np.mean(payoffs) (which represents this formula 
[ \bar X=\frac{1}{M}\sum_{i=1}^{M}X_i ]) and by multiplying this by the discount factor [ e^{-rT} ] where r = 0.05 and t =1, we get the discounted mean option 
price of today. This has been done across all the different simulations that ran from 10-100000. This gives us a reliable option for today. We got the monte 
Carlo estimate of £10.4245 via this method, the £10.4245 (not to be confused with the value of £10.4273 which we got from the set seed in the code) represents a 
mean discounted option price given to us from a simulation of 3 different programs each running 100000 simulation paths, the more simulation paths that are run 
generally the higher the precision of your results, this is due to the law of large numbers where in the context of this project the more simulations u run the 
more the results would converge to one value the sample mean converges towards the theoretical expected discounted payoff under the risk-neutral model. Under the 
same model assumptions, this is the Black–Scholes price of £10.4506. In probability theory, the law of large numbers is a mathematical law which states that the 
average of the results obtained from many independent random samples converges to the true value, if it exists. More formally, the law of large numbers of states 
that given a sample of independent and identically distributed values, the sample mean converges to the true mean. Although this does not guarantee that every 
increase causes the runs to get more accurate due to fluctuations caused by randomness. Random sampling may cause absolute error to increase between simulation 
runs. This is also why simulation runs with less simulations; the absolute error can be so high since 1 outlier can affect it. This will also be seen in graph 3 
later within this subsection. 

Standard error measures the sampling uncertainty of the monte Carlo estimate whereas the absolute error measures how far my Montecarlo estimate is from the black 
Scholes benchmark both ignoring whether the effect is positive or negative. standard error decreases as the number of simulations increases ((1/\sqrt M) because 
averaging a larger number of independent simulated payoffs reduces the sampling variability of the estimated mean. 

SE=\frac{s} {\sqrt M} 

Furthermore graph 2 is a visual representation of the Monte Carlo simulation we see, as the number of simulations increases the precision increases as sampling 
variability decreases the estimate generally converges towards the theoretical expected value, showing a general trend of increase in accuracy as time passes. 
Further consistency of the results around the theoretical price in comparison to the benchmark increases however there are periods where the random shock causes the results to move 
further away from the black Scholes benchmark, however the overall trend of the graph suggests convergence to the Black Scholes theoretical price. As mentioned 
previously its due to the law of large numbers which states, with fewer simulation counts individual random movements have a greater influence on the average 
payoff. Therefore as the number of simulations increases the influence of those singular random movements decreases and the average estimate becomes a clearer 
representation of the true mean. Furthermore, convergence is not always linear or smooth as i mentioned before an estimate with 100 simulations could be closer 
to the 
benchmark than an estimate with 1000 simulations but the one with 1000 simulations would still have lower standard error and less sampling uncertainty it would just be affected by random 
variation. Convergence refers to approaching a limit closer as the number of simulations increases which is what we generally see in the graph.

Graph 3 on the other hand shows absolute difference which is the difference between the Black Scholes benchmark and the average Monte Carlo payoff that we get. 
This graph gives a visual representation of how absolute error decreases as more simulations are run. The formula for absolute error is: 

text {Absolute Error} =|text {MC estimate}-text {BS price} 

Which is the modulus of the monte Carlo estimate minus the Black Scholes benchmark and the reason for using absolute error and by extension modulus, is because 
we don't want any positive values and negative values cancelling each other out. We see in this graph that Absolute error generally tends to decrease as the number of simulations increases because 
the sample mean converges towards the theoretical expected value. However, individual errors can fluctuate due to random sampling and are visible in the graph. absolute error calculates the 
difference between one monte Carlo estimate and the black Scholes benchmark, graph 2 shows convergence which graph 3 also partially shows but graph 3 emphasizes 
the accuracy of individual simulation runs in comparison with the black Scholes benchmark.

                         IMPORTANT NOTE THIS RESPIRATORY ONLY CONTAINS THE CODES FOR THE GRAPHS IF YOU ACTUALLY WANT TO SEE THEM ENTER THE CODE INTO GOOGLE COLLAB

    The Black Scholes formula  
 

The black Scholes model formula is: 

C=S_0N(d_1)-Ke^{-rT}N(d_2)

where d_1 and d_2 can be found out using these formulas: 

d_1= \frac{\ln(S_0/K) +(r+\frac12\sigma^2) T} {\sigma\sqrt T} 

d_2=d_1-\sigma\sqrt T 

The formula which is given can be split into 2 parts the first term and the second term whereby using the option parameters (found below) the model calculates a 
fair theoretical option price today producing the benchmark we used throughout this project of £10.4506. The 1st term {S_0N(d_1)} represents the stock-price 
component of the option's theoretical value (a fancy way of saying the value of the share accounted for uncertainty); the model factors in uncertainty and 
formulates where the stock value could go. The second term {Ke^{-rT}N(d_2)} represents the strike price of the stock being discounted adjusted by N(d_2) thus the 
discounting factors present and after subtracting them from each other we get C which is the theoretical premium. Furthermore there is no need to use Z as we did 
in the Monte Carlo part of the project as were not running simulations to find an estimate instead were using the option parameters to form a reliable 
theoretical option price value. Within the black Scholes formula you have d1 and d2 (2 mathematical constants) which can be both worked out by substituting in 
the option parameters into the formulas above once you do that you would get d_1= 0.35 and d_2 = 0.15 which are mathematical intermediates which are substituted 
into a normal distribution with mean 0 and standard deviation 1 aka the z distribution. The Z distribution is used since the black Scholes uses the standard 
normal distribution, why is the normal distribution? It's because in the GBM we use a log normal distribution meaning the stock price is logged and is normally 
distributed so we get not negative values to find random values of Z and the black Scholes model also assumes the stock prices to follow the GBM which assumes 
the logarithmically simulated stock paths follow a normal distribution both the monte Carlo and the black Scholes model use the normal distribution due to the 
underlying GBM model which both use. This allows the black Scholes to also account for uncertainty, however rather than using z to pull individual simulated 
paths it just calculates the end theoretical option price directly. By applying the cumulative standard normal distribution to d_1 and d_2, we obtain N (0.35) 
=0.6368 and N (0.15) =0.5596. These values represent the proportion of the area under the standard normal distribution lying to the left of each respective value 
the values were converted by the standard normal distribution into values that can be used in the black Scholes model formula. However, they each represent 
something important and are not just mathematical constants, N(d_2) represents the probability that the stock ends up in the money (above the strike price) under 
the risk neutral model. N(d_1) represents the stock price weight across a range of values which in simple words means the model also accounts for the size of the 
potential payoff, not just whether it is in the money. N(d_1) itself is not the size of the potential payoff; it is a weighing factor which helps calculate how 
large the payoff could be. (N(d_1)) and (N(d_2)) are the cumulative probabilities obtained by evaluating those values in the standard normal distribution. 

Component Meaning C The theoretical option price today S_0 Current stock price (£100) K Strike price (£100) r Risk-free interest rate (0.05) T Time until expiry 
(1 year) e^{-rT} Discount factor N(d_1) Cumulative standard normal probability evaluated at d_1 N(d_2) Cumulative standard normal probability evaluated at d_2 

 C=100(0.6368)-100e^ {-0.05} (0.5596)

 C\approx. £10.4506

     Model assumptions and limitations  
 

Limitations of this model are that the Geometric Brownian Motion assumes the random shocks to be normally distributed therefore a real market is not accurately 
replicated since in a normal distribution most values are around 0 however in a real market large price gaps can occur more frequently than a normal distribution 
can predict therefore the shocks may not be representative of something that would occur in a real market. Furthermore, another limitation of the GBM being every new 
shock is independent of previous shocks, whereas real markets show behaviors such as volatility clustering where a large movement one day can be followed with 
another large movement and small movements are often followed by other small movements Which the GBM cannot show.

Within real markets certain 
announcements from a company or events can show large temporary jumps in option price but since the GBM generates continuous stock paths meaning the stock path 
changes without sudden jumps, this is an important limitation of this model since it's a model that simplifies what an actual market would run like. It is 
important to note that in this project i did not take into account the possibility of the volatility and the risk free interest rate changing as time progresses, 
whereas in reality the risk free interest rate could change in a real market due to decisions made by the bank of England and volatility can also change 
dependent on economic uncertainty and company news such as the company gaining bad reputation for something they have done. 

Why did i use the black Scholes as the benchmark? 
 

It is to check whether the simulation works, the black Scholes model factors in all of the stock option parameters and it gives an analytical theoretical price 
using the black Scholes call pricing formula it is not a guess but a theoretical price which is derived using a mathematical formula under the option parameters. 
this price is not guaranteed if it ran, instead it is a theoretical estimate made using the model assumptions. And it is used to compare against the Monte Carlo 
simulation and check for the model's accuracy, if the 2 persistently show a large absolute error even after increasing the simulation count it may be indicative 
of a mathematical/coding/calculation error. 

    Monte Carlo convergence. 
 

It is also important to notice that randomness plays a part within the monte Carlo simulation, you can run the same program with the same option parameters and 
same number or simulations ran but get a different value and that is simply because the monte Carlo simulation generates the estimate using randomly generated 
outcomes increasing the number of simulations generally reduces sampling error however running more simulations cause individual random outcomes to have less of 
an effect on the estimate therefore a run with a thousand simulations may get lucky and get less absolute error than a run with 10000 simulations simply because 
were working with random outcomes The mathematical idea is that the sampling variability generally decreases as the number of simulations increases. Furthermore, 
to further investigate the consistency we have run each simulation at each count multiple times, i.e., i have run the simulations for 1000, 10K..... multiple 
times check it now, are we missing any vital information is there anything else we can add, or anything we can change to this to link it back to the original 
questions or is there no need to do that 

    Conclusion 
 

the main result i came to was that the monte Carlo simulation can accurately estimate the option price when compared to its black Scholes benchmark of £10.4506 
under the models' assumptions 

The model found that increasing the number of simulations ran decreased the sampling error and variability, making the results more stable and consistent since 
there was a lower chance for individual random results to affect the overall trend. however, increasing the number of simulations does not always decrease the 
absolute error since the averages can still show difference from the benchmark due to randomness. PS the model simulates stock prices to estimate the option 
price; it does not predict the price of the stock itself. 

Why use the monte Carlo simulation if we know that the black Scholes model gives a reliable stock option price? 

the point of this project was not to simply calculate the option price since otherwise there would be no need for the monte Carlo simulation to be ran since the 
black Scoles can find a reliable theoretical option price relatively easily instead the monte Carlo simulation was used to investigate: how simulation based 
pricing can estimate an options value reliably, we did this via the use of the monte Carlo simulation which via running a bunch of simulation paths we got option 
prices across many simulation runs and by averaging all the simulation paths and discounting them we get a reliable premium for the stock option today and to 
check whether the results were reliable we compared the results against the black Scholes model for accuracy and consistency which ties in with testing for 
sampling variability which means as the number of simulations increases the consistent and precision of the monte Carlo estimates would increase however this 
does not mean that every estimate would continue getting closer to the black Scholes benchmark with every increase in M u guarantee a decrease in sampling 
variability and improvement in precision this does not mean that the absolute error will always decrease. 

and the monte Carlo convergence which is a visual representation of the law of large number which states that increasing the number of simulations would bring 
the estimates closer to the benchmark Practical applications: Monte Carlo is particularly useful for more complex financial derivatives where an analytical 
pricing formula may not be available. in complicated derivatives such as those with less model assumptions the black Scholes cannot be used as it is only useful 
with specific model assumptions the monte Carlo simulation can still be ran millions of simulated paths can still come together to form an appropriate option 
price whereas a black Scholes model could not price a European call option with multiple expiry points, which a monte Carlo model can do provided you use an 
appropriate model. 

