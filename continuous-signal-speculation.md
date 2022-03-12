# Continuous Signal Speculation

* Copyright &copy; Christopher W. Johnson and contributors.  All rights reserved.
* Licensed under the MIT license

* See LICENSE file in the project root for details

## Overview

* This document is a white paper describing a type of financial software platform for market speculation
* The working title for this type of platform is a Continous Signal Speculation Platform, or CSSP for short
* The platform described in this document is intended to be implemented using decentralized blockchain technologies, but nothing in its design inherently prevents implementation using a purely centralized solution

## The Problem

### Overview

* The overarching problem a CSSP is intended to solve can be divided into two sub-problems, a general problem and a specific problem
* The general problem may be solved in a variety of ways, but this document is focusing on a particular subset of solutions for that general problem
* That subset of solutions faces its own primary problem, which is summarized in the Funding section

### The General Problem

* The future is uncertain
* Markets can experience varying degrees of instability
* Investors need to make predictions about future market states
* Speculative financial platforms address these problems by providing two primary functions:
  * Assisting people in making, managing, and applying predictions and integrating those predictions with other predictions (collaborative speculation)
  * Providing shock absorbers to markets and buffering investors from market instability

### Funding

* The primary problem facing speculative financial involves funding and can be divided into several funding-related sub-problems:
  * If a user makes a successful prediction, that user should make a profit
  * The funds for that profit need to come from somewhere, preferrably users who made unsuccessful predictions
  * For decentralized blockchain solutions, debt collection is not an option so any funding needs to be provided up front and readily available

## Betting

* To further define the problems and lay the foundation for the solutions, the problem domain will be defined in terms of betting
* At its heart, a speculative financial platform is a betting platform
  * Users bet whether a value will increase or decrease over time
* A betting platform has winners and losers

## The Solution

### Continous Signals

* Unlike most traditional betting platforms, the subject bet upon within financial markets is not a discrete outcome, but a continuous signal
* The best solution for betting on continuous signals is a system that uses continuous bets
  * In general, the best solutions are solutions that act as a mold for the problem they are solving, mirroring and masking the problem
  * The greater the discrepancy between the problem and the solution, the less effective the solution will be
* In a speculative financial platform, users bet that a value will either increase or decrease over time
* In traditional finance, these bets are called positions
  * A position betting on a positive vector is a long position
  * A position betting on a negative vector is short position
  * This document will use the terms *positive* and *negative* in place of *long* and *short* because the latter terms have associations and context that does not apply to a CSSP
  * This document will use the term *position* to refer to outstanding bets because the financial definition of *position* does not contain asscociations foreign to CSSP and is a useful term to carry over from traditional financial platforms
* Since it is measuring a continuous signal, a CSSP periodically samples a signal over time to produce discrete scalar values
* For each sample, the preceding sample is subtracted from that sample to produce a single-dimensional vector
* Depending on the direction of each sample's vector, one set of users win the bet for that sample and one set of users lose the bet for that sample
  * If a vector is zero, there are no winners or losers
  * If a vector is positive, positive positions win and negative positions lose
  * If a vector is negative, negative positions win and positive positions lose

### Positions

* A CSSP user can open any number of positions
* Each position belongs to a single signal
* The owner of a position can close a position at any time, though there is less need for closing CSSP positions than there is in traditional speculative financial platforms
  * CSSP positions only really needs to be closed when they are no longer needed and have become clutter

### Balances

* Each CSSP user has an account balance
* The account balance is a numeric value
* The account balance represents funds that user owns which are not tied to a position as collateral
* Users can deposit and withdraw funds into their account balance
* Users can move transfer funds from their account balance and into positions as collateral
* In certain cases described below, the platform will transfer winnings to a users account balance

### Collateral

* In a speculative financial system, losers should pay winners
* Debt collection is not an option for decentralized blockchain solutions so the funds used to pay winners need to be provided up front

* In a CSSP a losing party can never lose more than its collateral

#### Short Collateral

* For traditional markets, The value of an asset cannot go below zero
* Because of that lower limit, a positive position has a finite limit to its potential loss
* A negative position has no inherent limit to its potential loss
* Positive collateral is finite and quantifiable, making its collateral management simpler
* Negative collateral is infinite and unquantifiable, making its collateral management more complex
* While negative positions do not have inherent loss limits, their effective losses cannot exceed their collateral
* A CSSP contains mechanisms to minimize excessive loss, further explained in later sections

#### Maximum and Actual Collateral

* A CSSP position has two collateral properties:
  * The maximum collateral which is set by the position owner
  * The actual collateral which is dynamically managed by the platform software
    * When this document refers to just *collateral* it is referring to *actual collateral*
* The actual collateral can never exceed the maximum collateral, but can be less than the maximum collateral
* The owner of a position can adjust that position's maximum collateral at any time
* If the maximum collateral is reduce below the actual collateral, the platform will transfer any actual collateral over the maximum to the user's account balance
* If a user sets the maximum collateral of a position to zero then that position is effectively closed until the target collateral is increased
* When the actual collateral of a position is zero then that position is considered "hibernating"

#### Negative Signals

* A CSSP can also be used to speculate on signals that can contain negative values
* In such cases positive positions would no longer have a lower limit and would have the same inherently limitless potential loss that negative positions have

### Vertical Scaling

* A CSSP position has two multiplier properties:
  * The target multiplier which is set by the position owner
  * The effective multiplier which is dynamically set by the platform software and can never exceed the target multiplier
* Both multipliers are scalar values
* Both values can be anywhere within a range of zero to as large a number as the platform can practically support
* The winnings and losses of a position are multiplied by its effective multiplier
* Unlike traditional speculative financial platforms, CSSP multipliers do not affect risk
  * For a CSSP, only collateral determines the risk of a position
    * Collateral directly defines the max loss of a position

### Update Process

#### Overview

* A CSSP periodically executes an update process that performs the following operations:
  * Gathers fresh samples for its various signals
  * Adjusts the effective multiplier for each position
  * Transfers payments from losers to winners

#### Sampling

* A CSSP can support any number of distinct signals
* In most cases the signals will represent external market prices for particular asset types
* However the signals can represent anything that can be expressed in a waveform and can come from any source as long as users agree upon the authority of that source
* Sampling can happen at a resolution greater than update intervals, in which case new positions can use custom vectors where the previous sample is the closest sample to the position creation time instead of the last interval update sample

#### Multiplier Adjustment

* A CSSP needs to estimate the maximum vector magnitude for each signal
  * This can be either a fixed number or dynamically calculated by more elaborate systems that analyze historical trends
* Next, the maximum multiplier for each position is calculated
  * `max_multiplier = collateral / max_magnitude`
    * Either safe division needs to be applied here or a guarantee that `max_magnitude` is not zero
* Finally the effective multiplier is calculated and updated for each position
  * `new_position_scalar = if (is_new_position) `
    *  `(current_timestamp - position_creation_timestamp) / time_since_last_update`
    * `else 1`
  * `effective_multiplier = min(max_multiplier, target_multiplier) * new_position_scalar`
* A hibernating position (a position with no collateral) will always have an effective multiplier of zero

#### Payment

##### Losses

* Raw losses are calculated for each position
* The raw loss of a losing position is the absolute magnitude of its signal's sample vector multiplied by the position's effective multiplier
* Next the adjusted loss is calculated, which is either a position's raw loss or its collateral, whichever is less

##### Winnings

* Next raw winnings are calculated for each position
* The raw winnings of a winning position is the absolute magnitude of its signal's sample vector multiplied by the position's effective multiplier

##### Resolution

* The total adjusted losses and total raw winnings are summed up for each signal
* Next actual winnings are calculated and applied for each signal:
  * If the total adjusted losses are equal to or greater than the total raw winnings, then actual winnings are the same as raw winnings and cleanly distributed to each winner
  * Otherwise, rationing is applied, explained in the Rationing section below
  * If the total adjusted losses are greater than the total actual winnings then the actual losses are reduced, explained in the Payout section below

##### Rationing

* If a signal's total adjusted losses are less than its total winnings, the limited funds are distributed across the winning positions, weighted by each position's collateral
  * Positions with more collateral will receive a higher percentage of their raw winnings
* The rationing calculations are:
  * **TODO:** This math needs more work, particularly needing to factor in the ratio of total adjusted losses to total winnings
  * `total_collateral` = sum of collateral for all winning positions of the signal
  * For each position of that signal:
    * `actual_winnings = raw_winnings * collateral / total_collateral`

##### Payout

* The payout process effectively transfers funds from losing positions collateral to either winning position collateral or user account balances
* For each signal, total losses cannot exceed total winnings
* Any excess losses remain in the collateral of losing positions
* The first step in payout is to calculate the actual losses for each losing position
* **TODO:** Add calculations for the actual losses so they scale down when there are a lower amount of winnings (it will probably be similar to the rationing equation)
* When a position wins, its winnings are first used to fill its collateral to the the position's maximum collateral value, then any excess winnings are transferred to the user's account balance
  * `collateral_increase = min(max_collateral - collateral, winnings)`
  * `collateral = collateral + collateral_increase`
  * `account_balance = account_balance + winnings - collateral_increase`

### Inherent Behavior

* A CSSP position can have its collateral completely depleted, enter hibernation, and then have the market turn and earn back all of its collateral and make a profit
* Unlike traditional financial speculation platforms and betting platforms, a CSSP does not need to explicitly match orders or bets with counter-parties
  * A CSSP user can safely open a position without any counter-positions
    * As long as their are no counter-positions, a position will effectively never experience any wins or losses
    * Once a counter-position is opened, wins and losses will take effect
* Since a CSSP is not based on trading or orders it is potentially less susceptible to slippage than traditional financial speculation platforms
  * A CSSP can still experience slippage depending on latency and signal sampling frequency
* Unlike traditional financial speculation platforms, CSSP positions do not need stop loss orders
  * The way CSSPs handle position collateral effectively acts as a stop loss order
  * A feature similar to take profit orders would still be useful for CCSPs

