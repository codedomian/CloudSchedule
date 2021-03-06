# CloudSchedule
分布式调度系统，基于zookeeper ,netty,调度内核参考Spring schedule 执行表达式和Spring schedule一样，没有使用Quartz，客户端完全基于注解配置，使用同 Spring schedule一致，最少配置，使用简单


客户端使用只需作如下配置：
1.启用该调度器

@Configuration

@EnableQSchedule

public class ScheduleConfig {

    @Resource
    private Environment env;

    @Bean
    public QScheduleProperties qScheduleProperties() {
        QScheduleProperties properties = new QScheduleProperties();
        properties.setAppName(env.getProperty("schedule.appName"));
        properties.setEnv(env.getProperty("schedule.env"));
        properties.setNameSpace(env.getProperty("schedule.nameSpace"));
        properties.setPort(Integer.valueOf(env.getProperty("schedule.port")));
        properties.setUrl(env.getProperty("schedule.url"));
        return properties;
    }
}

2.使用注解进行定时任务操作


@QSchedule()

public class ScheduleClientDemo {
    
    @Schedule(cron = "*/10 * * * * ?")
    public void schedule1() {
        System.out.println("测试用例1");
    }

    @Schedule(cron = "*/10 * * * * ?", value = "test2")
    public void schedule2() {
        System.out.println("测试用例2");
    }
}


server端主要是进行管理和任务调度

![image](https://github.com/zjpjohn/CloudSchedule/blob/master/img/jietu1.png)
![image](https://github.com/zjpjohn/CloudSchedule/blob/master/img/jietu2.png)
![image](https://github.com/zjpjohn/CloudSchedule/blob/master/img/jietu3.png)



后续版本将主要做两方面：

1.引入leader选举，调度器进行无线扩容，对定时任务的调用进行负载均衡，平滑的在不同调度器上运行

2.调度器进行优化，是定时任务执行更高效
