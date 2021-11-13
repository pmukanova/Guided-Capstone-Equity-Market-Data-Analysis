# Guided-Capstone-Equity-Market-Data-Analysis

The goal of this project is to build an end-to-end data pipeline to ingest and process daily stock market data from multiple stock exchanges. The pipeline should maintain the source data in a structured format, organized by date. It also needs to produce analytical results that support business analysis.

### Data Source
The source data used in this project is randomly generated stock exchange data.
->Trades: records that indicate transactions of stock shares between broker-dealers.
->Quotes: records of updates best bid/ask price for a stock symbol on a certain exchange.

### Step 1: Database Table Design
- Implement database tables to host the trade and quote data.
- Since trade and quote data is added on a daily basis with high volume, the design needs to ensure the data growth over time doesn’t impact query performance. Most of the analytical queries operate on data within the same day.

### Step 2: Data Ingestion

- The source data comes in JSON or CSV files.
- Each file is mixed with both trade and quote records. The code needs to identify them by column rec_type: Trade is “T”, Quote is “Q”.
- Exchanges are required to submit all their data before the market closes at 4 pm every day. They might submit data in multiple batches. Your platform should pre-process the data as soon as they arrive.
- The ingestion process needs to drop records that do not follow the schema and partition it as "B" Bad records.

### Step 3: End of Day (EOD) Batch Load
- Loads all the progressively processed data for current day into daily tables for trade and quote.
- The Record Unique Identifier is the combination of columns: trade date, record type, symbol, event time, event sequence number.
- Exchanges may submit the records which are intended to correct errors in previous ones with updated information. Such updated records will have the same Record Unique Identifier, defined above. The process should discard the old record and load only the latest one to the table.
- Job should be scheduled to run at 5pm every day.
