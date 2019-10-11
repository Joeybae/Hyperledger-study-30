# Hyperledger-study-30

하이퍼레져 앱 만들기 실습 30일차 - 반복문 while, 배열, 배열과 반복문, 함수, 글 목록 출력 및 코드 정리

출처 : https://opentutorials.org/course/3332/21127

1. 반복문 - syntax/loop.js

        console.log('A');
        console.log('B');

        var i  = 0;
        while(i < 2){
            console.log('C1');
            console.log('C2');
            i = i + 1;
        }

        console.log('D');

2. 배열 - syntax/array.js

        var arr = ['A', 'B', 'C', 'D'];
        console.log(arr[1]);
        console.log(arr[3]);
        arr[2] = 3;
        console.log(arr);
        console.log(arr.length)
        arr.push('E');
        console.log(arr);

3. 배열과 반복문 - syntax/array-loop.js

        var number = [1, 400, 12, 34, 5];
        var total = 0;
        var i = 0;
        while(i < number.length){
            total = total + number[i];
            i = i + 1;
        }
        console.log(`total : ${total}`);

4. 함수 - syntax/function1.js

        f123();
        console.log('A');
        console.log('Z');
        console.log('B');
        f123();
        console.log('F');
        console.log('C');
        console.log('P');
        console.log('J');
        f123();
        console.log('U');
        console.log('A');
        console.log('Z');
        console.log('J');
        console.log('I');
        f123();

        function f123(){
          console.log(1);
          console.log(2);
          console.log(3);
          console.log(4);
        }

5. 함수 - syntax/function2.js

        console.log(Math.round(1.6)); //2
        console.log(Math.round(1.4)); //1

        function sum(first, second){ // parameter

          return first+second;
        }

        console.log(sum(2,4)); // argument

6. 글 목록 출력 - nodejs/readdir.js

        var testFolder = './data';
        var fs = require('fs');

        fs.readdir(testFolder, function(error, filelist){
            console.log(filelist);
        })

7. 코드 정리 - main.js

        var http = require('http');
        var fs = require('fs');
        var url = require('url');

        function templateHTML(title, list, body){
          return `
          <!doctype html>
          <html>
          <head>
            <title>WEB1 - ${title}</title>
            <meta charset="utf-8">
          </head>
          <body>
            <h1><a href="/">WEB</a></h1>
            ${list}
            ${body}
          </body>
          </html>
          `;
        }

        function templateList(filelist){
          var list = '<ul>';
          var i = 0;
          while(i < filelist.length){
            list = list + `<li><a href="/?id=${filelist[i]}">${filelist[i]}</a></li>`;
            i = i + 1;
          }
          list = list+'</ul>';
          return list;
        } 

        var app = http.createServer(function(request,response){
            var _url = request.url;
            var queryData = url.parse(_url, true).query;
            var pathname = url.parse(_url, true).pathname;
            if(pathname === '/'){
              if(queryData.id === undefined){
                fs.readdir('./data', function(error, filelist){
                  var title = 'Welcome';
                  var description = 'Hello, Node.js';
                  var list = templateList(filelist);
                  var template = templateHTML(title, list, `<h2>${title}</h2>${description}`);
                  response.writeHead(200);
                  response.end(template);
                })
              } else {
                fs.readdir('./data', function(error, filelist){
                  fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description){
                    var title = queryData.id;
                    var list = templateList(filelist);
                    var template = templateHTML(title, list, `<h2>${title}</h2>${description}`);
                    response.writeHead(200);
                    response.end(template);
                  });
                });
              }
            } else {
              response.writeHead(404);
              response.end('Not found');
            }
        });
        app.listen(3000);
