---
title: 'Bayesian machine learning:A/B Testing'
date: 2019-06-23 14:09:08
tags:
mathjax: true
categories:
- Statistics & Algorithms
---

Entry level, very easy

## Bayesian Basic
Difference in Frequentist(point estimate) and Bayesian(distribution):
Frequentist: $\hat{\theta}=argmax_{\theta}P(X|\theta)$
Bayesian: $P(\theta|X)$

The basic is 
$$p(A|B)=p(A,B)/p(B)$$
which is equal to 
$$p(A|B)=p(B|A)p(A)/p(B)$$
Some rules:
1.for discrete distributions, $p(B)=\sum_{A}p(A,B)=\sum_{A}p(B|A)p(A)$
2.for continuous distributions, $p(A|B)=\frac{p(B|A)p(A)}{\int {p(B|A)p(A)} \, {\rm d}A}$

Some related basic concepts:
1.Sensitivity: p(pred=1|disease=1)=p(pred=1,disease=1)/p(disease=1)=(TP/N)/((TP+FN)/N)=TP/(TP+FN)
2.Specificity: p(pred=0|disease=0)=p(pred=0,disease=0)/p(disease=0)=TN/(TN+FP)
3.Precision: TP/(TP+FP)=P(pred=1,disease=1)/p(pred=1)
4.Joint Probability density, assume gaussian distribution: $p(x_{1},x_{2},...,x_{N}=\prod_{i=1}^N \frac{1}{sqrt(2\pi 
(\sigma)^2)} exp(-1/2 \frac{(x_{i}-\mu)^2}{(\sigma)^2})$.

## Traditional A/B Testing
Example: You have a landing page where you get people to signup, but not everyone who visits your site will signup so that the conversion rate is the proportion of people who sign up. You want to compare your page with another new page. We know that the conversion rate(page1)=1/10 vs rate(page2)=2/10 is not as good as rate(page1)=10/100 vs rate(page2)=20/100. 
**Simple A/B Testing Recipe**
t test statistic $t=\frac{\overline{X_{1}} - \overline{X_{2}}}{s_{pool}\sqrt{2/N}}$ where $s_{pool}=\sqrt{\frac{(s_1)^2+(s_2)^2}{2}}$.
If the size of each group is different, t test statistic $t=\frac{\overline{X_{1}} - \overline{X_{2}}}{s_{pool}\sqrt{1/n_{1}+1/n_{2}}}$.
If the standard deviation of each group is different, use "Welch's t test". 
```
import numpy as np
from scipy import stats

N = 10
a = np.random.randn(N)+2
b = np.random.randn(N)

var_a = a.var(ddof=1)
var_b = b.var(ddof=1)
s = np.sqrt((var_a+var_b)/2)
t = (a.mean()-b.mean()) / (s*np.sqrt(2/N))
df = 2*N-2
p = 1-stats.t.cdf(t,df=df)
print "t:\t", t, "p:\t", 2*p

#the same result as built in scipy function
t2, p2 = stats.ttest_ind(a,b)
```

chi-square test statistic $(\chi)^2=\sum_{i} \frac{(observed_{i}-expected_{i})^2}{expected_{i}}$. Example in the course on udemy
```
import numpy as np
from scipy.stats import chi2
import matplotlib.pyplot as plt

class DataGenerator:
    def __init__(self,p1,p2):
        self.p1 = p1
        self.p2 = p2
    
    def next(self):
        click1 = 1 if (np.random.random()<self.p1) else 0
        click2 = 1 if (np.random.random()<self.p2) else 0
        return click1, click2


def get_p_value(T):
    det = T[0,0]*T[1,1]- T[0,1]*T[1,0]
    c2 = float(det)/T[0].sum()*det/T[1].sum()*T.sum()/T[:,0].sum()/T[:,1].sum()
    p = 1-chi2.cdf(x=c2,df=1)
    return p
    
def run_experiment(p1,p2,N):
    data = DataGenerator(p1,p2)
    p_values = np.empty(N)
    T = np.zeros((2,2)).astype(np.float32)
    for i in range(N):
        c1, c2 = data.next()
        T[0,c1] += 1
        T[0,c2] += 1
        if i < 10:
            p_values[i] = None
        else:
            p_values[i] = get_p_value(T)
    plt.plot(p_values)
    #plt.plot(np.ones(N)*0.05)
    plt.show()

run_experiment(0.1,0.11,20000)
```

practice in the course on udemy
```
from __future__ import print_function, division
from builtins import range
from scipy.stats import chi2, chi2_contingency
import numpy as np
import pandas as pd

#%%
#read data in
dat = pd.read_csv("D:/machine_learning_examples/ab_testing/advertisement_clicks.csv")
dat.head()

a = dat[dat["advertisement_id"] == "A"]
b = dat[dat["advertisement_id"] == "B"]
a = a["action"]
b = b["action"]

#%%
# function to calculate p
def get_p_value(T):
    det = T[0,0]*T[1,1]-T[0,1]*T[1,0]
    c2 = float(det) / T[0].sum() * det / T[1].sum() * T.sum() / T[:,0].sum() / T[:,1].sum()
    p = 1-chi2.cdf(x=c2, df=1)
    return p

A_clk = a.sum()
B_clk = b.sum()
A_noclick = a.size-a.sum()
B_noclick = b.size-b.sum()

T = np.array([[A_clk,A_noclick],[B_clk,B_noclick]])

print(get_p_value(T))
```

## Bayesian A/B Testing
The Explore/Exploit problem: If you get 3/3 from bandit 1, 0/3 from bandit 2, should you exploit bandit 1 or explore more bandits? Or as for the advertisement A and advertisement B, if A has more clicks than B in 10 clicks, should you exploit advertisement A or explore more?
**Epsilon-Greedy algorithm**
Choose a small number of epsilon between 0 and 1, and generate a random probability, if p < epsilon, then explore, otherwise exploit.

**UCBI**
Define upper and lower limit to represent where we believe true CTR(click-through rate) is. Using a Chernoff-Hoeffding bound, $\epsilon > 0$, then $P(\hat{\mu} > \mu + \epsilon) \leq exp(-2(\epsilon)^2 N)$ and the opposite site $P(\hat{\mu} < \mu + \epsilon) \leq exp(-2(\epsilon)^2 N)$, which is equal to say $P(|\hat{\mu} - \mu| \leq \epsilon) > 1-2exp(-2(\epsilon)^2 N)$. Therefore the probability of $\mu$ in an interval is larger than a function of the number of games.
We choose the epsilon $\epsilon = \sqrt{\frac{2lnN}{N_j}}$, where N is the number of total games and $N_j$ is the number of games played in bandit j.

**Bayesian A/B Testing**
$$P(\theta | X)=\frac{P(X|\theta)P(\theta)}{P(X)}$$
1.Conjugate Priors: If we choose beta distributions for $P(X|\theta)$ and $P(\theta)$, then we can make $P(\theta | X)$ have the same type of distribution as $P(\theta)$. 
Example:
$$P(\theta | X)=Beta(a^{'},b^{'})$$ where $a^{'}=a+\sum_{i=1}{N}x_{i}$ and $b^{'}=b+N-\sum_{i=1}{N}x_{i}$. And the prior is beta(1,1), which is equal to uniform(0,1).
```
#%%
#Bayesian A/B Testing simulation
import matplotlib.pyplot as plt
import numpy as np
from scipy.stats import beta

NUM_TRIALS = 2000
#true click-through rates
BANDIT_PROBABILITIES =[0.2,0.5,0.75]

class Bandit:
    def __init__(self,p):
        self.p=p
        self.a=1
        self.b=1
    
    def pull(self):
        #less than p, get 1 that if click-through rate is 0.2, random prob less than 0.2 will return 1 so 20% will return 1.
        return np.random.random()<self.p

    def sample(self):
        return np.random.beta(self.a,self.b)
    
    def update(self,x):
        self.a+=x
        self.b+=1-x

def plot(bandits, trial):
    x=np.linspace(0,1,200)
    for b in bandits:
        y=beta.pdf(x,b.a,b.b)
        plt.plot(x,y,label="real p: %.4f" % b.p)
    plt.title("Bandit distributions after %s trials" % trial)
    plt.legend()
    plt.show()

def experiment():
    bandits = [Bandit(p) for p in BANDIT_PROBABILITIES]

    sample_points=[5,10,20,50,100,200,500,1000,1500,1999]
    for i in range(NUM_TRIALS):
        bestb=None
        maxsample=-1
        allsamples=[]
        for b in bandits:
            sample=b.sample()
            allsamples.append("%.4f" % sample)
            if sample > maxsample:
                maxsample=sample
                bestb=b
        if i in sample_points:
            print("current samples: %s" % (allsamples))
            plot(bandits,i)
        x=bestb.pull()
        bestb.update(x)
if __name__=="__main__":
    experiment()
```

**Thompson Sampling Convergence**
Assuming the CTR $\theta$ of each bandit follows the distribution $beta((\alpha)_k, (\beta)_k)$. Everytime producing a random number from each bandit following the current distribution, and select the bandit with largest number. If it wins, then $\alpha = \alpha +1$, otherwise $\beta =\beta +1$. And interates until the CTR converges.
```
import matplotlib.pyplot as plt
import numpy as np
from bayesian_bandit import Bandit

def run_experiment(p1, p2, p3, N):
    bandits = [Bandit(p1),Bandit(p2),Bandit(p3)]
    data = np.empty(N)

    for i in range(N):
        j=np.argmax([b.sample() for b in bandits])
        x=bandits[j].pull()
        bandits[j].update(x)

        data[i]=x
    cumulative_average_ctr=np.cumsum(data) / (np.arange(N) + 1)

    plt.plot(cumulative_average_ctr)
    plot(np.ones(N)*p1)
    plot(np.ones(N)*p2)
    plot(np.ones(N)*p3)
    plt.ylim((0,1))
    plt.xscale("log")
    plt.show()

run_experiment(0.2,0.25,0.3,100000)
```




