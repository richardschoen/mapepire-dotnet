# .NET Client class for the IBM Mapepire WebSockets Data Server
This class is a dotnet WebSocket client class for the IBM i Mapepire WebSocket Data Server component.   

Learn more about the IBM i Mapepire Server component here:   
https://github.com/Mapepire-IBMi

```Note: Currently there is an issue with DotNet WebSockets and SSL. To test you will need to disable SSL when starting up the WebSocket server.```

# Starting up mapepire server without SSL for testing   
Start up the mapepire server without SSL via the following command:   
```MP_UNSECURE=true /QOpenSys/pkgs/bin/mapepire```

# Submit start up for mapepire server without SSL via QSHBASH   
```
SBMJOB CMD(QSHONI/QSHBASH CMDLINE('MP_UNSECURE=true /QOpenSys/pkgs/bin/mapepire') 
SETPKGPATH(*YES) PRTSTDOUT(*YES) PRTSPLF(STRMAPEPIR)
PASEJOBNAM(MAPEPIRETH)) JOB(STRMAPEPIR) JOBQ(QUSRNOMAX) USER(&USERID)
JOBMSGQFL(*WRAP) ALWMLTTHD(*YES)            
```

# Sample C# test sequence   
This is a very simple sample connect and query sequence.   

Copy the following statements into a Dotnet C# Console project. 
```
// Set user connection variables
bool secure = false; // This assumes non-SSL server
string host = "hostname";
int port = 8076;
string user = "user1";
string pass = "pass1";

// Instantiate Mapepire Client class
var client = new MapepireClient.Client();

// Connect to WebSocket server
var taskWebConnect = Task.Run(() => client.Connect(host,user,pass,port,secure));
taskWebConnect.Wait();

// Write out connection results
Console.WriteLine(client.GetConnectionResults());

// Run SQL Query
var taskWebQuery = Task.Run(() => client.ExecSqlQuery("select * from qiws.qcustcdt","q1"));
taskWebQuery.Wait();

// Write out sql query results
Console.WriteLine(client.GetQueryResults());

// Disconnect from WebSocket server
var taskWebDisconnect = Task.Run(() => client.Disconnect());
taskWebDisconnect.Wait();
```
