## Set up PostgreSQL Slow Query Alert

You can enable a slow query alert to monitor all SQL queries for a
PostgreSQL RDS instance and send out a notification once any query runs
for more than a preset number of seconds. This feature is useful for
monitoring a database for degraded performance before any impact is felt
by the end user. The steps below will show how to enable a slow query
logs and deliver them to [AWS SNS](https://aws.amazon.com/sns/) or
[Sentry](https://sentry.io/welcome/). All resources required to setup
slow query notifications will be made using Terraform.

1.  Enable query logging for PostgreSQL.
    The first step to setting up slow query logs is to enable query
    logging on the PostgreSQL RDS instance. You can enable query logs on
    RDS by updating either or both of these RDS parameter;

    - [log\_statement](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-STATEMENT):
      Modify this parameter to determine the type of logs that should
      be logged regardless of execution time. The default is **none**.
      Possible options are;

      1.  **all**: Capture all logs.
      2.  **ddl**: Capture all data definition language (DDL)
          statements such as *CREATE*, *ALTER*, and DROP.
      3.  **mod**: Capture all DDL and data modification language
          (DML) statements such as *INSERT*, *UPDATE*, and *DELETE*.

    - [log\_min\_duration\_statement](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-MIN-DURATION-STATEMENT):
      Modify this parameter to set a threshold in milliseconds for
      queries to be logged. This allows you to log all queries that
      take longer than the set parameter value.

      The terraform code snippet below can be used to create a
      PostgreSQL database parameter group to export all SQL queries
      that run for over 5 seconds.

      ```
      resource "aws_db_parameter_group" "postgres_databae_parameter" {
        name   = "postgres-database-parameter"
        family = "postgres14"

        parameter {
          name  = "log_min_duration_statement"
          value = "5000"
        }
      }
      ```

2.  Publishing slow query logs to Cloudwatch Logs:
    Once query logging has been enabled in the RDS postgres instance,
    the next step is to publish the logs data to Cloudwatch logs. To do
    this, you should set `enabled_cloudwatch_logs_exports` option on RDS
    to the preferred RDS log type that should be exported. Available
    options for PostgreSQL are `postgresql` and `upgrade`. For slow
    query logs, we will set this parameter to `postgresql`.
    Note:, The logs from the RDS instance will be exported to a
    Cloudwatch log group with this naming convention -
    `/aws/rds/instance/{my_instance}/postgresql`, where `my_instance` is
    the name of the RDS instance. A Cloudwatch log group with this name
    will be created automatically if doesn’t already exist.
    If you are using thoughtbot’s
    [terraform-aws-database](https://github.com/thoughtbot/terraform-aws-databases/tree/main/rds-postgres/primary-instance)
    terraform module, you can enable this option using the terraform
    code snippet below along with other necessary inputs;

    ```
    module "postgres" {
      source = "github.com/thoughtbot/terraform-aws-databases//rds-postgres/primary-instance?ref=v0.4.0"

      ...
      enabled_cloudwatch_logs_exports = ["postgresql"]
      ...
    }
    ```

3.  Create a Cloudwatch log group subscription to AWS SNS
    You can create a Log group subscription to send every message
    received by the Cloudwatch log group to an AWS SNS topic. For this
    step, we will be using a [Cloudwatch Logs
    extract](https://github.com/thoughtbot/flightdeck/tree/main/aws/cloudwatch-log-extract)
    terraform module. You can review the terraform code snippet below
    for a sample implementation of the module.
    ***Note***: A destination SNS topic should be created and passed
    into this terraform module.

    ```
    module "cloudwatch_log_extract" {
      source = "github.com/thoughtbot/flightdeck//aws/cloudwatch-log-extract?ref=v0.10.1"

      # Enter the Source CloudWatch log group to subscribe to for log messages.
      source_cloudwatch_log_group = "/aws/rds/instance/sandbox-database/postgresql"

      # Enter a log group filter pattern to pick which logs get sent to the lambda endpoint
      log_group_filter_pattern = "duration"

      # Enter a log message filter expression to pick out specific details to be sent to the destination SNS topic.
      # You may provide a regex pattern with capture groups, and enter the label for each capture group.
      log_message_filter = {
        regex_pattern = ".* duration: ([0-9.]+) ms .* content: (.*)"
        capture_group = {
          1 = "duration",
          2 = "message"
        }
      }

      # Enter any message attribute to be added to the SNS message.
      message_attributes = {
        database = "sandbox-database"
      }

      # Enter the destination SNS topic
      destination_sns_topic_arn = "arn:aws:sns:us-east-1:123456789:sandbox-database-sns-topic"
    }
    ```

4.  Send slow query messages from AWS SNS to Sentry
    Lastly, you can now push the messages from SNS to Sentry. You can do
    this by creating an SNS subscription to the destination SNS topic
    used in the previous step. Once this is done, any message publised
    to SNS will be forwarded to Sentry. For this step, we will be using
    an [SNS to Sentry
    integration](https://github.com/thoughtbot/aws-sns-sentry-delivery)
    terraform module. You can review the terraform code snippet below
    for a sample implementation of the module.

    ```
    module "sns_sentry_delivery" {
      source = "github.com:thoughtbot/aws-sns-sentry-delivery?ref=v0.1.1"

      name = "sandbox-database-messages"

      # Name of the AWS Secretmanager resource contaiing the sentry DSN credentials
      sentry_secret_name = sandbox-sentry-secret

      # Enter the source SNS topic
      source_sns_topic_arn = "arn:aws:sns:us-east-1:123456789:sandbox-database-sns-topic"
    }
    ```
