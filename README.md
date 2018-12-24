# server
当服务器收到网络请求，代码
var server = http.createServer(function(req,res){
    routePath(req,res)
})   //首先代理执行，将请求的url获取后调用routePath函数，
function routePath(req,res){
    var pathObj = url.parse(req.url,true)
    var handleFn = routes[pathObj.pathname]
    if(handleFn){
        req.query = pathObj.query
        var body = ''
        req.on('data',function(chunk){
            body += chunk
        }).on('end',function(){
            req.body = parseBody(body)
            handleFn(req,res)
        })
    }else {
        staticRoot(path.resolve(__dirname,'static'),req,res)
    }
}   //在routepath函数中首先去和routes对象匹配，匹配成功则请求对应
的动态页面，否则请求对应静态文件，静态文件路径通过staticRoot函数来实现
