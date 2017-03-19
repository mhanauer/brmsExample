# brmsExample
#install_github("paul-buerkner/brms")
data("kidney")
kidney
#install.packages("rstan", type = "source")
#install.packages("rstan", repos = "https://cloud.r-project.org/", dependencies=TRUE)
library(rstan)
library(brms)
fit1 <- brm(formula = time | cens(censored) ~ age * sex + disease
            + (1 + age|patient),
            data = kidney, family = lognormal(),
            prior = c(set_prior("normal(0,5)", class = "b"),
                      set_prior("cauchy(0,2)", class = "sd"),
                      set_prior("lkj(2)", class = "cor")),
            warmup = 1000, iter = 2000, chains = 4,
            control = list(adapt_delta = 0.95))
summary(fit1, waic = TRUE)
