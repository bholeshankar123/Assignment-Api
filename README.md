#  Assignment-Api
Database.js file
var mysql = require('mysql');  
module.exports = function handle_db(req, res) {  
    var pool = mysql.createPool({  
        connectionLimit: 100,  
        host: 'localhost',  
        user: 'root',  
        password: '*******',  
        database: 'test'  
    });  
    pool.getConnection(function (err, connection) {  
        if (err) {  
            console.error("This is error msg, when connecting to db: " + err);  
            connection.release();  
            res.json({ "code": 100, "status": "Error in connecting database" });  
            return;  
        }  
        console.log("from db config: connected as id: " + connection.threadId);  
        connection.on('error', function (err) {  
            res.json({ "code": 100, "status": "Error in connection database" });  
            return;  
        });  
        return connection;  
    });  
return pool;
}




app.js file
var createError = require('http-errors');
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');

var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');
var getUsersRouter = require('./routes/getuser');

var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', indexRouter);
app.use('/users', usersRouter);
app.use('/getuser', getUsersRouter);

// catch 404 and forward to error handler
app.use(function(req, res, next) {
  next(createError(404));
});

// error handler
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;


#SPA Localhost:3000



 
