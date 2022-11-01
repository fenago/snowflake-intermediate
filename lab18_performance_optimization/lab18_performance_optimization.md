### Lab : Performance Optimization: Dedicated Warehouses

Let's create the dedicated warehouses.
The first step, of course, is to create the virtual warehouses.
In this case, we have identified two kind of user groups, which is the data scientists and the database
administrators.
So the first step, of course, is to create these dedicated virtual warehouses.
So the first one will be dedicated to the data scientists and the second one will be dedicated to  DBAs.


```
//  Create virtual warehouse for data scientist & DBA

// Data Scientists
CREATE WAREHOUSE DS_WH 
WITH WAREHOUSE_SIZE = 'SMALL'
WAREHOUSE_TYPE = 'STANDARD' 
AUTO_SUSPEND = 300 
AUTO_RESUME = TRUE 
MIN_CLUSTER_COUNT = 1 
MAX_CLUSTER_COUNT = 1 
SCALING_POLICY = 'STANDARD';

// DBA
CREATE WAREHOUSE DBA_WH 
WITH WAREHOUSE_SIZE = 'XSMALL'
WAREHOUSE_TYPE = 'STANDARD' 
AUTO_SUSPEND = 300 
AUTO_RESUME = TRUE 
MIN_CLUSTER_COUNT = 1 
MAX_CLUSTER_COUNT = 1 
SCALING_POLICY = 'STANDARD';
```



This virtual warehouse has been created, but now we need to create also the users or the user roles
and assign these warehouses to these roles and to these users.
So let's have a look also at how we can do this before we do this node now that we should have the appropriate
rights to do this.
And therefore, we need to switch the role to the account admin.
So here we have the appropriate rights now.
And therefore the first step or the next step would be to create the roles for the data scientists and
the database administrators and then also grant usage on the warehouses to these roles that we have
created.
So let's go ahead and create the roles we have here, the role of the data scientists, and now we grant
usage on the warehouse.
So this has been executed successfully also here for the database administrators.
And now we have just created these roles and these roles have now grants to the usage of this warehouse.


```
// Create role for Data Scientists & DBAs

CREATE ROLE DATA_SCIENTIST;
GRANT USAGE ON WAREHOUSE DS_WH TO ROLE DATA_SCIENTIST;

CREATE ROLE DBA;
GRANT USAGE ON WAREHOUSE DBA_WH TO ROLE DBA;
```

But still, we don't have users set up.
That means that we have to set up the logins and assign these users to these roles.
So here in the first step, we will just create the users.
So in this case, we will have three data scientists and therefore we will create here these three users
with these three commands.
So you see that we just create users, we set password and login, and we have here the default role
assigned.
And this is also the default warehouse.
But still, we need to grant this role.
So all of the grants that are here are assigned to this role, also to these users.
So this is what we will do with these commands.
And now we also have the same thing, of course, also for the database administrators.
So it's basically the same thing.
We create the user here, the user name password, then we have the log in name and then also here the
default role and the default warehouse.
And then we have to assign again the roles to these users.
So in here we have two database administrators.
So let's grant also these roles to.



```
// Setting up users with roles

// Data Scientists
CREATE USER DS1 PASSWORD = 'DS1' LOGIN_NAME = 'DS1' DEFAULT_ROLE='DATA_SCIENTIST' DEFAULT_WAREHOUSE = 'DS_WH'  MUST_CHANGE_PASSWORD = FALSE;
CREATE USER DS2 PASSWORD = 'DS2' LOGIN_NAME = 'DS2' DEFAULT_ROLE='DATA_SCIENTIST' DEFAULT_WAREHOUSE = 'DS_WH'  MUST_CHANGE_PASSWORD = FALSE;
CREATE USER DS3 PASSWORD = 'DS3' LOGIN_NAME = 'DS3' DEFAULT_ROLE='DATA_SCIENTIST' DEFAULT_WAREHOUSE = 'DS_WH'  MUST_CHANGE_PASSWORD = FALSE;

GRANT ROLE DATA_SCIENTIST TO USER DS1;
GRANT ROLE DATA_SCIENTIST TO USER DS2;
GRANT ROLE DATA_SCIENTIST TO USER DS3;


// DBAs
CREATE USER DBA1 PASSWORD = 'DBA1' LOGIN_NAME = 'DBA1' DEFAULT_ROLE='DBA' DEFAULT_WAREHOUSE = 'DBA_WH'  MUST_CHANGE_PASSWORD = FALSE;
CREATE USER DBA2 PASSWORD = 'DBA2' LOGIN_NAME = 'DBA2' DEFAULT_ROLE='DBA' DEFAULT_WAREHOUSE = 'DBA_WH'  MUST_CHANGE_PASSWORD = FALSE;

GRANT ROLE DBA TO USER DBA1;
GRANT ROLE DBA TO USER DBA2;
```

Now, we will test these users.
So now let's look out of our account and see if this has worked properly and I'm going to log in again
using this role.
And here we see that we have now here again, this welcome screen, we don't need to see this so we
can all close this down.
We see we have already here this default role assigned.
And if we click on this to be able to change it, we have only this role and of course, the public
role that is always available for every login.
And if we have here the warehouses, we see that we have only this warehouse here available and the
same thing.
We can also see if we navigate here to the warehouse warehouses, stop.
Then we see that also in here we have this warehouse available.
So this is how easily we can set up dedicated virtual warehouses for certain user groups.
First, we just create the warehouse with the appropriate size.
Then we can create the rolls for these user groups and then we have to grant usage to this role for
these warehouses.
And then we can just create the logins.
We can set here the default rolls and the default warehouses and then also grant these roles to these
users are these logins that we have created.
So this was the first important aspect in terms of optimizing the performance.



```
// Drop objects again

DROP USER DBA1;
DROP USER DBA2;

DROP USER DS1;
DROP USER DS2;
DROP USER DS3;

DROP ROLE DATA_SCIENTIST;
DROP ROLE DBA;

DROP WAREHOUSE DS_WH;
DROP WAREHOUSE DBA_WH;
```