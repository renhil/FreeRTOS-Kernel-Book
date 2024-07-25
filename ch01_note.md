# 1 Preface

## 1.1 Multitasking in Small Embedded Systems

### 1.1.1 About the FreeRTOS Kernel

FreeRTOS 内核非常适合在微控制器或小型微处理器上运行的深度嵌入式实时应用程序。
此类应用程序通常同时包含硬实时和软实时要求。

软实时要求规定了时间期限——但超出期限并不会使系统变得无用。例如，响应按键太慢可能会使系统看起来反应迟钝，但实际上并不会使系统无法使用。

硬实时要求规定了时间期限——违反期限将导致系统彻底失效。例如，如果驾驶员安全气囊对碰撞传感器输入的响应速度过慢，则可能弊大于利。

FreeRTOS 内核是一个实时内核（或实时调度程序），它使基于 FreeRTOS 构建的应用程序能够满足其硬实时要求。
它使应用程序能够组织为一组独立的执行线程。例如，在只有一个核心的处理器上，一次只能执行一个执行线程。
内核通过检查应用程序设计者为每个线程分配的优先级来决定执行哪个线程。
在最简单的情况下，应用程序设计者可以为实现硬实时要求的线程分配较高的优先级，为实现软实时要求的线程分配较低的优先级。
以这种方式分配优先级将确保硬实时线程始终先于软实时线程执行，但优先级分配决策并不总是那么简单。

FreeRTOS is a collection of C libraries comprised of a real-time kernel
and a set of modular libraries that implement complementary
functionality.

Richard Barry originally developed FreeRTOS around 2003. Real-Time
Engineers Ltd, Richard's company, continued FreeRTOS development in
close partnership with the world's leading chip companies until Amazon
Web Services (AWS) took stewardship of FreeRTOS 2016. Richard now
continues his work on FreeRTOS as a senior principal engineer within the
AWS IoT team.  FreeRTOS is MIT licensed open source code, available for
any purpose.  You don't have to be an AWS customer to benefit from AWS's
stewardship!

The FreeRTOS kernel is ideally suited to deeply embedded real-time
applications that run on microcontrollers or small microprocessors. This
type of application typically includes a mix of both hard and soft
real-time requirements.

Soft real-time requirements state a time deadline—but breaching the
deadline would not render the system useless. For example, responding to
keystrokes too slowly might make a system seem annoyingly unresponsive
without actually making it unusable.

Hard real-time requirements state a time deadline—and breaching the
deadline would result in absolute failure of the system. For example, a
driver's airbag has the potential to do more harm than good if it
responded to crash sensor inputs too slowly.

The FreeRTOS kernel is a real-time kernel (or real-time scheduler) that
enables applications built on FreeRTOS to meet their hard real-time
requirements. It enables applications to be organized as a collection of
independent threads of execution. For example, on a processor that has
only one core, only a single thread of execution can execute at any
one time. The kernel decides which thread to execute by examining the
priority assigned to each thread by the application designer. In the
simplest case, the application designer could assign higher priorities
to threads that implement hard real-time requirements and lower
priorities to threads that implement soft real-time requirements.
Allocating priorities in that way would ensure hard real-time threads
always execute ahead of soft real-time threads, but priority assignment
decisions are not always that simplistic.

Do not be concerned if you do not fully understand the concepts in the
previous paragraph yet. The following chapters provide a detailed
explanation, with many examples, to help you understand how to use a
real-time kernel, and FreeRTOS in particular.

### 1.1.2 Value Proposition

FreeRTOS 内核在全球取得前所未有的成功，这得益于其令人信服的价值主张；
FreeRTOS 是专业开发的，质量控制严格，功能强大，支持良好，不存在任何知识产权所有权模糊性，
并且可以在商业应用中真正免费使用，而无需公开您的专有源代码。
此外，AWS 的管理提供了全球影响力、专业的安全事件响应程序、庞大而多样化的开发团队、形式验证、
渗透测试、内存安全证明方面的专业知识以及长期支持——同时将 FreeRTOS 保持为硬件、
开发工具和云服务中立的开源项目。
FreeRTOS 开发在 GitHub 中是透明的且由社区驱动，不需要任何特殊工具或开发实践。

您可以使用 FreeRTOS 将产品推向市场，甚至无需告诉我们，更不用说支付任何费用，成千上万的公司都是这样做的。


The unprecedented global success of the FreeRTOS kernel comes from its
compelling value proposition; FreeRTOS is professionally developed,
strictly quality controlled, robust, supported, does not contain any
intellectual property ownership ambiguity, and is truly free to use in
commercial applications without any requirement to expose your
proprietary source code. Further, AWS's stewardship provides a global
presence, expert security event response procedures, a large and diverse
development team, expertise in formal verification, pen testing, memory
safety proofs, and long-term
support – all while maintaining FreeRTOS as a hardware, development
tool, and cloud service-neutral open-source project. FreeRTOS
development is transparent and community-driven in GitHub, and does not
require any special tools or development practices.

You can take a product to market using FreeRTOS without even telling us,
let alone paying any fees, and thousands of companies do just that. If
at any time you would like to receive additional backup, or if your
legal team requires additional written guarantees or indemnification,
then our strategic partners provide simple low-cost commercial license
options. Peace of mind comes with the knowledge that you can opt to take
the commercial route whenever you choose.


### 1.1.3 A Note About Terminology

在 FreeRTOS 中，每个执行线程都称为“任务”。


In FreeRTOS, each thread of execution is called a 'task'. There is no
consensus on terminology within the embedded community, but I prefer
'task' to 'thread,' as thread can have a more specific meaning in some
fields of application.


### 1.1.4 Why Use an RTOS?

任务优先级有助于确保应用程序满足其处理期限，内核还可以带来其他不太明显的好处。
抽象出时间信息
RTOS 负责执行计时，并为应用程序提供与时间相关的 API。
可维护性/可扩展性
抽象出时间细节可以减少模块之间的相互依赖性，并允许软件以受控且可预测的方式发展。
此外，内核负责时间，因此应用程序性能不太容易受到底层硬件变化的影响。
模块化
任务是独立的模块，每个模块都应该有明确的目的。
团队发展
任务还应该具有明确定义的接口，以便于团队开发。
更容易测试
具有清晰接口、定义明确的独立模块的任务更容易进行单独测试。
代码重用
模块化程度更高、相互依赖性更少的代码设计更易于重用。
提高效率
使用 RTOS 的应用程序代码可以完全由事件驱动。无需浪费处理时间来轮询尚未发生的事件。
与事件驱动带来的效率相​​反的是，需要处理 RTOS 时钟中断并将执行从一个任务切换到另一个任务。
但是，不使用 RTOS 的应用程序通常包含某种形式的时钟中断。
空闲时间
当没有需要处理的应用程序任务时，将自动创建空闲任务。
空闲任务可以测量空闲处理能力、执行背景检查或将处理器置于低功耗模式。
能源管理
使用 RTOS 所带来的效率提升使得处理器能够在低功耗模式下花费更多时间。
每次运行空闲任务时将处理器置于低功耗状态，可以显著降低功耗。
FreeRTOS 还具有特殊的无滴答模式。
使用无滴答模式可使处理器进入比其他方式更低的功耗模式，并更长时间地保持低功耗模式。
灵活的中断处理
通过将处理推迟到应用程序编写者创建的任务或自动创建的 RTOS 守护程序任务（也称为计时器任务），
可以使中断处理程序保持非常短。
混合处理要求
简单的设计模式可以在应用程序中实现周期性、连续性和事件驱动处理的混合。
此外，通过选择适当的任务和中断优先级，可以满足硬实时和软实时要求。


There are many well-established techniques for writing good embedded
software without using a multithreading kernel. If the system under
development is simple, then these techniques might provide the most
appropriate solution. Using a kernel would likely be preferable in more
complex cases, but where the crossover point occurs will always be
subjective.

As already described, task prioritization can help ensure an application
meets its processing deadlines, but a kernel can bring other less
obvious benefits. Some of these are listed very briefly below.

- Abstracting away timing information

  The RTOS is responsible for execution timing and provides a time-related
  API to the application. That allows the structure of the application
  code to be more straightforward and the overall code size to be smaller.

- Maintainability/Extensibility

  Abstracting away timing details results in fewer interdependencies
  between modules and allows the software to evolve in a controlled and
  predictable way. Also, the kernel is responsible for the timing, so
  application performance is less susceptible to changes in the underlying
  hardware.

- Modularity

  Tasks are independent modules, each of which should have a well-defined
  purpose.

- Team development

  Tasks should also have well-defined interfaces, allowing easier
  team development.

- Easier testing

  Tasks that are well-defined independent modules with clean interfaces
  are easier to test in isolation.

- Code reuse

  Code designed with greater modularity and fewer interdependencies is
  easier to reuse.

- Improved efficiency

  Application code that uses an RTOS can be completely event-driven. No
  processing time needs to be wasted by polling for events that
  have not occurred.

  Countering the efficiency gained from being event driven is the need to process the RTOS tick
  interrupt and switch execution from one task to another. However,
  applications that don't use an RTOS normally include some form of tick
  interrupt anyway.

- Idle time

  The automatically created Idle task executes when there are no
  application tasks that require processing. The Idle task can measure
  spare processing capacity, perform background checks, or place the
  processor into a low-power mode.

- Power Management

  The efficiency gains that result from using an RTOS allow the processor to
  spend more time in a low power mode.

  Power consumption can be decreased significantly by placing the
  processor into a low power state each time the Idle task runs. FreeRTOS
  also has a special tick-less mode. Using the tick-less mode allows the
  processor to enter a lower power mode than would otherwise be possible
  and remain in the low power mode for longer.

- Flexible interrupt handling

  Interrupt handlers can be kept very short by deferring processing to
  either a task created by the application writer or the automatically
  created RTOS daemon task (also known as the timer task).

- Mixed processing requirements

  Simple design patterns can achieve a mix of periodic, continuous, and
  event-driven processing within an application. In addition, hard and
  soft real-time requirements can be met by selecting appropriate task and
  interrupt priorities.


### 1.1.5 FreeRTOS Kernel Features

FreeRTOS 内核具有以下标准特性：
先发制人或合作行动
可选时间分片
非常灵活的任务优先级分配
灵活、快速、轻量的任务通知机制
队列
二进制信号量
计数信号量
互斥锁
递归互斥锁
软件计时器
事件组
流缓冲区
消息缓冲区
协同例程（已弃用）
勾选钩子函数
空闲钩子函数
堆栈溢出检查
跟踪宏
任务运行时统计信息收集


The FreeRTOS kernel has the following standard features:

- Pre-emptive or co-operative operation
- Optional time-slicing
- Very flexible task priority assignment
- Flexible, fast and light-weight task notification mechanisms
- Queues
- Binary semaphores
- Counting semaphores
- Mutexes
- Recursive mutexes
- Software timers
- Event groups
- Stream buffers
- Message buffers
- Co-routines (deprecated)
- Tick hook functions
- Idle hook functions
- Stack overflow checking
- Trace macros
- Task run-time statistics gathering
- Optional commercial licensing and support
- Full interrupt nesting model (for some architectures)
- A tick-less capability for extreme low power applications (for some architectures)
- Memory Protection Unit support for isolating tasks and increasing application safety (for some architectures)
- Software managed interrupt stack when appropriate (this can help save RAM)
- The ability to create RTOS objects using either statically or
  dynamically allocated memory


### 1.1.6 Licensing, and The FreeRTOS, OpenRTOS, and SafeRTOS Family




The **FreeRTOS** MIT open source license is designed to ensure:

- FreeRTOS can be used in commercial applications.

- FreeRTOS itself remains freely available to everybody.

- FreeRTOS users retain ownership of their intellectual property.

See <https://www.FreeRTOS.org/license> for the latest open source
license information.

**OpenRTOS** is a commercially licensed version of FreeRTOS provided
under license from Amazon Web Services by a third party.

**SafeRTOS** shares the same usage model as FreeRTOS, but has been
developed in accordance with the practices, procedures, and processes
necessary to claim compliance with various internationally recognized
safety related standards.


## 1.2 Included Source Files and Projects

### 1.2.1 Obtaining the Examples that Accompany this Book

The zip file available for download from <https://www.FreeRTOS.org/Documentation/code>
contains all the source code, pre-configured project files, and
instructions necessary to build and execute the examples presented in
this book. Note the zip file will not necessarily contain the most
recent version of FreeRTOS.

The screenshots included in this book show the examples executing in a
Microsoft Windows environment, using the FreeRTOS Windows port. The
project that uses the FreeRTOS Windows port is pre-configured to build
using the free Community edition of Visual Studio, available from
<https://www.visualstudio.com/>. Note that while the FreeRTOS Windows
port provides a convenient evaluation, test, and development platform,
it does *not* provide true real-time behavior.

