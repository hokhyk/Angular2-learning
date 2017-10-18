# AJAX——核心XMLHttpRequest对象 

首先，需要先了解这个对象的属性和方法：


属性
	

说明

readyState
	

表示XMLHttpRequest对象的状态：0：未初始化。对象已创建，未调用open；

1：open方法成功调用，但Sendf方法未调用；

2：send方法已经调用，尚未开始接受数据；

3：正在接受数据。Http响应头信息已经接受，但尚未接收完成；

4：完成，即响应数据接受完成。

Onreadystatechange
	

请求状态改变的事件触发器（readyState变化时会调用这个属性上注册的javascript函数）。

responseText
	

服务器响应的文本内容

responseXML
	

服务器响应的XML内容对应的DOM对象

Status
	

服务器返回的http状态码。200表示“成功”，404表示“未找到”，500表示“服务器内部错误”等。

statusText
	

服务器返回状态的文本信息。


方法
	

说明

Open(string method,string url,boolean asynch,

String username,string password)
	

指定和服务器端交互的HTTP方法，URL地址，即其他请求信息；

Method:表示http请求方法，一般使用"GET","POST".

url：表示请求的服务器的地址；

asynch：表示是否采用异步方法，true为异步，false为同步；

后边两个可以不指定，username和password分别表示用户名和密码，提供http认证机制需要的用户名和密码。

Send(content)
	

向服务器发出请求，如果采用异步方式，该方法会立即返回。

content可以指定为null表示不发送数据，其内容可以是DOM对象，输入流或字符串。

SetRequestHeader(String header,String Value)
	

设置HTTP请求中的指定头部header的值为value.

此方法需在open方法以后调用，一般在post方式中使用。

getAllResponseHeaders()
	

返回包含Http的所有响应头信息，其中相应头包括Content-length,date,uri等内容。

返回值是一个字符串，包含所有头信息，其中每个键名和键值用冒号分开，每一组键之间用CR和LF（回车加换行符）来分隔！

getResponseHeader(String header)
	

返回HTTP响应头中指定的键名header对应的值

Abort()
	

停止当前http请求。对应的XMLHttpRequest对象会复位到未初始化的状态。


 对这个对象有了静态了了解，知道它长的什么样子，有什么功能了，下边该我们使用它了，当然这里我也用五步法写出代码来：

第一步：创建XMLHttpRuquest对象：
[javascript] view plain copy
print?

    var xmlhttprequest;  
       if(window.XMLHttpRequest){  
           xmlhttprequest=new XMLHttpRequest();  
           if(xmlhttprequest.overrideMimeType){  
               xmlhttprequest.overrideMimeType("text/xml");  
           }  
       }else if(window.ActiveXObject){  
           var activeName=["MSXML2.XMLHTTP","Microsoft.XMLHTTP"];  
           for(var i=0;i<activeName.length;i++){  
               try{  
                   xmlhttprequest=new ActiveXObject(activeName[i]);  
                   break;  
               }catch(e){  
                            
               }  
           }  
       }  
         
       if(xmlhttprequest==undefined || xmlhttprequest==null){  
           alert("XMLHttpRequest对象创建失败！！");  
       }else{  
           this.xmlhttp=xmlhttprequest;  
       }  



    第二步：注册回调方法

[javascript] view plain copy
print?

    <span style="font-size:18px;">xmlhttp.onreadystatechange=callback;  
    </span>  


    第三步：设置和服务器交互的相应参数
[javascript] view plain copy
print?

    <span style="font-size:18px;"> xmlhttp.open("GET","ajax?name=" +userName,true);  
    </span>  


    第四步：设置向服务器端发送的数据，启动和服务器端的交互


[javascript] view plain copy
print?

    <span style="font-size:18px;">  xmlhttp.send(null);</span>  


    第五步：判断和服务器端的交互是否完成，还要判断服务器端是否返回正确的数据

[javascript] view plain copy
print?

    <span style="font-size:18px;">//根基实际条件写callback的功能代码  
    function callback(){  
         if（xmlhttp.readState==4）{  
             //表示服务器的相应代码是200；正确返回了数据   
            if(xmlhttp.status==200){   
                //纯文本数据的接受方法   
                var message=xmlhttp.responseText;   
                //使用的前提是，服务器端需要设置content-type为text/xml   
                //var domXml=xmlhttp.responseXML;   
                //其它代码  
             }   
        }  
     }  
    </span>  


通过这五步XMLHttpRequest基本上就创建好，可以正常使用了，但是这是需要非常多的代码的，总不能每次创建都写这么多吧？当然不是了，我们学习了面向对象，我们将其必要相同的部分都抽象出来，使之成为一个独立类，别的直接调用即可，在网上看了一个，感觉写的挺好：


[javascript] view plain copy
print?

    //类的构建定义，主要职责就是新建XMLHttpRequest对象  
    var MyXMLHttpRequest=function(){  
        var xmlhttprequest;  
        if(window.XMLHttpRequest){  
            xmlhttprequest=new XMLHttpRequest();  
            if(xmlhttprequest.overrideMimeType){  
                xmlhttprequest.overrideMimeType("text/xml");  
            }  
        }else if(window.ActiveXObject){  
            var activeName=["MSXML2.XMLHTTP","Microsoft.XMLHTTP"];  
            for(var i=0;i<activeName.length;i++){  
                try{  
                    xmlhttprequest=new ActiveXObject(activeName[i]);  
                    break;  
                }catch(e){  
                             
                }  
            }  
        }  
          
        if(xmlhttprequest == undefined || xmlhttprequest == null){  
            alert("XMLHttpRequest对象创建失败！！");  
        }else{  
            this.xmlhttp=xmlhttprequest;  
        }  
          
        //用户发送请求的方法  
        MyXMLHttpRequest.prototype.send=function(method,url,data,callback,failback){  
            if(this.xmlhttp!=undefined && this.xmlhttp!=null){  
                method=method.toUpperCase();  
                if(method!="GET" && method!="POST"){  
                    alert("HTTP的请求方法必须为GET或POST!!!");  
                    return;  
                }  
                if(url==null || url==undefined){  
                    alert("HTTP的请求地址必须设置！");  
                    return ;  
                }  
                var tempxmlhttp=this.xmlhttp;  
                this.xmlhttp.onreadystatechange=function(){  
                    if(tempxmlhttp.readyState==4){  
                        if(temxmlhttp.status==200){  
                            var responseText=temxmlhttp.responseText;  
                            var responseXML=temxmlhttp.reponseXML;  
                            if(callback==undefined || callback==null){  
                                alert("没有设置处理数据正确返回的方法");  
                                alert("返回的数据：" + responseText);  
                            }else{  
                                callback(responseText,responseXML);  
                            }  
                        }else{  
                            if(failback==undefined ||failback==null){  
                                alert("没有设置处理数据返回失败的处理方法！");  
                                alert("HTTP的响应码：" + tempxmlhttp.status + ",响应码的文本信息：" + tempxmlhttp.statusText);  
                            }else{  
                                failback(tempxmlhttp.status,tempxmlhttp.statusText);  
                            }  
                        }  
                    }  
                }  
                  
                //解决缓存的转换  
                if(url.indexOf("?")>=0){  
                    url=url + "&t=" + (new Date()).valueOf();  
                }else{  
                    url=url+"?+="+(new Date()).valueOf();  
                }  
                  
                //解决跨域的问题  
                if(url.indexOf("http://")>=0){  
                    url.replace("?","&");  
                    url="Proxy?url=" +url;  
                }  
                this.xmlhttp.open(method,url,true);  
                  
                //如果是POST方式，需要设置请求头  
                if(method=="POST"){  
                    this.xmlhttp.setRequestHeader("Content-type","application/x-www-four-urlencoded");  
                }  
                this.xmlhttp.send(data);  
        }else{  
            alert("XMLHttpRequest对象创建失败，无法发送数据！");  
        }  
        MyXMLHttpRequest.prototype.abort=function(){  
            this.xmlhttp.abort();  
        }  
      }  
    }  


          当然这些都需要我们在实践中不断的摸索，使用，再总结属于自己的一套常用类，方法等。当然XMLHttpRequest还有“浏览器缓存问题”，“中文乱码问题”，“跨域访问问题”等等，因为都没有遇到过这些东西，所以这里先了解一下，以后遇到再认真研究！
          
# jquery中的ajax

以前写Ajax操作，是利用AJAX——核心XMLHttpRequest对象。
而JQuery也对Ajax异步操作进行了封装，这里看一下几种常用的方式。 $.ajax，$.post， $.get， $.getJSON。

         一， $.ajax，这个是JQuery对ajax封装的最基础步，通过使用这个函数可以完成异步通讯的所有功能。也就是说什么情况下我们都可以通过此方法进行异步刷新的操作。但是它的参数较多，有的时候可能会麻烦一些。看一下常用的参数：      

     var configObj = {

           method   //数据的提交方式：get和post

           url   //数据的提交路劲

           async   //是否支持异步刷新，默认是true

           data    //需要提交的数据

           dataType   //服务器返回数据的类型，例如xml,String,Json等

           success    //请求成功后的回调函数

           error   //请求失败后的回调函数

        }

     

    $.ajax(configObj);//通过$.ajax函数进行调用。

 

           好，看一个实际的例子吧，看一个进行异步删除的例子：
[javascript] view plain copy
print?

    <span style="font-size:18px;">          // 删除  
                    $.ajax({  
                        type : "POST",  //提交方式  
                        url : "${pageContext.request.contextPath}/org/doDelete.action",//路径  
                        data : {  
                            "org.id" : "${org.id}"  
                        },//数据，这里使用的是Json格式进行传输  
                        success : function(result) {//返回数据根据结果进行相应的处理  
                            if ( result.success ) {  
                                $("#tipMsg").text("删除数据成功");  
                                tree.deleteItem("${org.id}", true);  
                            } else {  
                                $("#tipMsg").text("删除数据失败");  
                            }  
                        }  
                    });  
    </span>  


           二，$.post，这个函数其实就是对$.ajax进行了更进一步的封装，减少了参数，简化了操作，但是运用的范围更小了。$.post简化了数据提交方式，只能采用POST方式提交。只能是异步访问服务器，不能同步访问，不能进行错误处理。在满足这些情况下，我们可以使用这个函数来方便我们的编程，它的主要几个参数，像method，async等进行了默认设置，我们不可以改变的。例子不再介绍。

    url:发送请求地址。

    data:待发送 Key/value 参数。

    callback:发送成功时回调函数。

    type:返回内容格式，xml, html, script, json, text,_default。

 

        三，$.get，和$.post一样，这个函数是对get方法的提交数据进行封装，只能使用在get提交数据解决异步刷新的方式上，使用方式和上边的也差不多。这里不再演示。

 

        四， $.getJSON，这个是进一步的封装，也就是对返回数据类型为Json进行操作。里边就三个参数，需要我们设置，非常简单：url,[data],[callback]。

 

        其实会了$.ajax方法，其它的就都会使用了，都是一样的，其实非常简单。

 

        但是这里还有一个问题，比较麻烦，就是如果页面数据量比较大，该怎么办呢？在常规表单的处理中，我们使用框架Struts2可以通过域驱动模式进行自动获取封装，那么通过ajax,如何进行封装呢？这里JQuery有一个插件，Jquery Form，通过引入此js文件，我们可以模仿表单Form来支持Struts2的域驱动模式，进行自动数据的封装。用法和$.ajax类似，看一下实际的例子，这里写一个保存用户的前台代码：

 
[javascript] view plain copy
print?

    <span style="font-size:18px;">  $(function(){  
            var options = {  
                beforeSubmit : function() {//处理以前需要做的功能  
                    $("tipMsg").text("数据正在保存，请稍候...");  
                    $("#insertBtn").attr("disabled", true);  
                },  
                success : function(result) {//返回成功以后需要的回调函数  
                    if ( result.success ) {  
                        $("#tipMsg").text("机构保存成功");  
                                          
                                           //这里是对应的一棵树，后边会介绍到，  
                        // 控制树形组件，增加新的节点  
                        var tree = window.parent.treeFrame.tree;  
                        tree.insertNewChild("${org.id}", result.id, result.name);  
                    } else {  
                        $("#tipMsg").text("机构保存失败");  
                    }  
                    // 启用保存按钮  
                    $("#insertBtn").attr("disabled", false);  
                },  
                clearForm : true  
            };  
          
            $('#orgForm').ajaxForm(options); //通过Jquery.Form中的ajaxForm方法进行提交  
        });  
    </span>  


 

       这样我们就不用再进行数据data的封装处理，大大简化了我们ajax的操作这样异步刷新的操作。综上为JQuery中ajax的操作，感觉使用多了，和form表单的处理还是非常相似的，只不过实现的功能不一样罢了。学习编程，其实就是学习对数据的流转处理，如何从前台获取，传输到服务器进行相应的处理，然后返回，进行相关的显示，把这个流程通过一些技术实现，就完成了软件的开发，感觉还是非常有意思的。
