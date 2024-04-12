### RDS Postgres Database Upgrade

This page will summarise the steps to upgrade an RDS Postgres database
using this [AWS guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.PostgreSQL.html#USER_UpgradeDBInstance.PostgreSQL.MajorVersion.Process).
You may review the referenced [AWS guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.PostgreSQL.html#USER_UpgradeDBInstance.PostgreSQL.MajorVersion.Process)
as well for a more comprehensive guide on the AWS database upgrade
process.

1.  Modify the database to use a default parameter group. If a custom
    parameter group is in use by the database, you may get an error
    stating that the current parameter group is not compatible with the
    version you are upgrading to. To resolve this, you should modify the
    parameter group attached to the database to one of the default
    parameter group created by AWS for the current Postgres version. |
    For example, if the database is currently on Postgres12, you may
    modify the parameter group to the `default.postgres12` parameter
    group.
2.  Confirm that the current database instance class is compatible with
    the Postgres version you are upgrading to. You may review the [list
    of supported Database engines for DB instance
    types](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.DBInstanceClass.html#Concepts.DBInstanceClass.Support).
3.  Confirm that there are no open prepared transactions using this
    command before commencing the upgrade. If the upgrade is initiated
    during off-hours period, there should be no open prepared
    transactions.
    `SELECT count(*) FROM pg_catalog.pg_prepared_xacts;`
4.  Initiate a database backup before starting the database upgrade.
5.  Have a database upgrade dry run to test the database upgrade on a
    non-impact environment or a duplicate of the database.
6.  Run the `ANALYZE` operation to refresh the `pg_statistic` table.
    Optimizer statistics aren't transferred during a major version
    upgrade, This operation will regenerate all statistics to avoid
    performance issues. You may add a verbose flag to show the progress
    of the operation.

    ```
    ANALYZE VERBOSE;
    ```

    See [ANALYZE](https://www.postgresql.org/docs/10/sql-analyze.html)
    in the PostgreSQL documentation.

#### Troubleshooting issues with the Postgres database upgrade.

The database upgrade could fail with prechecks procedure due to
incompatible setups or incompatible extensions. To check the cause of
the interruption, you may check the Postgres logs to confirm the cause
of the failure. You may review these documentation on how to retrieve
Postgres logs.

- <https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Procedural.Viewing.html>
- <https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Procedural.Downloading.html>

#### Known database errors

- During a previous database upgrade, I experiened issues with
  incompatible PostGIS postgres extensions. I found the issue by
  reviewin the Postgres logs and I was able to upgrade the postgres
  extension using this guide - <https://aws.amazon.com/premiumsupport/knowledge-center/rds-postgresql-upgrade-postgis/>
