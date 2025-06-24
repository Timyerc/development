# VSCode用CMSIS_Configuration_Wizard头文件向导

胡洋悦（yangyue.hu@ovobot.cc）

翻译整理自[https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/configWizard.html](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/configWizard.html)

## Configuration_Wizard使用规则

配置向导注释由注释项和注释修饰符组成。它们为配置文件在IDE中创建类似GUI的元素。使用类似GUI的方式可以使用户更容易检查和调整配置文件以满足应用需求。以下是注释规则的详细描述：

* 配置向导部分必须从前 100 行代码开始，并且必须以以下注释行开头：
    ```
        // <<< Use Configuration Wizard in Context Menu >>>
    ```
* 配置向导部分可以以以下可选注释结束：
    ```
        // <<< end of configuration section >>>
    ```

* 注释以代码中的注释形式编写。每一行注释必须以双斜杠（//）开头
* 默认情况下，紧随注释之后的下一个代码符号将被修改
* 表中标有 * 的项目可以跟随一个跳过值。跳过值会忽略一定数量的代码符号（参见表中的跳过示例）。此规则覆盖前一条规则。
* 表中标有 + 的项目可以跟随一个标识符。当存在标识符时，紧随与标识符匹配的符号之后的下一个代码符号将被修改。参见表中的标识符示例。标识符不能与跳过值一起使用
* 可以为项目添加描述性文本。该文本将显示在屏幕上（参见表）
* 注释项或注释修饰符中的空白字符将被忽略（文本除外）
* 在配置向导行中，除非用于包含注释项，否则不能使用 < 或 > 符号
* 下表列出了配置向导的注释项目、修饰符及其对应的功能：

| **注释项** | **文本** | **描述** |
| ---------|-------| ----- |
|  \<h> |	是	|标题。创建一个标题部分。所有被 \<h> 和 \</h> 包含的项目和选项属于一个组，可以展开。此条目不修改代码符号，只用于分组其他项目和修饰符<pre><code>// &lt;h&gt;Thread Configuration -- header without checkbox to group other items<br>// ...<br>// &lt;/h&gt;</code></pre> |
| \<e>*+ |	是|	启用的标题。创建一个带复选框的标题部分，复选框用于启用或禁用被 \<e> 和 \</e> 包含的所有项目和选项。 <pre><code>// &lt;e&gt;Round-Robin Thread switching                       -- header with checkbox<br>// ===============================<br>//<br><br>// &lt;i&gt; Enables Round-Robin Thread switching.             -- tooltip information for the header<br>#ifndef OS_ROBIN<br>#define OS_ROBIN 1                                       -- this value is set through the checkbox<br>#endif<br>// &lt;o&gt;Round-Robin Timeout [ticks] &lt;1-1000&gt;<br>// &lt;i&gt; Defines how long a thread will execute before a thread switch.<br>// &lt;i&gt; Default: 5<br>#ifndef OS_ROBINTOUT<br>#define OS_ROBINTOUT 5<br>#endif<br>// &lt;/e&gt;</code></pre>|
| \<e.i>*+ |	是 |	启用特定位(i)的标题（例如：<e.4> - 更改值的第4位）。<pre><code>// &lt;e.4&gt;Serial Number<br>// &lt;i&gt;Enable Serial Number String.<br>// &lt;i&gt;If disabled, Serial Number String will not be assigned to USB Device.<br>#define USBD0_STR_DESC_SER_EN           1</code></pre> |
| \</h> , \</e>, 或 \</c> |	是	| 标题、启用或注释的结束标记。|
| \<n> |	是 |	显示的通知文本。 <pre><code>// &lt;n&gt; This is shown as plain text</code></pre>|
| \<i> |	是 |	提示帮助信息，显示在悬停时。<pre><code>// <i>This is shown as a tooltip when hovering over a text.</i></code></pre>|
| \<c>* | 是	| 启用代码：创建复选框，用于取消或注释代码。当复选框被禁用时，所有行都将用双斜杠（//）注释。<pre><code>// &lt;c1&gt; Comment sequence block until block end when disabled<br>//&lt;i&gt; This may carry the block's description<br>foo<br>+bar<br>-xFoo<br>// &lt;/c&gt;</code></pre> |
| \<!c>*	| 是 |	禁用代码：创建复选框，用于注释或取消注释代码。 <pre><code>// &lt;!c1&gt; Comment sequence block until block end when enabled<br>//&lt;i&gt; This may carry the block's description<br><br>foo<br><br>+bar<br><br>-xFoo<br>// &lt;/c&gt;</code></pre> |
| \<q>*+	| 是 |	位值选项，可通过复选框设置。<pre><code>//  &lt;h&gt; Chip-select control<br>//     &lt;q&gt; ASYNCWAIT: Wait signal during asynchronous transfer<br>//      &lt;i&gt; Enables the FSMC to use the wait signal even during an asynchronous protocol.<br>// &lt;/h&gt;<br>#define RTE_FSMC_BCR1_ASYNCWAIT         0                             -- this is changed via a checkbox</code></pre> |
| \<o>*+ |	是 |	带有选择或数字输入的选项。<pre><code>// &lt;o&gt;Round-Robin Timeout [ticks] &lt;1-1000&gt;                             -- text displayed on screen. Range of [ticks] is [1..1000]<br>// &lt;i&gt; Defines how long a thread will execute before a thread switch.  -- tooltip info<br>// &lt;i&gt; Default: 5                                                      -- tooltip info. Both displayed in one tooltip.<br>#ifndef OS_ROBINTOUT<br>#define OS_ROBINTOUT 5<br>#endif<br>// &lt;/e&gt;</code></pre> |
| \<o.i>*+ |	是 |	修改单个位（例如：\<e.4> - 修改第4位）。<pre><code>// &lt;o.4&gt; &lt;o.0&gt;High-speed<br>//   &lt;i&gt;Enable High-speed functionality (if device supports it).<br>#define USBD0_HS</code></pre>|
| \<o.x..y>*+ |	是 |	修改位范围（例如：\<o.4..5> - 修改第4到5位）。<pre><code>//   &lt;h&gt;String Settings<br>//   &lt;i&gt;These settings are used to create the String Descriptor.<br>//     &lt;o.0..15&gt;Language ID &lt;0x0000-0xFCFF&gt;<br>//     &lt;i&gt;English (United States) = 0x0409.<br>//   &lt;/h&gt;<br>#define USBD0_STR_DESC_LANGID           0x0409</code></pre>|
| \<s>*+ |	是 |	带有ASCII字符串输入的选项。 <pre><code>//  &lt;s&gt;Manufacturer String<br>//  &lt;i&gt;String Descriptor describing Manufacturer.<br>#define USBD0_STR_DESC_MAN              L"Keil Software"</code></pre>|
| \<s.i>*+ |	是 |	带有字符限制的ASCII字符串输入选项。<pre><code>//  &lt;s.126&gt;Manufacturer String<br>//  &lt;i&gt;String Descriptor describing Manufacturer.<br>#define USBD0_STR_DESC_MAN              L"Keil Software"</code></pre> |
| **注释修饰符** |  | **描述** |
| \<0-31> |	否 |	表示选项字段的值范围|
| \<0-100:10> |	否 |	表示选项字段的值范围，步长为 10|
| \<0x40-0x1000:0x10> |	否 |	表示十六进制格式的值范围，步长为 10|
| \<value=> | 是 |	创建下拉列表并显示文本，值写入下一个项目。<pre><code>//   &lt;o&gt;Timer Thread Priority                                            -- creates a drop-down with the list below.<br>//                        &lt;1=&gt; Low<br>//     &lt;2=&gt; Below Normal  &lt;3=&gt; Normal  &lt;4=&gt; Above Normal<br>//                        &lt;5=&gt; High<br>//                        &lt;6=&gt; Realtime (highest)<br>//   &lt;i&gt; Defines priority for Timer Thread                               -- tooltip info<br>//   &lt;i&gt; Default: High                                                   -- tooltip info<br>#ifndef OS_TIMERPRIO<br>#define OS_TIMERPRIO   5<br>#endif</code></pre>|
| \<identifier=> | 是 |	创建下拉列表并显示根据标识符（如 dwt, systick, user）定义的文本。只能在 <o key-identifier> 上下文中使用！选中的文本对应的标识符将替换 <o ...> 标签中的标识符。<pre><code>//   &lt;o TIMESTAMP_SRC&gt;Time Stamp Source<br>//      &lt;dwt=&gt;     DWT Cycle Counter<br>//      &lt;systick=&gt; SysTick<br>//      &lt;user=&gt;    User Timer <br>//   &lt;i&gt;Selects source for 32-bit time stamp<br>#define TIMESTAMP_SRC  dwt</code></pre>|
| \<#+1>   <#-1> <#*8>   <#/3> |	否 |修改输入或显示的值，使用操作符（加、减、乘、除）。修改后的值将被赋给代码符号。<pre><code>// &lt;o&gt;Default Thread stack size [bytes] &lt;64-4096:8&gt;&lt;#/4&gt;<br>// &lt;i&gt; Defines default stack size for threads with osThreadDef stacksz = 0<br>// &lt;i&gt; Default: 200<br>#ifndef OS_STKSIZE<br>#define OS_STKSIZE 50<br>#endif</code></pre> |


## 示例代码

* 你可以将代码复制到 C 文件中，并在 uVision 编辑器中查看结果
```
// <<< Use Configuration Wizard in Context Menu >>>

// <h> 产测配置
// <e> 产测模式开关(SUPPORT_FACTORY)
#define SUPPORT_FACTORY 1
//<o>FACTORY_MODE_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define FACTORY_MODE_PIN PA13
// </e>

// <c> 串口2开关(USE_UART2)
#define USE_UART2
// </c>
// </h>

// <h>遥控器配置
// <c> 同一遥控器指令继续执行不复位(USE_AUTO_KEEP_RUN_WITH_SAME_CMD)
#define USE_AUTO_KEEP_RUN_WITH_SAME_CMD
// </c>

// <e> 2.4G选择低码率(RF_LOW_BAUDRATE)
#define RF_LOW_BAUDRATE 1
// </e>

//<h> 遥控器键码配置
//<o> 码头(RF_DATA_HEAD) <0x00-0xFF>
#define RF_DATA_HEAD 0xAA

//<o> 码尾(RF_DATA_TAIL) <0x00-0xFF>
#define RF_DATA_TAIL 0xFF

//<o> 停止(STOP_C) <0x00-0xFF>
#define STOP_C    0x03

//<o> 手动向上(UP_C) <0x00-0xFF>
#define UP_C      0x05

//<o> 手动向下(DOWN_C) <0x00-0xFF>
#define DOWN_C    0x18

//<o> 手动向左(LEFT_C) <0x00-0xFF>
#define LEFT_C    0x07

//<o> 手动向右(RIGHT_C) <0x00-0xFF>
#define RIGHT_C   0x09

//<o> 一键启动(SMART_C) <0x00-0xFF>
#define SMART_C   0x08

//<o> 自动向上(AUTOUP_C) <0x00-0xFF>
#define AUTOUP_C  0x12

//<o> 自动向左(AUTOLF_C) <0x00-0xFF>
#define AUTOLF_C  0x1A

//<o> 自动向右(AUTORT_C) <0x00-0xFF>
#define AUTORT_C  0x1E

//<o> 手动喷水(HAND_SPRAY_C) <0x00-0xFF>
#define HAND_SPRAY_C   0x02

//<o> 喷水模式切换(SPRAY_SWITCH_C) <0x00-0xFF>
#define SPRAY_SWITCH_C 0x01

//<o> 产测(FACTORY_C) <0x00-0xFF>
#define FACTORY_C 0x88

//<o> 没有功能(NONE_C) <0x00-0xFF>
#define NONE_C  0x00

//<o> BTN_A_AUTO <0x00-0xFF>
#define BTN_A_AUTO     0x22

//<o> BTN_A_MANU <0x00-0xFF>
#define BTN_A_MANU     0x23

//<o> BTN_B_PRESSED <0x00-0xFF>
#define BTN_B_PRESSED  0x20

//<o> BTN_B_RELEASE <0x00-0xFF>
#define BTN_B_RELEASE  0x21

//</h>

// <o> SPI_CS_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define SPI_CS_PIN PD0

// <o>LT8900_RESET_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define LT8900_RESET_PIN PD1
// </h>

//<e> 喷水配置(WATER_PUMP)
#define WATER_PUMP 1

//<h> 喷水驱动配置
//<h> 喷水引脚配置
// <o> SPRAY_PWM_LEFT_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define SPRAY_PWM_LEFT_PIN PA3

// <o> SPRAY_PWM_RIGHT_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define SPRAY_PWM_RIGHT_PIN PA2

//<o> 使用雾化片的个数(USABLE_SPRAY_CHANNEL_COUNT) <0-4>
#define USABLE_SPRAY_CHANNEL_COUNT 2

// <c> 左雾化片喷水开关(USE_LEFT_PUMP)
#define USE_LEFT_PUMP
// </c>

//</h>

//<h> 雾化片驱动参数配置

//<e> 使用PWM驱动雾化片开关(SPRAY_PWM)
#define SPRAY_PWM 1
//</e>

//<o> 雾化片驱动频率(SPRAY_FREQ) <2-300000>
#define SPRAY_FREQ              160000 // 160K

//<o> 雾化片定时器分频(SPRAY_TIM_PRESCALER) <0-100>
#define SPRAY_TIM_PRESCALER     3 // timer prescaler

//</h>

//</h>

#define SPRAY_TIM_PERIOD        (48000000 / (SPRAY_TIM_PRESCALER + 1) / SPRAY_FREQ - 1) // timer period
#define SPRAY_PWM_DUTY          (SPRAY_TIM_PERIOD / 2)

//<h>向上运动喷水配置
// <c> 向上运动喷水开关(USE_UP_PUMP_INDEPENDENT)
//#define USE_UP_PUMP_INDEPENDENT
// </c>

//<o> 向上运动喷水起始角度(WATER_PUMP_UP_START_ANGLE) <0-30>
#define WATER_PUMP_UP_START_ANGLE 5

//<o> 向上运动喷水结束角度(WATER_PUMP_UP_END_ANGLE) <0-30>
#define WATER_PUMP_UP_END_ANGLE 20

//<o> 向上运动喷水移动间隔(WATER_PUMP_UP_MOVE_CNT) <0-3>
#define WATER_PUMP_UP_MOVE_CNT 2

//<o> 向上运动第一次喷水时间(WATER_PUMP_UP_FIRST_DURATION) <0-500>
#define WATER_PUMP_UP_FIRST_DURATION 450

//<o> 向上运动喷水时间(WATER_PUMP_UP_DURATION) <0-500>
#define WATER_PUMP_UP_DURATION 150
//</h>

//<h> 喷水起始角度、结束角度、喷水间隔配置
//<o> 喷水起始角度(WATER_PUMP_START_ANGLE) <0-30>
#define WATER_PUMP_START_ANGLE 5

//<o> 喷水结束角度(WATER_PUMP_END_ANGLE) <0-30>
#define WATER_PUMP_END_ANGLE 20

//<o> 喷水机器移动间隔(WATER_PUMP_MOVE_CNT) <1-3>
#define WATER_PUMP_MOVE_CNT 2
//</h>

//<o> 喷水时间(WATER_PUMP_DURATION) <0-500>
#define WATER_PUMP_DURATION 150

//<o> 自动喷水等级(WATER_PUMP_INTERVAL_GRADE) <0-4>
#define WATER_PUMP_INTERVAL_GRADE 1

//<e> 开机默认自动喷水(USE_AUTO_SPRAY_DEFAULT)
#define USE_AUTO_SPRAY_DEFAULT     1
//</e>

#define WATER_PUMP_BUTTON_TYPE_A 1 //A键切换喷水模式，自动和手动切换，B键按下手动喷水，松开停止喷水
#define WATER_PUMP_BUTTON_TYPE_B 2 //A键切换到自动喷水模式，B键切换到手动喷水模式并持续喷水300ms

// <o> 按键切换喷水模式(WATER_PUMP_BUTTON_TYPE)
//<WATER_PUMP_BUTTON_TYPE_A=> A键切换喷水模式，自动和手动切换，B键按下手动喷水，松开停止喷水
//<WATER_PUMP_BUTTON_TYPE_B=> A键切换到自动喷水模式，B键切换到手动喷水模式并持续喷水300ms
#define WATER_PUMP_BUTTON_TYPE WATER_PUMP_BUTTON_TYPE_A

#define WATER_PUMP_AUTO_MODE_COUNT 1
#define WATER_PUMP_AUTO_MODE_2MOVE 2
#define WATER_PUMP_AUTO_MODE_2SPRAY 3
//</c>

//<o>  喷水模式选择(WATER_PUMP_AUTO_MODE)
//<WATER_PUMP_AUTO_MODE_COUNT=> 间隔一定时间喷水
//<WATER_PUMP_AUTO_MODE_2MOVE=> 向右移动两次喷一次
//<WATER_PUMP_AUTO_MODE_2SPRAY=> 顺时针和逆时针都喷水
#define WATER_PUMP_AUTO_MODE WATER_PUMP_AUTO_MODE_2SPRAY

// <c> 切换到自动喷水模式是蜂鸣器短响两声(AUTO_SPRAY_BUZZER_SHORT_2)
#define AUTO_SPRAY_BUZZER_SHORT_2
// </c>

// <c> 任何模式下都能使用手动喷水(USE_MANUAL_SPRAY_ALWAYS)
#define USE_MANUAL_SPRAY_ALWAYS
// </c>

//</e>

//<e>语音(VOICE_SUPPORT)
#define VOICE_SUPPORT 0 // 

// <o> VOICE_SET_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define VOICE_SET_PIN PC15

//<h> 语音词条配置
//<o> 静音(VOICE_SILENCE) <0-100>
#define VOICE_SILENCE 0

//<o> 欢迎语音(VOICE_WELCOME) <0-100>
#define VOICE_WELCOME 16

//<o> 自动向上(CLEAN_AUTO_UP) <0-100>
#define CLEAN_AUTO_UP 1

//<o> 自动向左(CLEAN_AUTO_LEFT) <0-100>
#define CLEAN_AUTO_LEFT 2

//<o> 自动向右(CLEAN_AUTO_RIGHT) <0-100>
#define CLEAN_AUTO_RIGHT 3

//<o> 清洁完成(CLEAN_FINISH) <0-100>
#define CLEAN_FINISH 4

//<o> 开启喷水(CLEAN_SPRAY_ON) <0-100>
#define CLEAN_SPRAY_ON 5

//<o> 关闭喷水(CLEAN_SPRAY_OFF) <0-100>
#define CLEAN_SPRAY_OFF 6

//<o> 增加喷水(CLEAN_SPRAY_ADD) <0-100>
#define CLEAN_SPRAY_ADD 7

//<o> 手动向上(CLEAN_UP) <0-100>
#define CLEAN_UP 8

//<o> 手动向下(CLEAN_DOWN) <0-100>
#define CLEAN_DOWN 9

//<o> 手动向左(CLEAN_LEFT0) <0-100>
#define CLEAN_LEFT 10

//<o> 手动向右(CLEAN_RIGHT) <0-100>
#define CLEAN_RIGHT 11

//<o> 开始清洁(CLEAN_START) <0-100>
#define CLEAN_START 12

//<o> 暂停清洁(CLEAN_PAUSE) <0-100>
#define CLEAN_PAUSE 13

//<o> 结束清洁(CLEAN_STOP) <0-100>
#define CLEAN_STOP 14

//<o> 词条个数(VOICE_LIST_NUM) <0-100>
#define VOICE_LIST_NUM 37
//</h>
//</e>

//<h>指示灯
//<e> 使用电源指示灯开关(USE_POWER_LED)
#define USE_POWER_LED 1
#if USE_POWER_LED
// <o> POWER_LED_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define POWER_LED_PIN PA7
#endif
//</e>

//<e> 使用充电指示灯开关(USE_CHARGE_LED)
#define USE_CHARGE_LED 1
#if USE_CHARGE_LED
// <o> CHARGE_LED_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define CHARGE_LED_PIN PB13

// <c> 软件关闭充电指示灯(USE_CHARGE_LED_SOFT_LAUNCH_OFF)
#define USE_CHARGE_LED_SOFT_LAUNCH_OFF
// </c>

#endif
//</e>

//<e> 使用报警指示灯开关(USE_ERROR_LED)
#define USE_ERROR_LED 1
#if USE_ERROR_LED
// <o>ERROR_LED_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define ERROR_LED_PIN PB11

// <o> 报警指示灯默认电平(ERROR_LED_ACTIVE_LEVEL) <0-1>
#define ERROR_LED_ACTIVE_LEVEL 1

#endif
//</e>

//<e> 使用状态指示灯开关(USE_STATUS_LED)
#define USE_STATUS_LED 1
#if USE_STATUS_LED
// <o> STATUS_LED_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define STATUS_LED_PIN PB12

// <o> 状态指示灯默认电平(STATUS_LED_ACTIVE_LEVEL） <0-1>
#define STATUS_LED_ACTIVE_LEVEL 1

#endif
//</e>

//</h>

//<h>蜂鸣器
// <o> BELL_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define BELL_PIN PB2
//<e> 使用beeper蜂鸣器驱动方式(USE_BEEPER)
#define USE_BEEPER 0
//</e>
//</h>

//<h>按键
//<e> 使用按键开关(USE_KEY)
#define USE_KEY 1
#if USE_KEY
// <o> START_BTN_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define START_BTN_PIN PC13
#endif
//</e>
//<o> 一键启动有效电平(START_BTN_ACTIVE_LEVEL) <0-1>
#define START_BTN_ACTIVE_LEVEL 0

// <e> 使用按键解绑涂鸦(SUPPORT_KEYUNBIND)
#define SUPPORT_KEYUNBIND 1
// </e>

//<c> 长按按键减小风机吸力(BUTTON_LONG_PRESS_MINISH_FAN_OUTPUT_PWMVALUE)
// #define BUTTON_LONG_PRESS_MINISH_FAN_OUTPUT_PWMVALUE
//</c>

//</h>

//<h>加密
//<e> 加密开关(ENCRYPTION_VERIFY)
#define ENCRYPTION_VERIFY 1
#if ENCRYPTION_VERIFY
// <o> TSD_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define TSD_PIN PC14

//<e> 二次加密(SECURITY_CHECK)
#define SECURITY_CHECK   0
//</e>
#endif
//</e>
//</h>

//<h>电池

// <o> BATT_POWER_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define BATT_POWER_PIN PB10

//<o> 电池分压比例(BATTERY_SCALE) <0-50>
#define BATTERY_SCALE 11
//<c> 开机电池最小电压(LITHIUM_BATTERY_MINVALUE)
//#define LITHIUM_BATTERY_MINVALUE                15.5
//</c>

//<c> 断电时，机器进入低功耗的电池电压值(LITHIUM_BATTERY_PROVALUE)
// #define LITHIUM_BATTERY_PROVALUE                12.5
//</c>

//<o> 电池电量检测周期(BATTERY_CHECKTIME) <100-1000>
#define BATTERY_CHECKTIME  500

//<c> 使用分立式电池充电方法(USE_BATT_CHARGE)
//  #define USE_BATT_CHARGE
//  #define BATT_CELL_NUM 2
//  #define BATT_CHG_FULL_VOLTAGE 4.1f
//  #define BATT_CHG_SLOW_VOLTAGE 2.5f
//  #define BATT_CHG_RESTART_VOLTAGE 4.0f
//  #define BATT_CHG_PWM_VALUE 10 // 20
//  #define BATT_CHG_SLOW_PWM_VALUE 5
//  #define BATT_CHG_MAX_PWM_VALUE 20
//</c>

//</h>

//<h>PWM

//<o> 使用PWM通道个数(USABLE_TIMER_CHANNEL_COUNT) <0-10>
#define USABLE_TIMER_CHANNEL_COUNT 5

// <o> MOTOR_PWM1_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define MOTOR_PWM1_PIN PA8

// <o> MOTOR_PWM2_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define MOTOR_PWM2_PIN PA9

// <o> MOTOR_PWM3_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define MOTOR_PWM3_PIN PA10

// <o> MOTOR_PWM4_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define MOTOR_PWM4_PIN PA11

// <o> FAN_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define FAN_PIN PA4

//</h>

//<h>ADC

//<e> 使用风机ADC开关(FAN_ADC_SW)
#define FAN_ADC_SW 1
#if FAN_ADC_SW
// <o> FAN_ADC_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define FAN_ADC_PIN PA0

//<o> 风机ADC检测通道(FAN_ADC_IDX) <0-10>
#define FAN_ADC_IDX 0

#endif
//</e>

//<e> 使用左电机ADC开关(MOTOR_LEFT_ADC)
#define MOTOR_LEFT_ADC 1
#if MOTOR_LEFT_ADC
// <o> MOTOR_LEFT_ADC_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define MOTOR_LEFT_ADC_PIN PA5

//<o> 左电机ADC检测通道(MOTOR_LEFT_ADC_IDX) <0-10>
#define MOTOR_LEFT_ADC_IDX 1

#endif
//</e>

//<e> 使用有电机ADC开关(MOTOR_RIGHT_ADC)
#define MOTOR_RIGHT_ADC 1
#if MOTOR_RIGHT_ADC
// <o> MOTOR_RIGHT_ADC_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define MOTOR_RIGHT_ADC_PIN PA6

//<o> 右电机ADC检测通道(MOTOR_RIGHT_ADC_IDX) <0-10>
#define MOTOR_RIGHT_ADC_IDX 2

#endif
//</e>

//<e> 使用电池ADC开关(BATTERY_ADC)
#define BATTERY_ADC 1
#if BATTERY_ADC
// <o> BATTERY_ADC_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define BATTERY_ADC_PIN PA7

//<o> 电池ADC检测通道(BATTERY_ADC_IDX) <0-10>
#define BATTERY_ADC_IDX 3

#endif
//</e>

//<e> 使用水量检测开关(WATER_ADC)
#define WATER_ADC 1
#if WATER_ADC
// <o> WATER_ADC_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define WATER_ADC_PIN PB1

//<o> 水量检测ADC通道(WATER_ADC_IDX) <0-10>
#define WATER_ADC_IDX 4

#endif
//</e>

//<o> ADC检测通道个数(ADC_CHANNEL_CNT) <0-10>
#define ADC_CHANNEL_CNT 5

//</h>

//<h>风机
//<h> 风机吸力等级调节配置
//<c> 使用风机等级调节开关(USE_FAN_LEVEL)
//#define USE_FAN_LEVEL
//</c>
#ifdef USE_FAN_LEVEL
//<o> 一级吸力(FAN_LEVEL_1) <0-100>
#define FAN_LEVEL_1 85

//<o> 二级吸力(FAN_LEVEL_2) <0-100>
#define FAN_LEVEL_2 92

//<o> 三级吸力(FAN_LEVEL_3) <0-100>
#define FAN_LEVEL_3 95

//<o> 使用中的风机吸力等级(FAN_DEFAULT_INDEX)
// <FAN_LEVEL_1=> FAN_LEVEL_1 <FAN_LEVEL_2=> FAN_LEVEL_2 <FAN_LEVEL_3=> FAN_LEVEL_3
#define FAN_DEFAULT_INDEX FAN_LEVEL_3
#endif
//</h>

//<h>风机动态吸力调节配置
//<c> 使用动态吸力调节开关(USE_FAN_LEVEL_DYNAMIC_COMP)
// #define USE_FAN_LEVEL_DYNAMIC_COMP
//</c>
#if USE_FAN_LEVEL_DYNAMIC_COMP
//<o> 动态吸力调节最大值(PWMVALUE_MAX) <0-100>
#define PWMVALUE_MAX 90

//<o> 动态吸力调节最小值(PWMVALUE_MIN) <0-100>
#define PWMVALUE_MIN 81
#endif
//</h>

//<h>风机闭环吸力调节配置
//<c> 使用闭环吸力调节开关(USE_FAN_OUTPUT_PID)
#define USE_FAN_OUTPUT_PID
//</c>

//<o> 闭环吸力最小吸力值(FAN_PWMVALUE_MIN) <0-100>
#define FAN_PWMVALUE_MIN 77

//<o> 闭环吸力最大吸力值(FAN_PWMVALUE_MAX) <0-100>
#define FAN_PWMVALUE_MAX 100

//<o> 闭环吸力默认气压值(DEFAULT_TARGET_FAN_PWMVALUE) <0-1000>
#define DEFAULT_TARGET_FAN_PWMVALUE 275

//<o> 闭环吸力最大气压值(MAX_TARGET_FAN_PWMVALUE) <0-1000>
#define MAX_TARGET_FAN_PWMVALUE 300

//<o> 闭环吸力最小气压值(MIN_TARGET_FAN_PWMVALUE) <0-1000>
#define MIN_TARGET_FAN_PWMVALUE 250

//</h>

//<o> 默认状态风机占空比(PWMVALUE) <0-100>
#define PWMVALUE 85

#define FAN_PWMVALUE_AT_IDLE PWMVALUE

//<c>使用风机软启动(USE_SOFT_LAUNCH_FAN)
#define USE_SOFT_LAUNCH_FAN
//</c>

//<o> 风机软启动延时(SOFT_LAUNCH_FAN_DELAY) <0-200>
#define SOFT_LAUNCH_FAN_DELAY 20

//</h>

//<h>边轮电机
//<o> 向上运动边轮电机最小输出(MIN_PWM_VALUE) <0-100>
#define MIN_PWM_VALUE 75

//<o> 向上运动边轮电机最小输出(UP_MIN_PWM_VALUE) <0-100>
#define UP_MIN_PWM_VALUE 75

#define MOTOR_START_UP 0

//<c>边轮使用无刷电机方案(USE_BRUSHLESS_MOTOR)
// #define USE_BRUSHLESS_MOTOR
//</c>

//<c>电机PWM反向(USE_MOTOR_PWM_INVERT)
#define USE_MOTOR_PWM_INVERT
//</c>

//<c>交换左右边轮(USE_L_R_MOTOR_INVERT)
// #define USE_L_R_MOTOR_INVERT
//</c>

//</h>

//<h>气压
//<c> 使用气压开关(USE_BARO)
#define BARO
//</c>

//<c> 使用HP203B(USE_BARO_HP203B)
#define USE_BARO_HP203B
//</c>
//</h>

//<h>陀螺仪

//<h>使用的陀螺仪型号
//<c> 使用mpu6887(USE_MPU6887)
#define USE_MPU6887
//</c>

//<c> 使用ICM42670N(USE_ICM42670N)
#define USE_ICM42670N
//</c>

//<c> 使用USE_LSM6DS3TR_C(USE_LSM6DS3TR_C)
//#define USE_LSM6DS3TR_C
//</c>

//<c> 使用USE_LSM6DSR(USE_LSM6DSR)
//#define USE_LSM6DSR
//</c>

//<c> 使用USE_MPU6050(USE_MPU6050)
//#define USE_MPU6050
//</c>

//<c> 使用USE_MPU6509(USE_MPU6509)
//#define USE_MPU6509
//</c>

//<c> 使用USE_QMI8658A(USE_QMI8658A)
//#define USE_QMI8658A
//</c>

//</h>

//<o> 检测边缘陀螺仪变化值阈值(GYRO_DIFF_THRESHOLD) <0-500>
#define GYRO_DIFF_THRESHOLD 140

//<o> 检测边缘堵转陀螺仪阈值(GYRO_THRESHOLD) <0-500>
#define GYRO_THRESHOLD 100

//<o> 向上运动时陀螺仪变化阈值(GYRO_UP_DIFF_THRESHOLD) <0-500>
#define GYRO_UP_DIFF_THRESHOLD 100

//<o> 向上运动时陀螺仪堵转阈值(GYRO_UP_THRESHOLD) <0-100>
#define GYRO_UP_THRESHOLD 45

//<o> 陀螺仪检测延时(GYRO_CHECK_DELAY) <0-200>
#define GYRO_CHECK_DELAY 130

//<e> 数据储存设置开关(PARA_ENABLE)
#define PARA_ENABLE 1
//</e>

//</h>

//<h> 自动模式配置

// <c> 第一行倾斜角度(LEANTOP_ANGLE)
//#define CLEANTOP_ANGLE -65
// </c>

//<c> 换行后撤一步开关(AUTO_MOVE_DOWN_BACK_ANGLE)
#define AUTO_MOVE_DOWN_BACK_ANGLE
//</c>

//<h>有边框检测阈值设置
//<o> 换行回撤角度(AUTO_MODE_BACK_ANGLE) <0-30>
#define AUTO_MODE_BACK_ANGLE 20
//<o> 换行下移次数(AUTO_MODE_MOVE_DOWN_NUM) <0-5>
#define AUTO_MODE_MOVE_DOWN_NUM 3

//<c>使用障碍物检测(USE_OBSTACLE_DETECT)
#define USE_OBSTACLE_DETECT
//</c>
//</h>

//<h>无边框检测阈值设置

//<c> 无边换行出去增加风机吸力开关(AUTO_MOVE_DOWN_FAN_OUTPUT_SET)
#define AUTO_MOVE_DOWN_FAN_OUTPUT_SET
//</c>

//<o> 无边换行下移次数(HANG_AUTO_MODE_MOVE_DOWN_CNT) <0-10>
#define HANG_AUTO_MODE_MOVE_DOWN_CNT 3

//<o> 无边气压初始值设置(FAN_THRD_INIT) <10000-13000>
#define FAN_THRD_INIT 10168

//<o> 向上运动漏气检测阈值(FAN_UP_THRD_ADD) <0-500>
#define FAN_UP_THRD_ADD 40

//<o> 正常漏气检测阈值(FAN_THRD_ADD) <0-500>
#define FAN_THRD_ADD 40//110

//<o> 允许漏气检测次数(HANG_CNT) <0-5>
#define HANG_CNT 3

//<o> 无边漏气检测延时(HANG_CHECK_DELAY) <0-500>
#define HANG_CHECK_DELAY 0
//</h>
//</h>

//<h> 开机方式
//<c>机器插上适配器即供电，单片机启动工作，长按按键开机(USE_SOFTWARE_POWER_ON)
#define USE_SOFTWARE_POWER_ON
//</c>

//<c>物理开关机(USE_HARDWARE_TOGGLE_POWER_ON)
// #define USE_HARDWARE_TOGGLE_POWER_ON
//</c>

//<h>物理开关+长按按键关机(HW_SWTICH_PIN)

//<e> 物理开关引脚配置(HW_SWTICH_PIN)
#define USE_HW_SWTICH_PIN 0
#if USE_HW_SWTICH_PIN
// <o> HW_SWTICH_PIN
// <PA0=> PA0 <PA1=> PA1 <PA2=> PA2 <PA3=> PA3 <PA4=> PA4 <PA5=> PA5 <PA6=> PA6 <PA7=> PA7 <PA8=> PA8 <PA9=> PA9 <PA10=> PA10 <PA11=> PA11 <PA12=> PA12 <PA13=> PA13 <PA14=> PA14 <PA15=> PA15
// <PB0=> PB0 <PB1=> PB1 <PB2=> PB2 <PB3=> PB3 <PB4=> PB4 <PB5=> PB5 <PB6=> PB6 <PB7=> PB7 <PB8=> PB8 <PB9=> PB9 <PB10=> PB10 <PB11=> PB11 <PB12=> PB12 <PB13=> PB13 <PB14=> PB14 <PB15=> PB15
// <PC0=> PC0 <PC1=> PC1 <PC2=> PC2 <PC3=> PC3 <PC4=> PC4 <PC5=> PC5 <PC6=> PC6 <PC7=> PC7 <PC8=> PC8 <PC9=> PC9 <PC10=> PC10 <PC11=> PC11 <PC12=> PC12 <PC13=> PC13 <PC14=> PC14 <PC15=> PC15
// <PD0=> PD0 <PD1=> PD1 <PD2=> PD2 <PD3=> PD3 <PD4=> PD4 <PD5=> PD5 <PD6=> PD6 <PD7=> PD7 <PD8=> PD8 <PD9=> PD9 <PD10=> PD10 <PD11=> PD11 <PD12=> PD12 <PD13=> PD13 <PD14=> PD14 <PD15=> PD15
#define HW_SWTICH_PIN PC14
#endif
//</e>

//<c> 长按按键开关机(USE_SWITCH_AND_LONG_PRESS_POWER_ON)
// #define USE_SWITCH_AND_LONG_PRESS_POWER_ON
//</c>

//</h>

//<e> 关机检测(POWER_OFF_CHECK)
#define POWER_OFF_CHECK 0
//</e>

//</h>

//<h> 停机检测

//<o> 停机检测次数(AUTO_MODE_DETECT_STOP_NUM) <0-10>
#define AUTO_MODE_DETECT_STOP_NUM 4

//<c> 自动模式只在左下角停止(AUTO_MODE_DETECT_STOP_LEFT)
#define AUTO_MODE_DETECT_STOP_LEFT 1
//</c>

//<e> 底部停机智能检测(BOTTOM_SMART)
#define BOTTOM_SMART 0

//<o> 底部停机智能检测阈值(BOTTOM_EDGE_DIFF) <0-300>
#define BOTTOM_EDGE_DIFF 30
//</e>

//<e> 使用下移换行检测停机(USE_AUTO_MOVE_DOWN_BOTTOM_EDGE_DETECT)
#define USE_AUTO_MOVE_DOWN_BOTTOM_EDGE_DETECT 1
//<o> 下移换行检测停机阈值(EDGE_BOTTOM_GYRO_DIFF) <0-300>
#define EDGE_BOTTOM_GYRO_DIFF 120
//</e>

//</h>

//<e>涂鸦
#define USE_BLE_TUYA 1

//<c> 使用通用涂鸦设置(USE_TUYA_COMMON)
#define USE_TUYA_COMMON
//</c>

//<c> 使用涂鸦升级(USE_BOOTLOADER)
#define USE_BOOTLOADER
//</c>

//<o> 涂鸦应用程序地址(APPLICATION_ADDRESS) <0x00-0xFFFFFFFF>
#define APPLICATION_ADDRESS     (uint32_t)0x08003000

//<s> 涂鸦PID(PRODUCT_KEY)
#define PRODUCT_KEY          "rlavhjr9"

//</e>

//<h>版本管理
//<o> 软件发布版本号(MCU_VERSION_PATCH_LEVEL) <0-9>
#define MCU_VERSION_PATCH_LEVEL      4

//<o> 硬件版本号（MCU_HARD_VER_NUM) <0x00-0xFFFFFF>
#define MCU_HARD_VER_NUM     0x010100

//<s> APP升级版本号(MCU_BOOTLOADER_VERSION)
#define MCU_BOOTLOADER_VERSION   "1.0.5"

//<o> bootloader实际版本号(MCU_BOOTLOADER_VER_NUM) <0x00-0xFFFFFF>
#define MCU_BOOTLOADER_VER_NUM  0x010005
//</h>

//<h>IIC 硬件IIC通信
//<o> I2C_SCL_PIN
//<GPIO_Pin_0=> GPIO_Pin_0 <GPIO_Pin_1=> GPIO_Pin_1 <GPIO_Pin_2=> GPIO_Pin_2 <GPIO_Pin_3=> GPIO_Pin_3 <GPIO_Pin_4=> GPIO_Pin_4 <GPIO_Pin_5=> GPIO_Pin_5 <GPIO_Pin_6=> GPIO_Pin_6 <GPIO_Pin_7=> GPIO_Pin_7 <GPIO_Pin_8=> GPIO_Pin_8
//<GPIO_Pin_9=> GPIO_Pin_9 <GPIO_Pin_10=> GPIO_Pin_10 <GPIO_Pin_11=> GPIO_Pin_11 <GPIO_Pin_12=> GPIO_Pin_12 <GPIO_Pin_13=> GPIO_Pin_13 <GPIO_Pin_14=> GPIO_Pin_14 <GPIO_Pin_15=> GPIO_Pin_15
#define I2C_SCL_PIN                  GPIO_Pin_8

//<o> I2C_SCL_GPIO_PORT
//<GPIOA=> GPIOA <GPIOB=> GPIOB <GPIOC=> GPIOC <GPIOD=> GPIOD <GPIOE=> GPIOE <GPIOF=> GPIOF
#define I2C_SCL_GPIO_PORT            GPIOB

//<o> I2C_SCL_GPIO_CLK
//<RCC_AHBENR_GPIOA=> RCC_AHBENR_GPIOA <RCC_AHBENR_GPIOB=> RCC_AHBENR_GPIOB <RCC_AHBENR_GPIOC=> RCC_AHBENR_GPIOC <RCC_AHBENR_GPIOD=> RCC_AHBENR_GPIOD
#define I2C_SCL_GPIO_CLK             RCC_AHBENR_GPIOB

//<o> I2C_SCL_SOURCE
//<GPIO_PinSource0=> GPIO_PinSource0 <GPIO_PinSource1=> GPIO_PinSource1 <GPIO_PinSource2=> GPIO_PinSource2 <GPIO_PinSource3=> GPIO_PinSource3 <GPIO_PinSource4=> GPIO_PinSource4 <GPIO_PinSource5=> GPIO_PinSource5 <GPIO_PinSource6=> GPIO_PinSource6 <GPIO_PinSource7=> GPIO_PinSource7 <GPIO_PinSource8=> GPIO_PinSource8
//<GPIO_PinSource9=> GPIO_PinSource9 <GPIO_PinSource10=> GPIO_PinSource10 <GPIO_PinSource11=> GPIO_PinSource11 <GPIO_PinSource12=> GPIO_PinSource12 <GPIO_PinSource13=> GPIO_PinSource13 <GPIO_PinSource14=> GPIO_PinSource14 <GPIO_PinSource15=> GPIO_PinSource15
#define I2C_SCL_SOURCE               GPIO_PinSource8

//<o> I2C_SCL_AF
//<GPIO_AF_0=> GPIO_AF_0 <GPIO_AF_1=> GPIO_AF_1 <GPIO_AF_2=> GPIO_AF_2 <GPIO_AF_3=> GPIO_AF_3 <GPIO_AF_4=> GPIO_AF_4 <GPIO_AF_5=> GPIO_AF_5 <GPIO_AF_6=> GPIO_AF_6 <GPIO_AF_7=> GPIO_AF_7 <GPIO_AF_8=> GPIO_AF_8
#define I2C_SCL_AF                   GPIO_AF_1

//<o> I2C_SDA_PIN
//<GPIO_Pin_0=> GPIO_Pin_0 <GPIO_Pin_1=> GPIO_Pin_1 <GPIO_Pin_2=> GPIO_Pin_2 <GPIO_Pin_3=> GPIO_Pin_3 <GPIO_Pin_4=> GPIO_Pin_4 <GPIO_Pin_5=> GPIO_Pin_5 <GPIO_Pin_6=> GPIO_Pin_6 <GPIO_Pin_7=> GPIO_Pin_7 <GPIO_Pin_8=> GPIO_Pin_8
//<GPIO_Pin_9=> GPIO_Pin_9 <GPIO_Pin_10=> GPIO_Pin_10 <GPIO_Pin_11=> GPIO_Pin_11 <GPIO_Pin_12=> GPIO_Pin_12 <GPIO_Pin_13=> GPIO_Pin_13 <GPIO_Pin_14=> GPIO_Pin_14 <GPIO_Pin_15=> GPIO_Pin_15
#define I2C_SDA_PIN                  GPIO_Pin_9

//<o> I2C_SDA_GPIO_PORT
//<GPIOA=> GPIOA <GPIOB=> GPIOB <GPIOC=> GPIOC <GPIOD=> GPIOD <GPIOE=> GPIOE <GPIOF=> GPIOF
#define I2C_SDA_GPIO_PORT            GPIOB

//<o> I2C_SDA_GPIO_CLK
//<RCC_AHBENR_GPIOA=> RCC_AHBENR_GPIOA <RCC_AHBENR_GPIOB=> RCC_AHBENR_GPIOB <RCC_AHBENR_GPIOC=> RCC_AHBENR_GPIOC <RCC_AHBENR_GPIOD=> RCC_AHBENR_GPIOD
#define I2C_SDA_GPIO_CLK             RCC_AHBENR_GPIOB

//<o> I2C_SDA_SOURCE
//<GPIO_PinSource0=> GPIO_PinSource0 <GPIO_PinSource1=> GPIO_PinSource1 <GPIO_PinSource2=> GPIO_PinSource2 <GPIO_PinSource3=> GPIO_PinSource3 <GPIO_PinSource4=> GPIO_PinSource4 <GPIO_PinSource5=> GPIO_PinSource5 <GPIO_PinSource6=> GPIO_PinSource6 <GPIO_PinSource7=> GPIO_PinSource7 <GPIO_PinSource8=> GPIO_PinSource8
//<GPIO_PinSource9=> GPIO_PinSource9 <GPIO_PinSource10=> GPIO_PinSource10 <GPIO_PinSource11=> GPIO_PinSource11 <GPIO_PinSource12=> GPIO_PinSource12 <GPIO_PinSource13=> GPIO_PinSource13 <GPIO_PinSource14=> GPIO_PinSource14 <GPIO_PinSource15=> GPIO_PinSource15
#define I2C_SDA_SOURCE               GPIO_PinSource9

//<o> I2C_SDA_AF
//<GPIO_AF_0=> GPIO_AF_0 <GPIO_AF_1=> GPIO_AF_1 <GPIO_AF_2=> GPIO_AF_2 <GPIO_AF_3=> GPIO_AF_3 <GPIO_AF_4=> GPIO_AF_4 <GPIO_AF_5=> GPIO_AF_5 <GPIO_AF_6=> GPIO_AF_6 <GPIO_AF_7=> GPIO_AF_7 <GPIO_AF_8=> GPIO_AF_8
#define I2C_SDA_AF                   GPIO_AF_1

//<o> I2C_NUM
//<I2C1=> I2C1 <I2C2=> I2C2
#define I2C_NUM                     I2C1

//<o> I2C_NUM_CLK
//<RCC_APB1Periph_I2C1=> RCC_APB1Periph_I2C1 <RCC_APB1Periph_I2C2=> RCC_APB1Periph_I2C2
#define I2C_NUM_CLK                 RCC_APB1Periph_I2C1

//</h>

// <<< end of configuration section >>>
```

## VSCode 安装 EIDE插件

* 安装 EIDE插件

![image](images/CMSIS_Configuration_Wizard1.png)

* 点开target.h鼠标右键选择 CMSIS Configuration Wizard 选项

![image](images/CMSIS_Configuration_Wizard2.png)

* 使用图像化向导配置target.h

![image](images/CMSIS_Configuration_Wizard3.png)





