## Manual Rollup Tool

This tool allows you to manually rollup the supplied resolutions from the finer ones. This assumes the raw data is existing to start with. If the raw data is not existing
or has ttl'd out, run the Cloud File backfiller tool to first backfill 5m rollups and then calculate the rollups belonging to higher resolutions, by running this tool.

An example configuration to be supplied while running the tool lies under RollupToolConfig. The most noteworthy settings are of the granularities to be rolled up.
For example: setting `METRICS_5M_ENABLED` to true will rollup 5m resolution from raw metrics, belonging to `SHARDS_TO_MANUALLY_ROLLUP` supplied shards and so on. Note that
enabling a granularity assumes that the rollups/raw data belonging to a finer granularity is present because it calculates the rollups belonging to enabled granularity from
a finer one. In other words, if multiple granularities are enabled, they will be rolled up one by one in increasing order of granularity. This also implies that a intermediate
granularity cannot be skipped unless rollups are already existing for it.

Example run of manual rollup tool:

java -Dblueflood.config=file:/home/chinmay/bf-RollupTool.conf -Dlog4j.configuration=file:/home/chinmay/log4jRollupTool.properties -Xms1G -Xmx2G -cp blueflood-rax-all-1.0.0-SNAPSHOT-jar-with-dependencies.jar com.rackspacecloud.blueflood.service.RunRollupTool

The following metrics emitted from the tool are useful:

1. com.rackspacecloud.blueflood.io.ManualRollup.ReRoll-Timer.mean : average time spent in calculating to rollups.
2. com.rackspacecloud.blueflood.io.ManualRollup.Time-taken-to-rollup-per-shard.count will give you the time spent on rolling up each shard. Also, useful in getting a count of shards as they are rolled.


The tool internally uses the astyanax writer, so there are other metrics especially the ones related to queue sizes, which might be helpful as well.

