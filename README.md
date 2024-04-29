# lib_PrepareFSDatabase
This library will help by preparing an offline FullSync database to be downloaded or installed within an Mobile Application.  This is usefull for preparing large offline databases that will be downloaded once and then synced for updates.

Sample of call :

- lib_PrepareFSDatabase.**PreparePrebuiltDatabase**
 - **database** = myrefdatabase
 - **destination** = RefProject/myrefdatabase.zip

To import using the Android SDK **2.1.5** :

    C8o c8o = new C8o(this, "http://endpoint:28080/convertigo/projects/RefProject", new C8oSettings().setDefaultDatabaseName("myrefdatabase"));
    c8o.callJson("fs://.download_bulk", C8o.FS_DB_ZIP_FILE, "myrefdatabase.zip").then(new C8oOnResponse<JSONObject>() {
        @Override
        public C8oPromise<JSONObject> run(JSONObject response, Map<String, Object> parameters) throws Throwable {
            return c8o.callJson("fs://.replicate_pull");
        }
    }).then(new C8oOnResponse<JSONObject>() {
        @Override
        public C8oPromise<JSONObject> run(JSONObject response, Map<String, Object> parameters) throws Throwable {
            // database is up-to-date
            return null;
        }
    });
    
This library need the jar of [prepare-pre-built-database-1.5.jar](https://github.com/convertigo/prepare-pre-built-database). The jar should be copied at the project root folder.

# Building a database for a given user
Since 1.3 this project now supports buillding databases for a given user. To do so call the __PreparePrebuiltDatabase__ sequence with the follwing parameters:

|name | usage | sample
| --- | ----- | -------
|database | The fullsync database name you want to prepare. This has to be the same name of an existing FullSync connector. | myfullsyncdatabase
|destination| (optional) The zip destination, relative to the projects folder. If the path is empty or invalid, the default destination is used. |  |
|user  | (optional) A user id for which this database has to be built. The data from the fullsync connector will be filtered for this user according to the groups he belongs to. |  |
|renew | (optional) Start a fresh replication [if true], remove the previous temporary database. | true or false |
|workdir | (optional) Specify the fullpath to the workdir used to replicate data locally before the zip creation. Default is the project "_private" folder. |  |
|convertigoUrl | (optional) Specify the Convertigo URL to use to perform the replication. Default is the Convertigo Server application URL. |  |
|socketTimeout | (optional) Specify the replication socket timeout in second. Default is 300 sec (5 min). | number |
|overrideZip | (optional) Specify if the destination zip should be replaced if exist. Default is false. | true or false |

