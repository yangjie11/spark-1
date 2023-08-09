# 内核 API 更新日志

在 rt-thread 源码进行版本发布时，若内核 (src) API 有更新，则将内核 API 的详细变动记录在此。

注：暂未包含 SMP 相关的接口，日志内容含：API 增加、API 删除、API 变更。

## 5.0.1

#### API 增加：

```c
/* ----- irq.c ----- */
rt_weak rt_bool_t rt_hw_interrupt_is_disabled(void) /* 5.0.1 */

/* ----- object.c ----- */
rt_err_t rt_object_get_name(rt_object_t object, char *name, rt_uint8_t name_size) /* 5.0.1 */

/* ----- thread.c ----- */
rt_err_t rt_thread_get_name(rt_thread_t thread, char *name, rt_uint8_t name_size) /* 5.0.1 */
```

#### API 变更：

```c
/* ----- ipc.c ----- */
/* 消息队列接收相关的 API 返回值类型更新，由 rt_err_t（接收成功/失败） 改为 rt_ssize_t(接收的实际消息大小) */
rt_ssize_t rt_mq_recv(rt_mq_t    mq,
                    void      *buffer,
                    rt_size_t  size,
                    rt_int32_t timeout) /* 5.0.1 */
rt_err_t   rt_mq_recv(rt_mq_t    mq,
                    void      *buffer,
                    rt_size_t  size,
                    rt_int32_t timeout) /* 5.0.0 */

rt_ssize_t rt_mq_recv_interruptible(rt_mq_t    mq,
                    void      *buffer,
                    rt_size_t  size,
                    rt_int32_t timeout) /* 5.0.1 */
rt_err_t rt_mq_recv_interruptible(rt_mq_t    mq,
                    void      *buffer,
                    rt_size_t  size,
                    rt_int32_t timeout) /* 5.0.0 */

rt_ssize_t rt_mq_recv_killable(rt_mq_t    mq,
                    void      *buffer,
                    rt_size_t  size,
                    rt_int32_t timeout) /* 5.0.1 */
rt_err_t rt_mq_recv_killable(rt_mq_t    mq,
                    void      *buffer,
                    rt_size_t  size,
                    rt_int32_t timeout) /* 5.0.0 */
```

## 5.0.0

- RT_WEAK 改为 rt_weak
- ALIGN 改为 rt_align
- 新增 driver.c 文件
- scheduler.c 分为 scheduler_mp.c（SMP 相关） 和 scheduler_up.c（单核）

#### API 增加：

```c
/* ----- device.c ----- */
// #ifdef RT_USING_DM
rt_err_t rt_device_bind_driver(rt_device_t device, rt_driver_t driver, void *node); /* 5.0.0 */
rt_device_t rt_device_create_since_driver(rt_driver_t drv,int device_id); /* 5.0.0 */
rt_err_t rt_device_probe_and_init(rt_device_t device); /* 5.0.0 */
// #endif


/* ----- driver.c(新增)----- */
// #ifdef RT_USING_DM
rt_err_t rt_driver_match_with_id(const rt_driver_t drv,int device_id); /* 5.0.0 */
rt_err_t rt_driver_match_with_dtb(const rt_driver_t drv,void *from_node,int max_dev_num); /* 5.0.0 */
// #endif


/* ----- ipc.c ----- */
rt_err_t rt_sem_take_interruptible(rt_sem_t sem, rt_int32_t timeout); /* 5.0.0 */
rt_err_t rt_sem_take_killable(rt_sem_t sem, rt_int32_t timeout); /* 5.0.0 */
void rt_mutex_drop_thread(rt_mutex_t mutex, rt_thread_t thread); /* 5.0.0 */
rt_uint8_t rt_mutex_setprioceiling(rt_mutex_t mutex, rt_uint8_t priority); /* 5.0.0 */
rt_uint8_t rt_mutex_getprioceiling(rt_mutex_t mutex); /* 5.0.0 */
rt_err_t rt_mutex_take_interruptible(rt_mutex_t mutex, rt_int32_t time); /* 5.0.0 */
rt_err_t rt_mutex_take_killable(rt_mutex_t mutex, rt_int32_t time); /* 5.0.0 */
rt_err_t rt_event_recv_interruptible(rt_event_t   event,
                       rt_uint32_t  set,
                       rt_uint8_t   opt,
                       rt_int32_t   timeout,
                       rt_uint32_t *recved); /* 5.0.0 */
rt_err_t rt_event_recv_killable(rt_event_t   event,
                       rt_uint32_t  set,
                       rt_uint8_t   opt,
                       rt_int32_t   timeout,
                       rt_uint32_t *recved); /* 5.0.0 */
rt_err_t rt_mb_send_wait_interruptible(rt_mailbox_t mb,
                         rt_ubase_t  value,
                         rt_int32_t   timeout); /* 5.0.0 */
rt_err_t rt_mb_send_wait_killable(rt_mailbox_t mb,
                         rt_ubase_t  value,
                         rt_int32_t   timeout); /* 5.0.0 */
rt_err_t rt_mb_recv_interruptibale(rt_mailbox_t mb, rt_ubase_t *value, rt_int32_t timeout); /* 5.0.0 */
rt_err_t rt_mb_recv_killable(rt_mailbox_t mb, rt_ubase_t *value, rt_int32_t timeout); /* 5.0.0 */
rt_err_t rt_mq_send_interrupt(rt_mq_t mq, const void *buffer, rt_size_t size); /* 5.0.0 */
rt_err_t rt_mq_send_killable(rt_mq_t mq, const void *buffer, rt_size_t size); /* 5.0.0 */
rt_err_t rt_mq_send_wait_interruptible(rt_mq_t     mq,
                                    const void *buffer,
                                    rt_size_t   size,
                                    rt_int32_t  timeout); /* 5.0.0 */
rt_err_t rt_mq_send_wait_killable(rt_mq_t     mq,
                                const void *buffer,
                                rt_size_t   size,
                                rt_int32_t  timeout); /* 5.0.0 */
rt_err_t rt_mq_recv_interruptible(rt_mq_t    mq,
                                void      *buffer,
                                rt_size_t  size,
                                rt_int32_t timeout); /* 5.0.0 */
rt_err_t rt_mq_recv_killable(rt_mq_t    mq,
                            void      *buffer,
                            rt_size_t  size,
                            rt_int32_t timeout); /* 5.0.0 */


/* ----- object.c ----- */
rt_object_t rt_custom_object_create(const char *name, void *data, rt_err_t (*data_destroy)(void *)) /* 5.0.0 */
rt_err_t rt_custom_object_destroy(rt_object_t obj) /* 5.0.0 */

```

#### API 变更：

```c
/* ----- device.c ----- */
rt_ssize_t rt_device_read(rt_device_t dev,
                         rt_off_t    pos,
                         void       *buffer,
                         rt_size_t   size) /* 5.0.0 */
rt_size_t  rt_device_read(rt_device_t dev,
                         rt_off_t    pos,
                         void       *buffer,
                         rt_size_t   size) /* 4.1.1 */

rt_ssize_t rt_device_write(rt_device_t dev,
                          rt_off_t    pos,
                          const void *buffer,
                          rt_size_t   size); /* 5.0.0 */
rt_size_t  rt_device_write(rt_device_t dev,
                          rt_off_t    pos,
                          const void *buffer,
                          rt_size_t   size); /* 4.1.1 */


/* ----- irq.c ----- */
rt_weak void rt_interrupt_enter(void) /* 5.0.0 */
        void rt_interrupt_enter(void) /* 4.1.1 */

rt_weak void rt_interrupt_leave(void) /* 5.0.0 */
        void rt_interrupt_leave(void) /* 4.1.1 */

rt_weak rt_uint8_t rt_interrupt_get_nest(void) /* 5.0.0 */
        rt_uint8_t rt_interrupt_get_nest(void) /* 4.1.1 */

```

## 4.1.1

相较 4.1.1-beta，内核 API 无更改。

## 4.1.1-beta

#### API 增加：

```c
/* ----- kservice.c ----- */
RT_WEAK void rt_hw_us_delay(rt_uint32_t us) /* 4.1.1-beta */
const char *rt_strerror(rt_err_t error)     /* 4.1.1-beta */
```

## 4.1.0

整理内存相关的文件以及 API，将用户使用的内存 API 统一放入 kservice.c 中。具体为：

用户在使用内存相关的 API 时，一般是 mem.c(small mem)，memheap.c 以及 slab.c 这三种管理算法三选一，这 3 个文件提供给用户的 API 是相同的。4.1.0 将这 3 个文件中提供给用户的 API 统一放入 kservice.c 中完成。

在 kservice.c 中提供内存管理算法 API（如 rt_system_heap_init, rt_malloc, rt_free 等），并加 RT_WEAK 修饰，以便用户选择外部内存管理算法。

#### API 增加：

```c
/* ----- clock.c ----- */
void rt_tick_sethook(void (*hook)(void)) /* 4.1.0 */


/* ----- kservice.c ----- */
char *rt_strcpy(char *dst, const char *src) /* 4.1.0 */
void rt_malloc_sethook(void (*hook)(void *ptr, rt_size_t size)) /* 4.1.0 */
void rt_free_sethook(void (*hook)(void *ptr)) /* 4.1.0 */
void *rt_page_alloc(rt_size_t npages) /* 4.1.0 */
void rt_page_free(void *addr, rt_size_t npages) /* 4.1.0 */


/* ----- memheap.c ----- */
void rt_memheap_info(struct rt_memheap *heap,
                     rt_size_t *total,
                     rt_size_t *used,
                     rt_size_t *max_used) /* 4.1.0 */
```

#### API 删除：

```c
/* ----- thread.c ----- */
void rt_thread_timeout(void *parameter) /* 4.1.0 delete*/

```

#### API 变更：

```c
/* ----- kservice.c ----- */
RT_WEAK void *rt_memcpy(void *dst, const void *src, rt_ubase_t count) /* 4.1.0 */
        void *rt_memcpy(void *dst, const void *src, rt_ubase_t count) /* 4.0.4 */

void *rt_memmove(void *dest, const void *src,  rt_size_t n) /* 4.1.0 */
void *rt_memmove(void *dest, const void *src, rt_ubase_t n) /* 4.0.4 */

        rt_int32_t rt_memcmp(const void *cs, const void *ct,  rt_size_t count) /* 4.1.0 */
RT_WEAK rt_int32_t rt_memcmp(const void *cs, const void *ct, rt_ubase_t count) /* 4.0.4 */

char *rt_strncpy(char *dst, const char *src,  rt_size_t n) /* 4.1.0 */
char *rt_strncpy(char *dst, const char *src, rt_ubase_t n) /* 4.0.4 */

rt_int32_t rt_strncmp(const char *cs, const char *ct,  rt_size_t count) /* 4.1.0 */
rt_int32_t rt_strncmp(const char *cs, const char *ct, rt_ubase_t count) /* 4.0.4 */

RT_WEAK int rt_vsnprintf(char *buf,
                        rt_size_t size,
                        const char *fmt,
                        va_list args) /* 4.1.0 */
rt_int32_t  rt_vsnprintf(char       *buf,
                        rt_size_t   size,
                        const char *fmt,
                        va_list     args) /* 4.0.4 */

       int rt_snprintf(char *buf, rt_size_t size, const char *fmt, ...) /* 4.1.0 */
rt_int32_t rt_snprintf(char *buf, rt_size_t size, const char *fmt, ...) /* 4.0.4 */

       int rt_vsprintf(char *buf, const char *format, va_list arg_ptr) /* 4.1.0 */
rt_int32_t rt_vsprintf(char *buf, const char *format, va_list arg_ptr) /* 4.0.4 */

       int rt_sprintf(char *buf, const char *format, ...) /* 4.1.0 */
rt_int32_t rt_sprintf(char *buf, const char *format, ...) /* 4.0.4 */

RT_WEAK  int rt_kprintf(const char *fmt, ...) /* 4.1.0 */
RT_WEAK void rt_kprintf(const char *fmt, ...) /* 4.0.4 */


/* ----- 以下内存相关 API 增加 RT_WEAK 修饰，以便用户使用外部内存管理算法 ----- */
RT_WEAK void  rt_system_heap_init(void *begin_addr, void *end_addr) /* 4.1.0 */
RT_WEAK void *rt_malloc(rt_size_t size) /* 4.1.0 */
RT_WEAK void *rt_realloc(void *rmem, rt_size_t newsize) /* 4.1.0 */
RT_WEAK void *rt_calloc(rt_size_t count, rt_size_t size) /* 4.1.0 */
RT_WEAK void  rt_free(void *rmem) /* 4.1.0 */
RT_WEAK void  rt_memory_info(rt_size_t *total,
                             rt_size_t *used,
                             rt_size_t *max_used) /* 4.1.0 */
```

## 4.0.4

#### API 增加：

```c
/* ----- idle.c ----- */
void rt_thread_defunct_enqueue(rt_thread_t thread) /* 4.0.4 */
rt_thread_t rt_thread_defunct_dequeue(void) /* 4.0.4 */


/* ----- ipc.c ----- */
rt_err_t rt_mutex_trytake(rt_mutex_t mutex) /* 4.0.4 */
rt_err_t rt_mb_urgent(rt_mailbox_t mb, rt_ubase_t value) /* 4.0.4 */


/* ----- scheduler.c ----- */
void rt_scheduler_switch_sethook(void (*hook)(struct rt_thread *tid)) /* 4.0.4 */
```

#### API 删除：

```c
/* ----- device.c ----- */
rt_err_t rt_device_init_all(void) /* 4.0.4 delete */
```

#### API 变更：

```c
/* ----- idle.c ----- */
/* 函数名变更，并使用 static 修饰 */
static void rt_defunct_execute(void)     /* 4.0.4 */
       void rt_thread_idle_excute(void)  /* 4.0.3 */


/* ----- kservice.c, 增加 RT_WEAK 修饰 ----- */
RT_WEAK void *rt_memset(void *s, int c, rt_ubase_t count) /* 4.0.4 */
        void *rt_memset(void *s, int c, rt_ubase_t count) /* 4.0.3 */

RT_WEAK rt_int32_t rt_memcmp(const void *cs, const void *ct, rt_ubase_t count) /* 4.0.4 */
        rt_int32_t rt_memcmp(const void *cs, const void *ct, rt_ubase_t count) /* 4.0.3 */

RT_WEAK void rt_kprintf(const char *fmt, ...) /* 4.0.4 */
        void rt_kprintf(const char *fmt, ...) /* 4.0.3 */

RT_WEAK void *rt_malloc_align(rt_size_t size, rt_size_t align) /* 4.0.4 */
        void *rt_malloc_align(rt_size_t size, rt_size_t align) /* 4.0.3 */

RT_WEAK void rt_free_align(void *ptr) /* 4.0.4 */
        void rt_free_align(void *ptr) /* 4.0.3 */


/* ----- thread.c ----- */
static void   _thread_exit(void) /* 4.0.4 */
       void rt_thread_exit(void) /* 4.0.3 */
```

## 4.0.3

#### API 增加：

```c
/* ----- clock.c ----- */
RT_WEAK rt_tick_t rt_tick_get_millisecond(void) /* 4.0.3 */


/* ----- memheap.c ----- */
void rt_memory_info(rt_uint32_t *total,
                    rt_uint32_t *used,
                    rt_uint32_t *max_used) /* 4.0.3 */


/* ----- object.c ----- */
int rt_object_get_length(enum rt_object_class_type type) /* 4.0.3 */
int rt_object_get_pointers(enum rt_object_class_type type, rt_object_t *pointers, int maxlen) /* 4.0.3 */
```

#### API 删除：

```c
/* ----- clock.c ----- */
void rt_system_tick_init(void) /* 4.0.3 delete */


/* ----- object.c ----- */
void rt_system_object_init(void) /* 4.0.3 delete */
```

#### API 变更：

```c
/* ----- kservice.c ----- */
rt_int32_t  rt_strcasecmp(const char *a, const char *b)  /* 4.0.3 */
rt_uint32_t rt_strcasecmp(const char *a, const char *b)  /* 4.0.2 */
```

## 4.0.2

#### API 增加：

```c
/* ----- ipc.c ----- */
rt_err_t rt_mq_send_wait(rt_mq_t     mq,
                         const void *buffer,
                         rt_size_t   size,
                         rt_int32_t  timeout) /* 4.0.2 */


/* ----- thread.c ----- */
rt_err_t rt_thread_delay_until(rt_tick_t *tick, rt_tick_t inc_tick)
```

#### API 变更：

```c
/* ----- ipc.c ----- */
rt_err_t rt_mq_urgent(rt_mq_t mq, const void *buffer, rt_size_t size) /* 4.0.2 */
rt_err_t rt_mq_urgent(rt_mq_t mq,       void *buffer, rt_size_t size) /* 4.0.1 */


/* ----- irq.c ----- */
RT_WEAK rt_uint8_t rt_interrupt_get_nest(void) /* 4.0.2 */
        rt_uint8_t rt_interrupt_get_nest(void) /* 4.0.1 */
```

## 4.0.1

#### API 变更：

```c
/* ----- clock.c ----- */
rt_tick_t rt_tick_from_millisecond(rt_int32_t ms) /* 4.0.1 */
      int rt_tick_from_millisecond(rt_int32_t ms) /* 4.0.0 */
```

## 4.0.0

#### API 变更：

```c
/* ----- ipc.c ----- */
rt_err_t rt_mb_send(rt_mailbox_t mb,  rt_ubase_t value)  /* 4.0.0 */
rt_err_t rt_mb_send(rt_mailbox_t mb, rt_uint32_t value) /* 3.1.1 */

rt_err_t rt_mb_recv(rt_mailbox_t mb,  rt_ubase_t *value, rt_int32_t timeout) /* 4.0.0 */
rt_err_t rt_mb_recv(rt_mailbox_t mb, rt_uint32_t *value, rt_int32_t timeout) /* 3.1.1 */


/* ----- memheap.c ----- */
rt_err_t rt_memheap_init(struct rt_memheap *memheap,
                         const char        *name,
                         void              *start_addr,
                         rt_size_t         size) /* 4.0.0 */
rt_err_t rt_memheap_init(struct rt_memheap *memheap,
                         const char        *name,
                         void              *start_addr,
                         rt_uint32_t        size) /* 3.1.1 */

void *rt_memheap_alloc(struct rt_memheap *heap,  rt_size_t size)  /* 4.0.0 */
void *rt_memheap_alloc(struct rt_memheap *heap, rt_uint32_t size) /* 3.1.1 */
```

## 3.1.1

点击查看 [V3.1.1 版本的 API 参考手册](https://www.rt-thread.org/document/api/)

