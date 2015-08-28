## Core Location

#### CLLocationManager 类

- 通过CLLocationManager类方法获取当前的权限状态
	- `+ (CLAuthorizationStatus)authorizationStatus `

- 通过CLLocationManager对象方法申请权限 
	- `- (void)requestAlwaysAuthorization`
	- `- (void)requestWhenInUseAuthorization`

`注意点:`

`1.应该仅在当前的权限状态为 kCLAuthorizationStatusNotDetermined 时再去申请权限`

`2.申请权限的方法是iOS8之后新增的方法,使用时要判断当前设备的运行环境`

`3.申请时可能会出现不弹出提示的问题`

```
1> 未在info.plist中配置 <NSLocationAlwaysUsageDescription/NSLocationWhenInUseDescription>
2> CLLocationManager对象被设置成局部变量
```
- 通过其代理方法获取坐标
	
```
//	说明:该方法会一直调用,而且每次都会将坐标加到数组中
- (void)locationManager:(CLLocationManager *)manager
	 didUpdateLocations:(NSArray *)locations
```
`说明:location中存放的是CLLocation对象`

 `通过 - (void)startUpdatingLocation
以及 - (void)stopUpdatingLocation 方法控制该方法的调用
`

- 常用属性

```
//	设置坐标更新的距离阈值
@property(assign, nonatomic) CLLocationDistance distanceFilter;

//	设置期望的坐标定位精度
@property(assign, nonatomic) CLLocationAccuracy desiredAccuracy;
/**
extern const CLLocationAccuracy kCLLocationAccuracyBestForNavigation
`extern const CLLocationAccuracy kCLLocationAccuracyBest;`
extern const CLLocationAccuracy kCLLocationAccuracyNearestTenMeters;
extern const CLLocationAccuracy kCLLocationAccuracyHundredMeters;
extern const CLLocationAccuracy kCLLocationAccuracyKilometer;
extern const CLLocationAccuracy kCLLocationAccuracyThreeKilometers;
*/

//	接收到的坐标
@property(readonly, nonatomic, copy) CLLocation *location;
```
#### CLGeocoder 类
**用来解析位置的详细信息**

- 主要方法

`typedef void (^CLGeocodeCompletionHandler)(NSArray *placemarks, NSError *error);`
`placemarks中存放的是CLPlacemark对象`

```
//	地理编码 -> 通过名称或者编号之类的描述获取详细的地区信息
- (void)geocodeAddressString:(NSString *)addressString completionHandler:(CLGeocodeCompletionHandler)completionHandler;

//	地理编码 -> 通过名称或者编号之类的描述并附加坐标以及半径确定详细的地区信息
- (void)geocodeAddressString:(NSString *)addressString inRegion:(CLRegion *)region completionHandler:(CLGeocodeCompletionHandler)completionHandler;

//	反地理编码 -> 通过经纬度获取相应的地区的详细信息
- (void)reverseGeocodeLocation:(CLLocation *)location completionHandler:(CLGeocodeCompletionHandler)completionHandler;
```



