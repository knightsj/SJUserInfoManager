# SJUserInfoManager
This API uses FMDB to operate 'userInfo database' which contains customized userInfo field.

First of all, if it is more convenient for you to read Chinese documents, you can enter the website below which introduces this project in Chinese language:

[Knight_SJ的简书：高度封装FMDB框架：各用一句代码更新（添加&修改），查询，删除用户信息](http://www.jianshu.com/p/7702f58be845)

这里再简单介绍一下这个框架：
- 该框架封装了iOS的FMDB框架，可以自定义表的名字和字段名，在沙盒中无限制创造数据库表格。
- 表格里包含一个默认字段：user_id，意图是为了将用户信息和用户id绑定在一起。因为通常用户id是无法更改的，它同步于用户的生命周期，所以将user_id设置为默认字段。
- 需要开发者自定义的字段只限于一个，而且其对应的值是字符串属性。这样一来，就可以随意制作任何有关用户信息的表格：用户姓名表格，用户签名表格，用户年龄表格等等。而且，如果是多用户登录，只需要在相同用户信息的表格内添加一份新用户的信息即可，非常方便。
- 关于表格的操作功能：
  1. 更新操作：包括添加用户信息和更新用户信息。在更新用户信息时，如果该用户存在（由user_id）判断，则更改用户信息；如果该用户不存在，则增加该用户信息（添加行）。
  2. 查询某个用户信息操作：传入表的名字和用户id，可以返回用户信息字符串。如果不存在该用户id，则有相应提示。
  3. 查询所有用户信息操作：传入表的名字和自定义的字段，可以返回该表内所有用户信息。
  4. 删除操作：传入表的名字和用户id，可以删除这个表里对应用户的信息（删除行）。如果不存在该用户id，则有相应提示。

# Function
By using this API, you can customize the name of the userInfo database and write it into the document folder in sandbox.

In this userInfo database, there are only two different fields.  One is  the **default field** named : “user_id” and the other one is the **customized field** which can be defined by developers(e.g. user_name, user_phonenumber etc).

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

## 1. The Operation Page in this Demo:
![Operation Page](https://github.com/Shijie0111/SJUserInfoManager/blob/master/Resources/Pic_1.png)
> In this Page, we can update(add,change),query and delete specific userInfo using specific user_id.

####1.1 The outlets of this page:
```
//In the insert part
@property (strong, nonatomic) IBOutlet UITextField *insertUserIdTextfield;      
@property (strong, nonatomic) IBOutlet UITextField *insertUserInfoValueTextfiled;

//In the query part
@property (strong, nonatomic) IBOutlet UITextField *queryUserIdTextfield;       
@property (strong, nonatomic) IBOutlet UILabel *queryUserInfoValueLabel;     
    
//In the delete part
@property (strong, nonatomic) IBOutlet UITextField *deleteUserIdTextField;       
```

####1.2 Define the name of the database and the customized field
```
#define DATABASENAME  @"userInfoTable" 
#define USERINFOFIELD @"user_name"    
```

####1.3 The action of inserting user info:
```
- (IBAction)insertAction:(id)sender {
   
    NSString *result = @"";
    
    if (self.insertUserInfoValueTextfiled.text.length == 0) {
        
         result = @"Please Input UserID!";
        
    }else{
        
         result = [SJUserInfoManager updateUserInfoIntoDataBase:DATABASENAME withUserID:self.insertUserIdTextfield.text andUserInfoField:USERINFOFIELD andUserInfoValue:self.insertUserInfoValueTextfiled.text];
    }
   
   [self showAlertWithTitle:result];
    
}

```

####1.4 The action of querying user info:
```
- (IBAction)queryUserInfoValue:(UIButton *)sender {
    
    NSString *result = @"";
    
    if (self.queryUserIdTextfield.text.length == 0) {
        
        result = @"Please Input UserID!";
        self.queryUserInfoValueLabel.text = @"";
        
    }else{
        
        result =  [SJUserInfoManager queryUserInfoInDataBase:DATABASENAME WithUserID:self.queryUserIdTextfield.text andUserInfoField:USERINFOFIELD];
        self.queryUserInfoValueLabel.text = result;
        [self showAlertWithTitle:result];
        
    }
```

####1.5 The action of deleting user info:
```
- (IBAction)deleteUserInfoWithUserID:(UIButton *)sender {
    
    NSString *result = @"";
    
    if (self.deleteUserIdTextField.text.length == 0) {
        
        result = @"Please Input UserID!";
        
    }else{
        
        result =  [SJUserInfoManager deleteUserInfoInDataBase:DATABASENAME WithUserID:self.deleteUserIdTextField.text];
    }
    
    [self showAlertWithTitle:result];
    
}
```

## 2. The UserInfo List Page in this Demo:
![UserInfo List Page](https://github.com/Shijie0111/SJUserInfoManager/blob/master/Resources/Pic_2.png)
> In this page, we can see all the userInfo in current database.

####2.1 Get all user's info and send them to the second page:

```
-(void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{
    if ([segue.destinationViewController isKindOfClass:[DataListTableViewController class]]) {
        if ([segue.identifier isEqualToString:@"userInfosList"]) {
            
            NSDictionary *userInfosDict = [SJUserInfoManager queryUserInfosInDataBase:DATABASENAME andUserInfoField:USERINFOFIELD];
            DataListTableViewController *dataListVc = (DataListTableViewController *)segue.destinationViewController;
            dataListVc.userInfosDict = userInfosDict;
        }
    }    
}
```

####2.2 Show all user's info in the UITableView of the second page:
```
- (void)viewDidLoad {
    
    [super viewDidLoad];
    
    NSString *alertTitle = [self.userInfosDict objectForKey:@"result"];
    [self showAlertWithTitle:alertTitle];
    
    NSArray *userInfosArray = [self.userInfosDict objectForKey:@"userInfosArray"];
    if ([userInfosArray count] != 0) {
         self.userInfosArray = userInfosArray;
        [self.tableView reloadData];
    }
    
}
```

## 3. The sqlite file in sandbox:
![The sqlite file](https://github.com/Shijie0111/SJUserInfoManager/blob/master/Resources/Pic_3.png)
> We can see that the data of the sqlite file which is in the sandbox is same as the data of the userInfo list page.

Thank you for read this!


