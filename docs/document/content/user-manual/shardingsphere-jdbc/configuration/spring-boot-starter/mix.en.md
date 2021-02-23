+++
title = "Mixed Rules"
weight = 6
+++

## Configuration Item Explanation
```properties
# data source configuration
spring.shardingsphere.datasource.names= primary-ds0,primary-ds1,primary-ds0-replica0,primary-ds1-replica0

spring.shardingsphere.datasource.primary-ds0.url= # Database URL connection
spring.shardingsphere.datasource.primary-ds0.type=  # Database connection pool type name
spring.shardingsphere.datasource.primary-ds0.driver-class-name= # Database driver class name
spring.shardingsphere.datasource.primary-ds0.username= # Database username
spring.shardingsphere.datasource.primary-ds0.password= # Database password
spring.shardingsphere.datasource.primary-ds0.xxx=  # Other properties of database connection pool

spring.shardingsphere.datasource.primary-ds1.url= # Database URL connection
# ...Omit specific configuration.

spring.shardingsphere.datasource.primary-ds0-replica0.url= # Database URL connection
# ...Omit specific configuration.

spring.shardingsphere.datasource.primary-ds1-replica0.url= # Database URL connection
# ...Omit specific configuration.

# Sharding rules configuration
# Databases sharding strategy
spring.shardingsphere.rules.sharding.default-database-strategy.standard.sharding-column=user_id
spring.shardingsphere.rules.sharding.default-database-strategy.standard.sharding-algorithm-name=default-database-strategy-inline
# Binding table rules configuration ,and multiple groups of binding-tables configured with arrays
spring.shardingsphere.rules.sharding.binding-tables[0]=t_user,t_user_detail
spring.shardingsphere.rules.sharding.binding-tables[1]= # Binding table names,multiple table name are separated by commas
spring.shardingsphere.rules.sharding.binding-tables[x]= # Binding table names,multiple table name are separated by commas
# Broadcast table rules configuration
spring.shardingsphere.rules.sharding.broadcast-tables= # Broadcast table names,multiple table name are separated by commas

# Table sharding strategy
# The enumeration value of `ds_$->{0..1}` is the name of the logical data source configured with replica-query
spring.shardingsphere.rules.sharding.tables.t_user.actual-data-nodes=ds_$->{0..1}.t_user_$->{0..1}
spring.shardingsphere.rules.sharding.tables.t_user.table-strategy.standard.sharding-column=user_id
spring.shardingsphere.rules.sharding.tables.t_user.table-strategy.standard.sharding-algorithm-name=user-table-strategy-inline

# Data encrypt configuration
# Table `t_user` is the name of the logical table that uses for data sharding configuration.
spring.shardingsphere.rules.encrypt.tables.t_user.columns.user_name.cipher-column=user_name
spring.shardingsphere.rules.encrypt.tables.t_user.columns.user_name.encryptor-name=name-encryptor
spring.shardingsphere.rules.encrypt.tables.t_user.columns.pwd.cipher-column=pwd
spring.shardingsphere.rules.encrypt.tables.t_user.columns.pwd.encryptor-name=pwd-encryptor

# Data encrypt algorithm configuration
spring.shardingsphere.rules.encrypt.encryptors.name-encryptor.type=AES
spring.shardingsphere.rules.encrypt.encryptors.name-encryptor.props.aes-key-value=123456abc
spring.shardingsphere.rules.encrypt.encryptors.pwd-encryptor.type=AES
spring.shardingsphere.rules.encrypt.encryptors.pwd-encryptor.props.aes-key-value=123456abc

# Key generate strategy configuration
spring.shardingsphere.rules.sharding.tables.t_user.key-generate-strategy.column=user_id
spring.shardingsphere.rules.sharding.tables.t_user.key-generate-strategy.key-generator-name=snowflake

# Sharding algorithm configuration
spring.shardingsphere.rules.sharding.sharding-algorithms.default-database-strategy-inline.type=INLINE
# The enumeration value of `ds_$->{user_id % 2}` is the name of the logical data source configured with replica-query
spring.shardingsphere.rules.sharding.sharding-algorithms.default-database-strategy-inline.algorithm-expression=ds$->{user_id % 2}
spring.shardingsphere.rules.sharding.sharding-algorithms.user-table-strategy-inline.type=INLINE
spring.shardingsphere.rules.sharding.sharding-algorithms.user-table-strategy-inline.algorithm-expression=t_user_$->{user_id % 2}

# Key generate algorithm configuration
spring.shardingsphere.rules.sharding.key-generators.snowflake.type=SNOWFLAKE
spring.shardingsphere.rules.sharding.key-generators.snowflake.props.worker-id=123

# Replica query configuration
# ds_0,ds_1 is the logical data source name of the replica-query
spring.shardingsphere.rules.replica-query.data-sources.ds_0.primary-data-source-name=primary-ds0
spring.shardingsphere.rules.replica-query.data-sources.ds_0.replica-data-source-names=primary-ds0-replica0
spring.shardingsphere.rules.replica-query.data-sources.ds_0.load-balancer-name=replica-random
spring.shardingsphere.rules.replica-query.data-sources.ds_1.primary-data-source-name=primary-ds1
spring.shardingsphere.rules.replica-query.data-sources.ds_1.replica-data-source-names=primary-ds1-replica0
spring.shardingsphere.rules.replica-query.data-sources.ds_1.load-balancer-name=replica-random

# Load balance algorithm configuration
spring.shardingsphere.rules.replica-query.load-balancers.replica-random.type=RANDOM
```