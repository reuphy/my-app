var createError = require('http-errors');
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');
const cors = require('cors') 

var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');

var app = express();
app.use(cors()); 

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.get('/test', (req, res) => {
  // res.send('Bienvenue sur ma route personnalisée !');
  res.header('Access-Control-Allow-Origin', '*'); // Ou spécifiez votre domaine
  return res.redirect(`/users`);
});

app.use('/', indexRouter);
app.use('/users', usersRouter);

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
-------------------------
  constructor(private message: MessageService, private http: HttpClient) { }
  
  ngOnInit() {
 
  }

  test() {
    this.http.get<any>('http://localhost:3000/test').subscribe({
      next: (data) => {
        console.log('data ',data);
      },
      error: (error:any) => {
        console.log('err ',error);
        console.log('err ',error.url);
        // redirect
        // window.location.href = error.url;
      } 
    });
  }
