This repository contains data, analysis, and predictions related to Wuhan2019 coronavirus epidemic.


## Epidemic model basics

One of the basic models that captures the dynamics of an epidemic is called SIR model[[1]](https://en.wikipedia.org/wiki/Compartmental_models_in_epidemiology#The_SIR_model). The model consists of three compartments – S for the number susceptible, I for the number of infectious, and R for the number recovered (or immune). We will start with this model and learn about the dynamics of Wuhan2019 coronavirus. The model can be expressed by the following set of ordinary differential equations:

<p align="center">
  <img src="https://latex.codecogs.com/gif.latex?\\\frac{dS}{dt}&space;=&space;-\frac{\beta&space;I&space;S}{N}&space;\\&space;\frac{dI}{dt}&space;=&space;\frac{\beta&space;I&space;S}{N}&space;-&space;\gamma&space;I&space;\\&space;\frac{dR}{dt}&space;=&space;\gamma&space;I" title="\\\frac{dS}{dt} = -\frac{\beta I S}{N} \\ \frac{dI}{dt} = \frac{\beta I S}{N} - \gamma I \\ \frac{dR}{dt} = \gamma I" />
</p>


where $S$ is the susceptible population, $I$ is the infected while $R$ represents the recovered. It follows that $S(t)+I(t)+R(t)=Constanct=N$ where $N$ is the total population. The two transition rates: $\beta$ represents is the average number of contacts per person times the probability of disease transmission in a contact between a susceptible and an infectious subject; $\gamma$ can be interpreted as mean recovery rate, thus $\gamma=1/D$ where $D$ is the average duration of the infection.

## Basic reproduction ratio

An important ratio $R_0=\frac{\beta}{\gamma}$, is the so-called [basic reproduction ratio](https://en.wikipedia.org/wiki/Basic_reproduction_number). From a recently published manuscript by Zhang and Shen, et al [[2]](https://www.biorxiv.org/content/10.1101/2020.01.23.916726v1.full.pdf), the ratio $R_0$ was estimated to be 4.71 (4.50-4.92) when the epidemic started on 12th December 2019, but its effective reproduction numbers dropped to 2.08 (1.99-2.18) as of 22th January 2020. Compared with SARS and MERS, 2019-nCov was similar to SARS ($R_0$=4.91) and MERS in Jeddah (3.5-6.7).


## Prediction setup

| Parameter   | Value         | Description                                     |
|-------------|---------------|-------------------------------------------------|
| Day 1       | 12/01/2019    | The first day of infection                      |
| Control Day | 01/23/2020    | The day of transportation shutdown              |
| N           | 1,400,000,000 | Total population                                |
| I0          | 1             | Initial infected population                     |
| Re0         | 0             | Initial recovered population                    |
| S0          | 1,399,999,999 | Susceptible to infection                        |
| R0-P1       | 2.9           | Basic reproduction rate in Phase1               |
| R0-P2       | 0.29          | Basic reproduction rate in Phase2               |
| gamma       | 0.1           | Recovery rate, cure duration 10 days on average |


We set day 1 to be Dec. 1st 2019. On 8 December 2019, the first patient was recorded by Chinese authorities to start showing symptoms. A later Chinese report finds an earlier case on 1 December, pointing to an even earlier origin.[[3]](https://www.sciencemag.org/news/2020/01/wuhan-seafood-market-may-not-be-source-novel-virus-spreading-globally).

It is natural to break down the whole process into two major phases - contagion phase, and control phase, separated by the transportation shutdown on 01/23/2020. This is a very approximate way of modeling as there would not be a cliff-like change in reproduction rate, but only uses as a reference in this study.

In the contagion phase, while the virus is highly contagious, not sufficient protections were taken, the reproduction rate (R0-P1) is 2.9. This is a prorated number (compared to 4.71) after analyzing the current spread status. However, a small change on the rate could have substantial impact. In phase 2, the reproduction rate of the virus would not change, however, the transportation shutdown puts a significant impact on the reproduction rate (R0-P2), effectively, the rate can be deducted by 90%.

The recovery rate, based on the reported medical records [[4]](http://www.xinhuanet.com/politics/2020-01/29/c_1125510896.htm),[[5]](http://www.xinhuanet.com/2020-01/29/c_1125510585.htm), can be estimated to be 10 days on average.


## Simulated prediction

A simulation of 90 days was performed starting from Dec. 1th 2019 until Feb. 28th 2020.

<img src="https://github.com/kealyn/Wuhan2019/blob/master/vsactual.png" width="800">

<img src="https://github.com/kealyn/Wuhan2019/blob/master/tencent.png" width="800">

_The data is collected up to Jan. 31st, 2020_

As noted, the infected people kept a steady increment until the transportation was shut and then followed by a drop tail. The peak number of people infected is approximately 23,000. This number could be different from the publicly announced numbers [[6]](https://news.qq.com/zt2020/page/feiyan.htm) due to a couple of reasons:
* The result here is from a basic model and simulation, with an estimated R0, thus could be very approximate.
* The number of confirmed cases would always be less than the number of actual infected cases
* The incubation period of the virus varies from 2 days up to 14 days, there will be a catch up on the number of confirmed cases with the same delay

The actual data of publicly announced cases are collected by the authors from [[2]](https://www.biorxiv.org/content/10.1101/2020.01.23.916726v1.full.pdf), I extended by additional days.

From the above points, it is understandable there would be a delay in discovering cases - 15 days, based on empirical results. So we can deduce from the simulations:
* The peak number of infections is around 20,000 from public report
* The reported confirmed cases would reach this peak around 15 days later, i.e. 68 days after Dec. 1st, which is approximately Feb. 8th.


I also did a quick study on the case if the transportation shut was not performed in time.


<img src="https://github.com/kealyn/Wuhan2019/blob/master/no_transportation_shut.png" width="800">

As we could see, the model tells us there will be 5,663 people infected by the virus if the transportation shutdown was delayed only by 1 day.


## Reference

[1] https://en.wikipedia.org/wiki/Compartmental_models_in_epidemiology#The_SIR_model
[2] Modelling the epidemic trend of the 2019 novel coronavirus outbreak in China, https://www.biorxiv.org/content/10.1101/2020.01.23.916726v1.full.pdf
[3] https://www.sciencemag.org/news/2020/01/wuhan-seafood-market-may-not-be-source-novel-virus-spreading-globally
[4] 武汉感染新型冠状病毒肺炎医护人员首批集中出院，复盘救治过程有哪些发现？http://www.xinhuanet.com/politics/2020-01/29/c_1125510896.htm
[5] “病毒无情人有情！” 四川首例治愈病人出院高喊“武汉加油” http://www.xinhuanet.com/2020-01/29/c_1125510585.htm

