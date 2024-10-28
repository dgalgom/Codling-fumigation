# Codling-fumigation
The research that generated these data was in part funded by New Zealand pipfruit growers. Data are from trials that studied the mortality response of codling moth to fumigation with methyl bromide.

## Variables explanation
  + `dose`: injected dose of methyl bromide, in mg per cubic meter 
  + `tot`: number of insects in chamber
  + `dead`: number of insects dying
  + `pobs`: proportion dying
  + `cm`: control mortality (i.e., at dose 0)
  + `ct`: concentration-tim sum
  + `Cultivar`: a factor with levels `BRAEBURN`, `FUJI`, `GRANNY`, `Gala`, `ROYAL`, `Red Delicious`, and `Splendour`
  + `gp`: a factor which has a different level for each different combination of `Cultivar`, `year`, and `rep` (replicate)
  + `year`: a factor with levels 1988 and 1989
  + `numcm`: total number of control insects

The main purpose of this data analysis was to compare the model fit and biological plausibility of the predictions of different models using two different likelihoods: **Gaussian** and **beta**.

## Model fitting

Our linear model (i.e., Gaussian) was fitted using the `brms` package: `model_1 <- brm(pobs ~ 1 + Cultivar + dose, data = data, family = gaussian, chains = 4, iter = 2000, seed = 25102024)`;
And the beta model was fitted like this: `model_2 <- brm(pobs ~ 1 + Cultivar + dose, data = data, family = Beta, chains = 4, iter = 2000, seed = 25102024)`

### Model fit comparison
Comparing model fit parametes and posterior predictive checks, the beta model seems to present slightly better fit.
![plotsss](https://github.com/user-attachments/assets/92213546-1a59-4059-bc4c-0704017ad50c)

There was great overlapping within and between Markov Chain Monte Carlo chains of the model parameters. 

![pp_lin](https://github.com/user-attachments/assets/daf01ae7-ab7c-450f-9e06-e9331b92cdac)
![pp_beta](https://github.com/user-attachments/assets/b0654fd5-c4d1-4373-a9a7-24b0b9cc89a7)

Posterior predictive checks seem to favour the beta model too (the second image).

## Predictions

![lin_pred](https://github.com/user-attachments/assets/ce369700-18bc-49d9-9731-ddf684602b1d)

When we fitted the predictions for the average marginal effects for the applied dose (i.e., a continuous predictor), these predictions from the beta model respect the boundaries across the full range of doses and proportions (i.e., the horizontal white lines at the edges on the panels mark off the (0, 1) boundaries for proportion values, and the vertical ones mark off the dose range (0, 30)). Nevertheless, the predictions for the Gaussian model cross the upper and lower limits in the y-axis. Besides, our beta model yielded predictions that were considered more biologically plausible than the linear one based on scientific evidence, a factor that sould be taken into account when interpreting our results.

![beta_pred](https://github.com/user-attachments/assets/f85b4eb8-d083-40bd-8882-93b8b6bfbdf4)

### Comparing both set of predictions

![comb_pred](https://github.com/user-attachments/assets/e314d71d-4920-4532-9780-622f7a2a0022)


## BONUS: the importance to be efficient

![average_dr](https://github.com/user-attachments/assets/2b97eae6-ac57-44d2-8448-21a4deff134c)

We can predict in a more accurate way which is the minimal effective dose required to reach a predefined effective threshold (e.g., in this case, we established that a treatment was effective if it eliminated, at least, 75% of the insects). 
That is of the utmost importance because of (1) the economic impact that could have apply a lower dose that is considered minimally effective; and (2) for the sake of efficiency (i.e., lower doses require less detoxification time). On average, doses of 22 mg achieved the minimal effective threshold. However, in the practice, we have to select one of these treatments, and we want to select the optimal one (i.e., the one that using a lower dose eliminates the same or a higher number of insects).
Therefore, we predicted at which dose each treatment reached the 75% of eliminates insects, and the "Red Delicious" treatment was considered the most efficient: at 21 mg, it eliminates roughly 77% of the insects with a 95% credible interval from 73% to 80% (in fact, this one had associated the lowest uncertainty in their estimates).


|Cultivar     |dose|.epred   |.lower   |.upper   |.width|.point|.interval|
|------------:|----|---------|---------|---------|------|------|---------|
|BRAEBURN     |21  |0.7538827|0.6915763|0.8106397|0.95  |mean  |qi       |
|FUJI         |24  |0.7535776|0.6867785|0.8123199|0.95  |mean  |qi       |
|Gala         |21  |0.7512868|0.6872217|0.8068230|0.95  |mean  |qi       |
|GRANNY       |23  |0.7638317|0.7017598|0.8200018|0.95  |mean  |qi       |
|Red Delicious|21  |0.7689771|0.7327355|0.8038977|0.95  |mean  |qi       |
|ROYAL        |25  |0.7714857|0.6991540|0.8336567|0.95  |mean  |qi       |
|Splendour    |22  |0.7670987|0.7042669|0.8250529|0.95  |mean  |qi       |
