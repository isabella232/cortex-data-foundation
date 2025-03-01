## March 2022 - Release 2.1

### Data Foundation

This release brings the following changes Cortex Data Foundation.

**Features & Improvements:**

* Templatized deployment and SQL Flavour for views, `setting.yaml `file and test harness to deploy either ECC or S4:
    1. New split source of data with `MANDT = 050` for ECC and `MANDT = 100` for S/4. These will populate automatically based on the `_SQL_FLAVOUR` parameter in the build (default to 'ECC')
    2. Original dataset from ECC and S/4 mixed still present in the source bucket (`MANDT = 800`)
    3. Views `FixedAssets`, `SDStatus_Items`, `GLDocumentsHdr`, `BSID` and `BSAD` implementations, `InvoiceDocuments_flow`,`DeliveriesStatus_PerSalesOrg`, `SalesFulfillment_perOrder`, `SalesFulfillment`, `UoMUsage_SAMPLE` will be adapted to S/4 with source tables in the next minor release.
* Additional views for ECC and S/4 with Accounts Receivables and On-Time/In-Full analysis:
    -  Billing
    - OrderToCash
    - AccountingDocumentsReceivables
* Additional helper functions for currency conversion
* Additional function for Due Date calculation for discounts
* New helper function for Fiscal period
* Renamed CDC parameters to match Reporting and ML Models.
* SAP Reporting now has a SQL Flavour switch to correctly deploy S4 or ECC based on the `_SQL_FLAVOUR` parameter. Possible valus are `ECC` or `S4`
* Enhanced reporting and calculation logics in:
    - `Deliveries`
    - `PurchaseDocuments`
    - `SalesOrders`
* Reduced CDC deployment copy step by 80% implementing multithreaded copy.
* Optimized overall build time when Cloud Build has the container cached, improving overall deployment time by 50%


**Bug Fixes:**
* Fix for Runtime views not generated from `setting.yaml`
* Deployment dependencies causing errors on view creation


**Known issues:**
-   Hierarchy flattener not configured to find sets in new test data 
-   `.INCLUDE` in some tables in some versions of S/4 is known to be marked as key, which results in invalid SQLs in DAG generation. Recommendation as of now is to clear the _keyflag_ field in the offending `.INCLUDE` entry in the replicated `DD03L` table in BigQuery when generating the DAG. A better solution will be provided in a future release.

### Also check out
- **New!** Cortex Application Layer: a [Google Marketplace](https://console.cloud.google.com/marketplace/product/cortex-public/cloud-cortex-application-layer) offering showcasing how to consume the Cortex Data Foundation in your own cloud-native applications!
    - Best practices
    - Access patterns
    - Full working example with sample code
- **New!** [Looker Blocks](https://github.com/looker-open-source/block-cortex-sap) cool dashboards such as:
    - Orders Fulfillment
    - Order Snapshot (efficiency of Orders vs Deliveries)
    - Order Details
    - Sales Performance - Review the sales performance of Products, Division, Sales organization and Distribution channel.
    - Billing and Pricing
