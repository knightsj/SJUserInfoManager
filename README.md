# SJUserInfoManager
This API uses FMDB to operate 'userInfo database' which contains customized userInfo field.

First of all, if it is more convenient for you to read Chinese documents, you can enter the website below which introduces this project in Chinese language:

[Knight_SJ的简书：高度封装FMDB框架：各用一句代码更新（添加&修改），查询，删除用户信息](http://www.jianshu.com/p/7702f58be845)

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


