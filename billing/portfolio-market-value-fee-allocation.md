# Portfolio Market-Value Fee Allocation

Status: Proposal under #10

## Purpose

Investor-level costs that are attributable to more than one issuer should not be assigned solely to whichever issuer first sourced, onboarded, or otherwise appears to "own" the investor account. Where an investor holds securities from multiple issuers, allocable costs should instead be syndicated across those issuers according to the economic weight of their securities in the investor's portfolio.

## Allocation basis

For an allocable cost `C`, each eligible security position `i` receives a portfolio weight based on its market value at the applicable billing snapshot:

`market_value_i = quantity_i × price_i`

`raw_weight_i = market_value_i / Σ market_value_j`

Absent a de minimis exclusion, the issuer associated with position `i` is allocated:

`allocated_cost_i = C × raw_weight_i`

The result is pro-rata syndicated billing. For example, if Company A represents 15% of an investor's eligible securities market value and Company B represents 29%, they are allocated 15% and 29%, respectively, of the shared allocable cost. The remaining cost is allocated across the remaining eligible positions according to their portfolio weights.

## De minimis positions

A billing policy MAY establish a de minimis threshold below which a position is excluded from syndicated billing. The threshold MUST be explicit and applied consistently. It may be expressed as a minimum portfolio weight and, where appropriate, a minimum allocable charge.

After de minimis positions are excluded, the remaining positions are re-normalized so that their adjusted weights total 100%:

`adjusted_weight_i = market_value_i / Σ market_value_k`

where `k` includes only positions that remain billable after the de minimis rule is applied.

The final allocation is then:

`allocated_cost_i = C × adjusted_weight_i`

This prevents trivial holdings from generating economically immaterial inter-agent or inter-issuer billing while preserving pro-rata allocation among material holdings.

## Billing snapshot and valuation

All positions used for one allocation MUST be valued using the same billing snapshot. The valuation source and treatment of securities without a readily available market price MUST be deterministic and documented by the applicable billing policy so that an issuer's allocation cannot depend on which agent happens to perform the calculation.

Only securities included in the applicable syndicated-billing scope participate in the denominator. Cash and other non-security assets are excluded unless a separate billing policy expressly includes them.

## Rounding

Allocations MUST be rounded to the smallest billable unit of the settlement currency. Any rounding remainder SHOULD be assigned deterministically, such as to the position with the largest unrounded allocation, so that the sum of issuer allocations equals the original allocable cost exactly.

## Inter-agent objective

The same calculation should be reproducible by any participating transfer agent from the agreed portfolio snapshot, valuation inputs, billing scope, and de minimis parameters. This allows an investor to hold TAD3 assets administered by different agents without making one agent or one issuer the artificial owner of investor-level shared costs.
