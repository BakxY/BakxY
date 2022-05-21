# Connect to SQL Database using JavaScript

## SETUP
- install: `npm install mysql`
- import (JS): `const mysql = require('mysql');`
- import (TS): ?

## Usage
- create new connection: 
```
var con = mysql.createConnection({
    host: "IP-Address",
    user: "Username",
    password: "Password",
    database: 'Database'
});
```

- connect to database:
```
con.connect(function(err) {
    if (err) {
      return console.error('error: ' + err.message);
    }
  
    console.log('Connected to the MySQL server.');
});
```

- end connect to database:
```
con.end(function(err) {
    if (err) {
      return console.log('error:' + err.message);
    }
    console.log('Close the database connection.');
});
```

- query database:
```
con.query('YOUR QUERY',
    function (err, result) {
    if(err)
        console.log('Error executing the query - ' + err);
    else
        console.log('Result: ', result) 
});
```