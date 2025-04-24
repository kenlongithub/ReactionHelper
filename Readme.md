# Reaction Helper
<!-- TOC -->
* [Reaction Helper](#reaction-helper)
    * [Specs](#specs)
<!-- TOC -->
### Specs
The intent is to match the functionality of Twine T2, while not being hampered by the limitations of Google Sheets (or spreadsheets in general).
I may very well be overreaching here, but inspection of what the ESI can provide (via https://esi.evetech.net) leads me to believe all of these things are possible to implement - ie, the data needed is there.
Theoretically, at least.

* Ensure visibility into success/failure of ESI endpoint, with listing of last time of successful update
* Load character data from ESI
* Load market data for $REGION from ESI
* Inventory Warehouse data
  * Tracked in $REGION + Jita, not dealing with multiple regions yet. 
  * Get purchase price of goods, maintain average 
  * Built goods are tracked at their total build price
    * `context_id_type` `industry_job_id` can be used to match wallet journal with industry for independent tracking of facility tax and job cost.
  * Track average cost over time to allow querying what the warehouse cost was at a past date
    * I will regret this, as it will require a db.
  * Pre-existing stock tracked at Sell price in selected $REGION/Jita at initialization time - this will also be used for assets that do not have an order in history to be matched to.
    * order history expires after 1 month/1000 entries, so making best usage of this feature does require running it regularly.
  * Add shipping costs to warehouse average
    * Find courier contracts, check they are completed, if so, grab the `type_id`s and quantities, determining the shipping cost per item, and add to warehouse cost as appropriate
* Market Tracking
  * Set expected stock levels
  * Show profit based on warehouse cost at time market order was placed
  * Track real profit/loss over time
  * Use market history state to handle re-priced/expired orders and update profit/warehouse data at time of expiration/cancellation
* Industry tracking
  * Track jobs needed to meet stock levels + configured overstock
  * Track jobs needed for inputs - recommend purchase/build depending on profitability
* Industrial planning
  * Calculate profitability of reactions
  * Set purchase of inputs from $REGION/Jita, or mix.
  * Calc both build from market purchase, and build from warehouse + market 
    * Add option to limit to only warehouse and show what can be built from on-hand 
  