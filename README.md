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