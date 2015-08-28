## MapKit
### 地图
#### MKMapView
- 自定义MKAnnotationView 

	`不用给模型显示设置数据,但是必须在重写的setAnnotation:方法中调用父类的方法`
	- [super setAnnotation:annotation]; 
		
	`该属性用来设置title的显示`
	- @property (nonatomic) BOOL canShowCallout;
- 自定义MKAnnotation
	- 自定义Annotation继承自NSObject,遵守MKAnnotation协议
	- 将协议中的方法重写,并增加自己需要的属性(系统会默认生成相应的方法)
- 代理MKMapViewDelegate中常用方法

```
//	设置大头针的View显示 -> 在添加Annotation时.
//	返回nil,则默认使用MapKit提供的MKAnnotationView
- (MKAnnotationView *)mapView:(MKMapView *)mapView viewForAnnotation:(id <MKAnnotation>)annotation;

//	在MKAnnotationView被加载到(但未渲染)MapView并且固定position时调用该方法
//	该方法可以通过已确定的position对新加载的MKAnnotationView设置加载动画
- (void)mapView:(MKMapView *)mapView didAddAnnotationViews:(NSArray *)views;
```
	

### 导航
#### MapItem


### 路线
#### MKDirections
- 添加polyline
- 调用MKMapView 的代理方法 -> 对Overlay进行渲染

### 百度地图
####
