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
    
This library need the jar of [prepare-pre-built-database-1.3.jar](https://github.com/convertigo/prepare-pre-built-database)

# Building a database for a given user
Since 1.3 this project now supports buillding databases for a given user. To do so call the __PreparePrebuiltDatabase__ sequence with the follwing parameters:

|name | usage | sample
| --- | ----- | -------
|database | the name of the database to source data from, must be the name of a fullsync connector | myfullsyncdatabase
|destination| (optional) the path wher the prebuild database must be build. By default the database will be built in the project's root directory |  |
|user  | (optional) a user id for wich this database has to be built. The data from the fullsync connector will be flitered for this user according to the groups he belongs to |
| renew | (optional) if the database has to be built from scratch or just updated with new data | true or false |

