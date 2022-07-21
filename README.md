# vertx-mysql-iam-auth-bug

Prerequisites:

* create **RDS with IAM auth enabled**: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.Enabling.html
* create **database user** for IAM auth: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.DBAccounts.html
* create **IAM user** with AWS access key
* configure aws cli access using this IAM user access key: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html
* configure IAM policy for this IAM user allowing to connect to RDS instance as db user using IAM auth: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.IAMPolicy.html
* check that you can generate RDS auth token and connect using MySQL CLI: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.Connecting.AWSCLI.html

To reproduce (replace RDS endpoint):

```bash
hengroen:~/g/a/vertx-mysql-iam-auth-bug$ TOKEN=$(aws rds generate-db-auth-token \
   --hostname iam-auth-test.cluster-XXXXXXX.eu-west-1.rds.amazonaws.com \
   --port 3306 \
   --username dbuser)

hengroen:~/g/a/vertx-mysql-iam-auth-bug$ DB_HOST=iam-auth-test.cluster-XXXXXXX.eu-west-1.rds.amazonaws.com DB_USER=dbuser DB_NAME=information_schema DB_PASS="${TOKEN}" gradle -q run
URI: mysql://dbuser@iam-auth-test.cluster-XXXXXXX.eu-west-1.rds.amazonaws.com/information_schema
============ Try with MySQL connector ============
1
============ Try with Vertx ============
Access denied for user 'dbuser'@'10.160.3.126' (using password: YES)
```
