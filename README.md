# SJUserInfoManager
This API uses FMDB to operate 'userInfo database' which contains customized userInfo field.

# Function
By using this API, you can customize the name of the userInfo database and write it into Document folder in sandbox.

In this userInfo database, there are only two different fields.  One is  the default field named : “user_id_” and the other one is customized field which can be defined by developers(e.g. user_name, user_phonenumber etc).

The reason why user_id is the default field is that user_id can not be changed in the database system, while users can easily change their user_name or user_signature and so on.

# Integration
If you want to use this API, please put the SJUserInfoManager.h & SJUserInfoManager.m and FMDB API into your project.

# Public APIs in SJUserInfoManager
```
`
/*  ********** Create table with tableName and fieldName **********
 *  * @param   dataBaseName    tableNameString
 *    @param   userInfoField   fieldNameString
 *  * @return  if the table is successfully created
 *  */
+ (BOOL)createDataBaseWithName:(NSString *)dataBaseName andUserInfoField:(NSString *)userInfoField;

/*  ********** update specific userInfo with specific userID and userInfoField and userInfoValue **********
 *  * @param   dataBaseName    tableNameString
 *    @param   userID          userIDString
 *    @param   userInfoField   fieldNameString
 *    @param   userInfoValue   userInfoValueString
 *  * @return  the result of updating specific userInfo
 *  */
+ (NSString *)updateUserInfoIntoDataBase:(NSString *)dataBaseName withUserID:(NSString *)userID andUserInfoField:(NSString *)userInfoField andUserInfoValue:(NSString *)userInfoValue;

/*  ********** Query specific userInfoValue with tableName and userID and userInfoField **********
 *  * @param   dataBaseName    tableNameString
 *    @param   userID          userIDString
 *    @param   userInfoField   fieldNameString
 *  * @return  specific userInfoValue
 *  */
+ (NSString *)queryUserInfoInDataBase:(NSString *)dataBaseName WithUserID:(NSString *)userId andUserInfoField:(NSString *)userInfoField;

/*  ********** Query all userInfos in this table with userInfoField **********
 *  * @param   dataBaseName    tableNameString
 *    @param   userInfoField   fieldNameString
 *  * @return  all the userInfos in this table
 *  */
+ (NSDictionary *)queryUserInfosInDataBase:(NSString *)dataBaseName andUserInfoField:(NSString *)userInfoField;

/*  ********** Delete specific userInfo with specific userID **********
 *  * @param   dataBaseName    tableNameString
 *    @param   userId          userIDString
 *  * @return  the result of deleting specific userInfo
 *  */
+ (NSString *)deleteUserInfoInDataBase:(NSString *)dataBaseName WithUserID:(NSString *)userId;
```
`
# How to use SJUserInfoManager
In order to show how to user SJUserInfoManager, I offer you a demo to show how it works.

### The Operation Page in this Demo:
![Operation Page](https://github.com/Shijie0111/SJUserInfoManager/Resources/Pic_1.png)
> In this Page, we can update(add,change),query and delete specific userInfo using specific user_id._

### The UserInfo List Page in this Demo:
![UserInfo List Page](https://github.com/Shijie0111/SJUserInfoManager/Resources/Pic_2.png)
> In this page, we can see all the userInfo in current database.

### The sqlite file in sandbox:
![The sqlite file](https://github.com/Shijie0111/SJUserInfoManager/Resources/Pic_3.png)
> We can see that the data of the sqlite file which is in the sandbox is same as the data of the userInfo list page.

Thank you for read this!


