# Compute Returns

`Beangrow` lets you easily calculate investment returns by fetching data right from your beancount file.   
See this document for details:
https://docs.google.com/document/d/1nPsMIunLnDvdsg6TSsd0PZb7jngojNpFlqnaX36WRp8/


### Installation
1. `pip3 install beancount`
2. `git clone git@github.com:beancount/beangrow.git`


### Usage
1. `python beangrow/configure.py ledger.beancount > config` 
2.  examine `config` file, adjust if needed
3. `python beangrow/compute_returns.py ledger.beancount config output`


### Examples
check `beangrow/examples` folder for example of beangrow project


### Trouble shooting
+ commodity declarations are optional in beancount but are required for beangrow, use `beancount.plugins.check_commodity` plugin to enforce them

+ `output/prices/prices.beancount` contains list of price directives that `compute_returns.py` missed in order to its job properly. If you see entries in it then

  + either run `beangrow/download_prices_from_file.py output/prices/prices.beancount` and copy fetched data into your `ledger.beancount`
  + or manually add missing entries to your `ledger.beancount`

  after that rerun `python beangrow/compute_returns.py ledger.beancount config output` for more precise results.
  
+ `output/investments` contains signature and summary files. When your first set up your beangrow project examine them to make sure that they match explanation in [beangrow wiki's](https://docs.google.com/document/d/1nPsMIunLnDvdsg6TSsd0PZb7jngojNpFlqnaX36WRp8/edit) "Handling Transactions using the Signature" section.


### Main Scripts
There are three related scripts in project:

- configure.py: This attempts to automatically infer and generate configuration
  to compute returns from an existing Beancount ledger.

- compute_returns.py: This extracts data for each of the investments defined in
  the configuration and computes the returns and generates output for each
  requested returns report.

- download_prices.py: The compute_returns.py script outputs a list of missing
  (or inadequately dated) price directives to properly do its job as a
  side-product. This script can read that file and fetch those missing dates,
  which you can insert in your ledger and then rerun compute_returns.py for a
  more precise calculation.
