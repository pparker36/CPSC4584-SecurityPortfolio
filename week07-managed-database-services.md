# Week 7: Managed Database Services

**HarborTech Cloud Operations Playbook**  
**Ticket:** TKT-2026-0007  
**Topic:** Database Operations Assessment — Keep, Manage, or Migrate?

## HarborTech Ticket Summary

HarborTech received a request from a café that uses a MySQL database running on Amazon EC2. The database is working, but the café's team spends a lot of time doing updates, patching, backups, and other maintenance. My job was to look at the workload and figure out whether they should keep managing the database on EC2 or consider moving to a managed database service. I mainly looked at Amazon RDS, Aurora, and DynamoDB. This was a recommendation task, not permission to move the production database.

## Client Impact

If the database goes down, customers and employees might not be able to use the café's application or complete transactions. A slow database could also make the application frustrating to use. Moving to the wrong service could create extra costs or cause problems with queries and connections. A migration without enough planning could lead to downtime or missing data, so testing and having a recovery plan would be important.

## AWS Services Involved

- **Amazon EC2:** Where the café currently runs its MySQL database and manages the server.
- **Amazon RDS:** A managed relational database service that could reduce database maintenance work.
- **Amazon Aurora:** A MySQL-compatible relational database option worth comparing when there are stronger performance or availability needs.
- **Amazon DynamoDB:** A managed NoSQL database that could fit separate customer preference records accessed mostly by a key.
- **Relational and NoSQL databases:** Two different ways of organizing and accessing information.
- **Multi-AZ:** An availability option that helps a database recover from certain infrastructure failures.
- **AWS Database Migration Service (DMS):** A service that can help move or replicate data between databases.
- **Schema conversion:** Changing database structures or code when the source and target engines are not fully compatible.
- **Security groups and database connectivity:** Network rules and connection settings that control whether the application can reach a database.

## Virtualization Connection

With the database on EC2, AWS provides the virtual machine, but the café still has to manage the guest operating system and MySQL software. With RDS or Aurora, AWS takes care of more of the underlying infrastructure and routine database operations. That means the café can spend less time maintaining the server, but it still has to manage its data, permissions, application connections, and database design. A managed service doesn't mean AWS handles everything.

## Evidence Reviewed

The ticket says the café already uses MySQL with structured tables, SQL queries, and transactions. This makes a relational database a good starting point. The database is available, but patching, backups, and recovery planning take staff time. I would still need to find out the database size, transaction volume, traffic patterns, backup and recovery goals, allowed downtime, budget, and any MySQL features the application depends on. I would also need to check the connection settings, security group rules, and whether the application would work with a new database endpoint. Before migration, we would need to know whether any schema or application changes are required.

The ticket also includes a different example workload: separate customer preference records looked up mostly by a known key, without relational joins, and with unpredictable traffic. That workload sounds like a better fit for DynamoDB than the café's existing transactional MySQL database.

## AWS Academy Activity: Actions and Results

I used AWS CloudShell in the **us-east-1** region. Here is what I actually checked. These are resource checks and a blocked creation attempt, **not evidence of a completed migration**.

### Action 1 — Attempt to Create an RDS Database

**Console action:** I opened Amazon RDS in the AWS Management Console and tried to create a database for testing.

**Observed result:** AWS denied the request because my lab account was not authorized to perform `rds:CreateDBInstance`.

**What this shows:** My AWS Academy account had permission limits. I could review the service, but I couldn't create that RDS instance in the lab.

### Action 2 — Check RDS Instances

**Command:**

```bash
aws rds describe-db-instances --region us-east-1 --query 'DBInstances[].{Name:DBInstanceIdentifier,Engine:Engine,Status:DBInstanceStatus}' --output table
```

**Observed result:** The command returned to the prompt without an error or displayed instance rows.

**What this shows:** No RDS instances were returned by that check in `us-east-1`. It doesn't show a successful database creation or migration.

### Action 3 — Check DynamoDB Tables

**Command:**

```bash
aws dynamodb list-tables --region us-east-1 --output table
```

**Observed result:** The command displayed a `ListTables` heading with no table names listed.

**What this shows:** The command worked, but no DynamoDB tables were returned in `us-east-1`.

### Final Verification

I reviewed the RDS and DynamoDB listing results and the earlier RDS permission error. I did **not** verify a working new database, application connection, migrated data, or a completed database migration. The permission restriction is still a limitation of my lab results.

## Operational Analysis

The café's main issue isn't that MySQL stopped working. The problem is how much effort goes into maintaining it on EC2. Since the application already uses structured data, SQL, and transactions, I think it makes sense to stay with a relational database. Amazon RDS for MySQL looks like the first option to evaluate because it can reduce some of the work around backups and maintenance. Aurora could also work, but I would need evidence that the café actually needs its particular performance or availability features. DynamoDB makes more sense for the separate key-based customer preference example, not automatically for the café's existing MySQL workload.

## Recommendation

I would recommend **evaluating Amazon RDS for MySQL first**, instead of immediately migrating. It keeps a relational database model and lets AWS handle more of the infrastructure and routine operations. The café would still be responsible for its data, access permissions, application integration, and choosing the right configuration. Before deciding, I would compare costs and availability options, check MySQL compatibility, confirm backup and recovery requirements, and test connectivity and migration in a non-production environment. I'd also have a rollback plan ready. I wouldn't assume Aurora or DynamoDB is better just because they're different AWS services.

## Escalation Notes

The RDS creation attempt was blocked by the AWS Academy account's `rds:CreateDBInstance` permission. That would need help from the instructor or lab administrator if the activity requires creating an instance. For a real client, the supervisor and client would also need to approve the migration approach, any production connectivity or security group changes, downtime, availability configuration, and the final cutover. I did not make or approve any production changes.

## Lessons Learned

This week helped me understand that choosing a database isn't just about picking the most advanced service. You have to look at how the application stores and uses data, what maintenance is taking up time, and how much downtime the business can handle. I also learned that managed databases make some operations easier, but the customer still has responsibilities. Finally, I learned to report what AWS actually showed me. An empty command result or a permission error is still useful evidence, but it isn't proof that a database was created or migrated.

## Professional Vocabulary

- **Relational database:** A database that stores data in tables and lets you query relationships using SQL.
- **NoSQL:** A type of database design that doesn't depend on the traditional relational table-and-join model.
- **Amazon RDS:** An AWS service that manages many routine tasks for relational databases.
- **Amazon Aurora:** An AWS-managed relational database engine compatible with MySQL or PostgreSQL, depending on the version selected.
- **Amazon DynamoDB:** A managed NoSQL database that works well for certain key-based access patterns and changing request levels.
- **Multi-AZ:** Using more than one Availability Zone to improve resilience if infrastructure fails.
- **Database Migration Service (DMS):** An AWS tool for moving or replicating data from one database to another.
- **Schema conversion:** Updating database structure or database code when moving between different engines or designs.
- **Managed service:** A service where AWS handles some of the setup, infrastructure, or maintenance for you.
- **Workload fit:** Choosing a service based on what the application actually needs instead of just what sounds popular.

---

**Status:** Assessment and lab checks documented. No successful database creation or production migration was verified.
