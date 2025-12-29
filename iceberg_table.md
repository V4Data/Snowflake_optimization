## Iceberg Table Optimizations

### 1. Schema Evolution Problem

Problem: Traditional tables require full data rewrites for schema changes (add/drop columns), causing hours of downtime and massive compute costs.

-- TRADITIONAL TABLES: EXPENSIVE REWRITES
ALTER TABLE orders ADD COLUMN customer_segment STRING;
-- Full table rewrite: 10TB = 5+ hours + high costs

Solution: Iceberg schema evolution - add/drop/rename columns WITHOUT rewriting data.

-- ICEBERG: INSTANT SCHEMA CHANGES
ALTER TABLE iceberg_orders ADD COLUMN customer_segment STRING;
-- Metadata only: <1 second, zero data movement!

Results: Schema changes: Seconds instead of hours, Zero data rewrite costs, No downtime

### 2. Metadata Refresh Problem

Problem: Iceberg tables with external catalogs show stale data due to unsynced metadata. Queries miss recent inserts/updates.

-- STALE: No metadata refresh
SELECT * FROM iceberg_table WHERE date = '2025-12-29';  -- Misses new data!

Solution: Enable AUTO_REFRESH and manual sync commands.

-- Auto-refresh table
CREATE ICEBERG TABLE stock_analysis (...) AUTO_REFRESH = TRUE;

-- Manual refresh
CALL SYSTEM$ICEBERG_REFRESH_TABLE('db.schema.table');

Results: Real-time data visibility, 100% query accuracy, Metadata sync < 10s
