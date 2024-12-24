---
title: Solar Index Insurance
short_title: Solar Index Insurance
subtitle: Bootstrap pricing methods to reduce basis risk
keywords: [solar energy, insurance]

authors:
  - name: Naoki S. Conning
    affiliations:
      - id: stuy
        institution: Stuyvesant High School
        city: New York
        region: NY
        postal_code: 11372
        email: nconning60@stuy.edu



exports:
  - format: tex
    template: plain_latex
    output: exports/mypaper.tex
# use: `myst build mypaper.md --pdf`

downloads:
  - file: exports/mypaper.pdf
    title: A PDF of the article

bibliography:
  - references.bib



abstract: |
  The goal of this project is to develop an open-source framework for end-to-end analysis of the design and pricing of location and system-specific solar power option contracts. 
  
---

## Introduction

The sun offers an inexhaustible source of clean energy, yet the world still relies on the costly and polluting extraction of fossil fuels for over 80% of its energy needs.   Fortunately, recent rapid technological progress has made solar photovoltaic (PV) generation the most cost-effective method for generating electricity in many regions, according to the International Energy Agency (IEA). This, coupled with the adaptability of PV systems—from residential rooftops to expansive solar farms—creates a unique opportunity to decentralize and democratize the energy grid and protect the environment.

Unfortunately, while solar energy offers attractive returns, the upfront investment and inherent variability of solar power generation acts as a deterrent to wider adoption. This is particularly true for smaller adopters who may lack the capacity to evaluate risks or absorb potential fluctuations. 

The goal of this project is to develop an open-source framework for end-to-end analysis of the design and pricing of location and system-specific solar power option contracts. 

The aim is to provide a chain of accessible tools and models to empower informed decision-making, mitigate financial risks associated with solar power variability, and accelerate the transition to a decentralized renewable energy future.


Some interesting references include {cite:p}`salazar-pena2023` on bootstrap methods.

Our China data is described in {cite:t}`salazar-pena2023`

## Why solar parametric insurance?


Solar power output is variable: generated power is dependent on cloud cover and other atmospheric conditions that vary greatly by  day, time, location, and equipment.

Uncertainty reduction to spur adoption: Solar projects require large up front investments to generate expected but risky returns. Adopters, specifically residential or small-scale farms fear of being unable to meet mortgage or income obligations.

Parametric insurance advantages: unlike conventional indemnity insurance which involves costly and time-consuming damage assessments, payouts are made quickly based on simple trigger events such as lower than threshold solar irradiance. Readily available and verifiable data.

Disadvantages:  solar panel power output is affected by many other factors such as temperature, and every panel performs differently under different circumstances. Insured may remain exposed to uncompensated 'basis risk'.


## Improving parametric insurance

Multi-Variable Precision: An index based on predicted output accounts for variables other than irradiance, that factor into PV output, including temperature, and system specific variables.

Validated Models: PVLIB (see output modeling box) is a trusted, open source model developed by solar energy scientists.

Greater Accuracy: Using a trusted Chinese solar farm dataset (Yao et al, 2021) PVLIB predicted power output had a 0.96 correlation with actual output. An irradiance only index achieves only a 0.89 correlation. A large literature validates PVLIB predictive performance. 

See the Proof: The graphs at right demonstrates the close alignment between PVLIB's predicted output and actual measured output, over the course of several days, and from a solar station over a full year.



:::{figure} ./images/actualpv.png
:label: actualpv
:align: center

Actual PV output from PVOD station 7 and PVLIB simulated output over 10 day span
:::


## Lower cost insurance through better modeling

When we insure solar power systems, we need a way to know when to make payouts. The simplest approach is to look at solar irradiance 
- basically how much sunlight is hitting the ground - and pay when it drops below a certain level. 
But solar panels don't just depend on sunlight. Temperature and humidity ond other environmental factors also affect how 
efficiently they work. When our insurance payouts don't match up well with actual output shortfalls (and hence income flows), we call this "basis risk."

For example, we could have people buy car insurance based only on a weather index that pays out when it rains, given that accidents 
are more likely to happen when it rains. But accidents depend on many other factors - road conditions, traffic, driver behavior. 
Rain index insurance leaves 
Similarly, solar power output depends on more than just sunlight. When we use better models that account for all these factors (like the pvlib library that simulates solar panel performance), we can match payouts more closely to actual losses. This reduces basis risk.

Why does this matter for insurance costs? Insurance companies hate uncertainty. When they're unsure about whether their payouts will match actual losses, they charge extra to cover this uncertainty. It's like adding a safety buffer. They also need to keep more money in reserve, just in case. When we reduce basis risk by using better models, we can shrink this safety buffer and reduce the reserves needed. This means lower costs that can be passed on as savings to customers.

There are other ways to make insurance cheaper too. By understanding exactly when solar output is more versus less predictable (like those summer months when variance is lower), insurance companies can charge lower premiums during safer periods. And when they insure multiple solar installations, they can balance risks across their portfolio - kind of like diversifying an investment portfolio - which can also lead to lower costs.



## Bootsrapping

Bootstrap methods are a powerful tool for estimating the distribution of a statistic by resampling with replacement from the data. This is particularly useful when the distribution of the statistic is unknown or difficult to derive analytically.


When certain conditions are met -- intuitively that the data is 'representative' of the population -- then resampling from the data can approximate sampling from the population.

In the context of solar insurance, bootstrapping can be used to estimate the distribution of solar power output, and thus the distribution of payouts under a parametric insurance contract. This can be used to price the contract, and to estimate the basis risk associated with the contract.

Simple example 

### Block bootstrap

The example assumed the data generated by an independently and identically distributed or i.i.d.  process. In other words it assumes each sample is an independent draw from the same distribution.  This is not the case for solar power output, which is autocorrelated.

There are however modifications to the basic bootstrapping procedure
One widely applied technique for time series is the block bootstrap.

we want our resampling procedure to capture this very sequential information.

"To perform a block bootstrap, you set some block size ℓ
, and split your data into contiguous blocks xi,xi+1,…,xi+l−1
. You then perform resampling with replacement of the blocks of data in order to generate a bootstrapped sample, with a uniform distribution over all blocks. Here too, there are various nuances, depending on whether you allow your initial blocks to overlap or not, how you concatenate them, etc. One major point to observe about this class of methods is that while the blocks are contiguous, resampling effectively shuffles the order of the blocks."

From https://stats.stackexchange.com/questions/449613/when-can-you-apply-the-bootstrap-to-time-series-models

block bootstrap samples groups of data points (blocks) together, which helps maintain the natural correlations present in weather time series data like daily temperature fluctuations or seasonal trends

Moving block bootstrap:
This method slides the block window across the time series, allowing for more flexibility in capturing different time scales. 
Non-overlapping block bootstrap:
Blocks are chosen without overlap, which can be useful when the data exhibits strong seasonal patterns

## useful

https://medium.com/@jcatankard_76170/block-bootstrapping-with-time-series-and-spatial-data-bd7d7830681e

