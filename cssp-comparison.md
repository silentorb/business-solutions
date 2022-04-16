# Benefits of CSSP over Perpetuals

## Positions Can Be Open Without Counter-positions or Liquidity

* Perpetuals are over-dependent on counter-positions, where positions need counter-positions and/or pre-existing liquidation before they can be opened
* CSSP positions have a minimal dependence on counter-positions—they only need counter-positions to compete
* CSSP positions can be opened and active without any counter positions or liquidity provision
* A CSSP is like an automated pick-up basketball game where a single player can arrive and start shooting hoops and once opponents arrive the gameplay seamlessly becomes competitive
  * Likewise, when opponents leave the game the last remaining player can seamlessly switch to shooting hoops solo
* Because of minimal counter-position dependence, sudden drastic CSSP counter-position closings do not cause instability like it can for perpetuals

## Automatic Reinvestment

* Current perpetuals platforms lack automatic reinvestment (exponential profit)
* In order to reinvest perpetual profit, a user must close a position in order to realize the profit and then open a new position with the realized profit
* CSSP positions are periodically realized, and by setting the position's maximum collateral, a user can determine how much profit is transferred to the user's wallet and how much profit is reinvested into the position's collateral

## No Leverage

* CSSP positions have direct PnL scaling, but that scaling does not involve borrowed assets
* CSSP positions effectively have the benefits of leverage with far less risk of bankruptcy
* CSSP position PnL is periodically resolved and at a relatively high frequency, making position bankruptcy unlikely
* If a CSSP position does become bankrupt, that case is not as detremental as it is for perpetuals
  * Since there are no virtual markets to balance, the worst-case-scenario for CSSP position bankruptcy is winning positions make smaller profits
    * This behavior is similar to auto-deleveraging but in a more fair and natural manner

## No Funding Payments

* In order to synchronize the mark price with the index price, winning positions periodically pay losing positions
* The less mentioned secondary purpose of funding payments is to incentive less attractive counter-positions
* A CSSP does not have a mark price to synchronize
* A CSSP has minimal dependence on counter-positions

## No Auto-Deleveraging

* Due to leveraging and the rigid design of perpetual position state, perpetual platforms need secondary systems that pay debts of bankrupt positions with funds from profitable positions
* Since a CSSP does not need leveraging, has more dynamic positions, and is continually realized, it does not need auto-deleveraging
* The essence of auto-deleveraging inherently happens in a CSSP and in a more fair and balanced manner
  * If a market dramatically shifts so that some positions lose everything, the winning positions don't need to be directly tapped for funds—they will simply have a smaller counter-pool and smaller profits

## Automatic Risk Adjustment

* By default, perpetual positions have fixed risk
* Perpetual platforms can add automatic risk adjustment but it is an added complication and as much an art as it is a science
* Since CSSP positions are dynamic and iterative, they inherently have automatic risk adjustment
  * As a position earns funds, its maximum "bet" increases
  * As a position loses funds, its maximum "bet" decreases

## Simpler Model

* No virtual asset trading
  * CSSPs can support position trading
* No need for market orders
* Less need for stop loss orders
  * Each CSSP position effectively has a stop loss order set at a price derived from the position's collateral
  * It still would be useful to allow CSSP users to specify an additional stop loss price that is different from the inherent stop loss price
* No syncing of two different prices
* No order matching, funding payments, leveraging, liquidity provision, or auto-deleveraging

## Larger Customer Base

* Due to its simpler nature, a CSSP is more accessible to users that would be turned away by the esoteric complexity of perpetuals