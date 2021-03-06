# 应用云 Analytics iOS 编程手册

## 开始之前


如果这是您首次向应用添加 Analytics，请完成以下步骤：

在应用云控制台中关联您的应用

1. 安装 [应用云 SDK](https://console.cloud.tencent.com/tac)。
2. 在 [应用云 控制台](https://console.cloud.tencent.com/tac)中，将您的应用添加到您的应用云项目中。
3. 参考 [Analytics 配置文档](https://console.cloud.tencent.com/tac)，配置并初始化  Analytics。


### 记录事件

事件可让您了解您的应用中发生了什么，例如用户操作、系统事件或错误。

>**注意：**
>在记录时间之前您需要先引入头文件 `#import <TACCore/TACCore.h>`。

在配置好 TACAppliction 之后，您就可以使用 `[TACAnalyticsService trackEvent:event]` 来记录事件了。

~~~
TACAnalyticsEvent* event = [TACAnalyticsEvent eventWithIdentifier:@"demo-appear-event"];
[TACAnalyticsService trackEvent:event];
~~~

为帮助您着手，Analytics SDK 定义了许多推荐的事件，这些事件可通用于各种应用类型，包括零售和电子商务、旅行以及游戏应用，比如常见的页面追踪。

### 统计事件时长

事件时长可以统计某个事件的时长，比如用户访问某个页面的时长

~~~
- (void)viewDidLoad {
    [super viewDidLoad];
    TACAnalyticsEvent* event = [TACAnalyticsEvent eventWithIdentifier:@"duration-event"];
    _durationEvent = event;
    // Do any additional setup after loading the view from its nib.
}
- (IBAction)durationStart:(id)sender
{
    [TACAnalyticsService trackEventDurationBegin:_durationEvent];
}

- (IBAction)durationEnd:(id)sender
{
    [TACAnalyticsService trackEventDurationEnd:_durationEvent];
}
~~~
### 会话统计

会话统计用于统计启动次数，由SDK本身维护，通常开发者无需额外设置或调用接口。

以下3种情况下，会视为用户打开一次新的会话：

1. 应用第一次启动，或者应用进程在后台被杀掉之后启动

2. 应用退到后台或锁屏超过一定时间之后再次回到前台，默认是30秒，你也可以根据业务需要修改。

3. 调用 SDK 提供的TACAnalyticsService 的 exchangeNewSection 方法

```
/**
 检测session是否过期，若过期，则生成一个新Session事件
 事件上报方式按照全局上报方式上报
 */

+ (void) exchangeNewSession;
```

### 定制 Analytics 服务
我们提供了一些高级配置项，您可以通过这些配置项定制化您的 Analytics 服务。**请注意，您需要在启动服务前完成配置。**        

参照 TACAnalyticsOptions 中的 定义，你可以修改上报策略、会话时长等，注意需要在服务启动前完成配置。
~~~
@interface TACAnalyticsOptions : TACBaseOptions

/**
 客户端鉴权密钥，与appID共同验证以确定调用合法。需要配置到客户端SDK中，无法更换。该配置只能在json配置文件中设置。
 @note 该配置只能在json配置文件中设置。
 */
@property (nonatomic, strong, readonly) NSString* appKey;

/**
 Analytics数据上报策略,您只能选择一种上报策略，不可叠加使用
 @warning 您只能选择一种上报策略，不可叠加使用.
 @note 默认值采用：TACAnalyticsStrategyLaunch
 */
@property (nonatomic, assign) TACAnalyticsStrategy strategy;

/**
 最大批量发送消息个数，默认30，注意仅在发送策略为BATCH时有效
 */
@property (nonatomic, assign) NSInteger minBatchReportCount;

/**
 上报策略为PERIOD时发送间隔，单位毫秒，默认一天（24*60*60*1000）
 @note 单位为毫秒
 */
@property (nonatomic, assign) uint64_t sendPeriodMillis;

/**
 会话超时时长，在该时间段内用户再次应用则视为同一次会话，默认30000ms。
 @ntoe 单位为毫
 */
@property (nonatomic, assign) uint64_t sessionTimeoutMillis;


/**
 设置是否开启自动统计页面访问，默认开启
 @note 默认为YES
 */
@property (nonatomic, assign) BOOL autoTrackPageEvents;


/**
 智能上报，开启以后设备接入WIFI会实时上报。否则按照全局策略上报。默认打开。
 @note 默认为YES
 */
@property (nonatomic, assign) BOOL smartReporting;


@end
~~~

例如修改上报策略(修改其它配置也同理)：
~~~
TACAnalyticsOptions* analysisOptions = options.analyticsOptions;
//设置为实时上报
analysisOptions.strategy = MTA_STRATEGY_INSTANT;
~~~


### 控制自动页面追踪

默认我们会对用户使用时的页面跳转进行埋点，如果您不希望使用改功能可以关闭该功能

~~~
 TACApplicationOptions* options = [TACApplicationOptions defaultApplicationOptions];
 options.analyticsOptions.autoTrackPageEvents = NO;
     ....
[TACApplication configurateWithOptions:options];
~~~


## 其他功能

其他功能请参考 TACAnalyticsService.h 中的定义
