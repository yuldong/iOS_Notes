## 网络
### 请求与响应
#### NSURLConnection

#### NSURL
#### NSURLRequest -> NSMutableURLRequest
#### NSURLResponse -> NSHTTPURLResponse
- 网络请求方式
	- 同步请求
		- block方式处理响应
	
	- 异步请求
		- 代理方式处理响应 <NSURLConnectionDataDelegate>
		- 常用方法
		
		```
			- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response;
			- (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data;
			- (void)connectionDidFinishLoading:(NSURLConnection *)connection;
			- (void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error;
		```
		- block方式处理响应
		
`注意:block一般都会在设置的queue中执行`

**NSURLConnection与NSRunLoop的关系:**

```
1.在主线程初始化一个NSURLConnection对象后,会将该对象的一切加载到mainRunLoop中,所以仍可以触发代理方法
2.在子线程中,如果不是通过 - (instancetype)initWithRequest:(NSURLRequest *)request delegate:(id)delegate startImmediately:(BOOL)startImmediately 初始化一个NSURLConnection对象,则不会触发代理方法 -> 因为子线程中的currentRunLoop必须主动创建运行才能保留,否则对象初始化完成后无法通过runLoop监听对象的事件
3.NSURLConnection的代理方法默认是在主线程中执行的,可以通过- (void)setDelegateQueue:(NSOperationQueue*) queue 方法设置代理方法执行的线程
```

### NSURLSession - iOS 7 之后推荐的网络通信解决方案


### JSON与XML解析
#### JSON
- NSJSONSerialization
	
	```
	+ (BOOL)isValidJSONObject:(id)obj;
	+ (NSData *)dataWithJSONObject:(id)obj options:(NSJSONWritingOptions)opt error:(NSError **)error;
	+ (id)JSONObjectWithData:(NSData *)data options:(NSJSONReadingOptions)opt error:(NSError **)error;
	```
- 三方框架 <JSONKit,SBJSON,MJExtension>

#### XML
- DOM解析
	- 一次性加载到内存,然后再解析 -> GDataXML
- SAX解析 
	- 从根元素开始,边加载,边解析 -> NSXMLParser
