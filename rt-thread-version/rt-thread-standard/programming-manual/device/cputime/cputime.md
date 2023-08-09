# CPUTIME 设备

内核中的定时器是以 tick 为精度实现的。出现精度不高的情况。cputime 通过对接硬件定时器来实现，提高了精度，不过作为代价是，在每个 bsp 中都需要具体实现 cputime 的对接工作。

#### cputime 相关接口

这些定时器接口除了定时器控制块与内核中的定时器不同外，其他的参数、返回值以及用法基本一致，详情请看：[定时器管理方式](/rt-thread-version/rt-thread-standard/programming-manual/timer/timer?id=定时器的管理方式)

| **函数** | **描述**                           |
| -------------------- | ---------------------------------- |
| rt_cputimer_detach();|定时器脱离|
| rt_cputimer_create();|定时器创建|
| rt_cputimer_delete();|定时器删除|
| rt_cputimer_start();|定时器启动|
| rt_cputimer_stop();|定时器停止|
| rt_cputimer_control();|定时器控制|

这些是一些高精度延时的接口（这里只提供了接口，具体精度能不能达到还是要看硬件定时器中的实现）

| **函数** | **描述**                           |
| -------------------- | ---------------------------------- |
| rt_cputime_sleep();|cputime tick 精度延时|
| rt_cputime_ndelay();|纳秒精度延时|
| rt_cputime_udelay();|微秒精度延时|
| rt_cputime_mdelay();|毫秒精度延时|

#### 如何对接 cputime?

1. 对接硬件定时器，使其形成单调递增的时钟，并且到达指定时间可以触发回调
2. 根据硬件定时器的精度情况来对接 cputime 的 ops

下面是一些如何对接到 cputime 的示例代码

```c

#include <rthw.h>
#include <rtdevice.h>
#include <rtthread.h>

#include <board.h>

#define HWTIMER_DEV_NAME   "timer7"

/* 递增 count 作为 cpu tick 使用，不可被设置，只可单调递增 */
rt_uint64_t count = 0;

/* 当 CPU tick 到达 timeout_tick 时触发回调 */
rt_uint64_t timeout_tick = 0;

void (*hwtimer_timeout)(void *param);
void *parameter = RT_NULL;

/* 精度 ns */
static double cortexm_cputime_getres(void)
{
    return 10000;
}

/* cpu tick */
static uint64_t cortexm_cputime_gettime(void)
{
	return count;
}

/* 设置超时时间及函数 */
static int cortexm_cputime_settimeout(uint64_t tick, void (*timeout)(void *param), void *param)
{
	timeout_tick = tick;
	hwtimer_timeout = timeout;
	parameter = param;
	return 0;
}

/* 定义 ops */
const static struct rt_clock_cputime_ops _cortexm_ops =
{
    cortexm_cputime_getres,
    cortexm_cputime_gettime,
	cortexm_cputime_settimeout
};

/* 初始化 ops */
int cortexm_cputime_init(void)
{
    /* bsp 在这样使用的时候，不用管内部有没有实现，这个函数会覆盖掉内部的实现 */
    clock_cpu_setops(&_cortexm_ops);
    return 0;
}
INIT_BOARD_EXPORT(cortexm_cputime_init);

/* 硬件定时器超时函数 */
static rt_err_t timeout_cb(rt_device_t dev, rt_size_t size)
{
    count++;
    if(count == timeout_tick)
    {
        /* 到达指定时间后清空到达时间，并触发回调 */
        timeout_tick = 0;
        if(hwtimer_timeout != RT_NULL)
            hwtimer_timeout(parameter);
    }
    return 0;
}

/* 硬件定时器初始化 */
static int hwtimer_init()
{
    rt_err_t ret = RT_EOK;
    rt_hwtimerval_t timeout_s;    
    rt_device_t hw_dev = RT_NULL; 
    rt_hwtimer_mode_t mode;     
    rt_uint32_t freq = 1000000;            

    hw_dev = rt_device_find(HWTIMER_DEV_NAME);
    if (hw_dev == RT_NULL)
    {   
        /* 硬件定时器失败时，清空设置超时的函数，使 cputime 不可用，
        防止使用 rt_cputime_sleep() 时卡死 */
        _cortexm_ops.cputime_settimeout = RT_NULL;
        rt_kprintf("hwtimer sample run failed! can't find %s device!\n", HWTIMER_DEV_NAME);
        return RT_ERROR;
    }

    ret = rt_device_open(hw_dev, RT_DEVICE_OFLAG_RDWR);
    if (ret != RT_EOK)
    {
        _cortexm_ops.cputime_settimeout = RT_NULL;
        rt_kprintf("open %s device failed!\n", HWTIMER_DEV_NAME);
        return ret;
    }
    /* 设置硬件定时器的超时时间 */
    rt_device_set_rx_indicate(hw_dev, timeout_cb);

    rt_device_control(hw_dev, HWTIMER_CTRL_FREQ_SET, &freq);
    mode = HWTIMER_MODE_PERIOD;
    ret = rt_device_control(hw_dev, HWTIMER_CTRL_MODE_SET, &mode);
    if (ret != RT_EOK)
    {
        _cortexm_ops.cputime_settimeout = RT_NULL;
        rt_kprintf("set mode failed! ret is :%d\n", ret);
        return ret;
    }
    timeout_s.sec = 0;   
    timeout_s.usec = 10;

    /* 设置硬件定时器的精度，这里应和上述的 cortexm_cputime_getres 函数保持一致 */
    if (rt_device_write(hw_dev, 0, &timeout_s, sizeof(timeout_s)) != sizeof(timeout_s))
    {
        _cortexm_ops.cputime_settimeout = RT_NULL;
        rt_kprintf("set timeout value failed\n");
        return RT_ERROR;
    }
    return ret;
}
INIT_APP_EXPORT(hwtimer_init);
```
