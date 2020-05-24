# Device Supervisor App用户手册
Device Supervisor App（以下简称Device Supervisor）为用户提供了便捷的数据采集、数据处理和数据上云功能，支持ISO on TCP、ModbusRTU等多种工业协议解析。
本手册以采集PLC的数据并上传至Thingsboard云平台为例说明如何通过Device Supervisor App实现PLC数据采集和数据上云。以下将InGateway501简称为“IG501”；InGateway902简称为“IG902”。

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Device Supervisor App用户手册](#device-supervisor-app用户手册)
  - [概览](#概览)
  - [1.准备硬件设备及其数据采集环境](#1准备硬件设备及其数据采集环境)
    - [1.1 硬件接线](#11-硬件接线)
      - [1.1.1 以太网接线](#111-以太网接线)
      - [1.1.2 串口接线](#112-串口接线)
    - [1.2 设置LAN网络参数：在局域网访问PLC](#12-设置lan网络参数在局域网访问plc)
    - [1.3 设置WAN网络参数：传输数据至MQTT服务器](#13-设置wan网络参数传输数据至mqtt服务器)
    - [1.4 更新InGateway设备软件版本](#14-更新ingateway设备软件版本)
  - [2.配置Device Supervisor App](#2配置device-supervisor-app)
    - [2.1 安装并运行Device Supervisor](#21-安装并运行device-supervisor)
    - [2.2 Device Supervisor数据采集配置](#22-device-supervisor数据采集配置)
      - [2.2.1 添加PLC设备](#221-添加plc设备)
      - [2.2.2 添加变量](#222-添加变量)
      - [2.2.3 配置告警策略](#223-配置告警策略)
      - [2.2.4 配置分组](#224-配置分组)
  - [3.监控PLC数据](#3监控plc数据)
    - [3.1 本地监控PLC数据](#31-本地监控plc数据)
      - [3.1.1 本地监控数据采集](#311-本地监控数据采集)
      - [3.1.2 本地监控告警](#312-本地监控告警)
    - [3.2 在Thingsboard上监控PLC数据](#32-在thingsboard上监控plc数据)
      - [3.2.1 配置Thingsboard](#321-配置thingsboard)
      - [3.2.2 配置云服务](#322-配置云服务)
  - [附录](#附录)
    - [导入导出配置](#导入导出配置)
    - [全局参数](#全局参数)
    - [自定义数据格式](#自定义数据格式)
      - [发布](#发布)
      - [回调函数说明](#回调函数说明)
    - [其他网关操作](#其他网关操作)
    - [Thingsboard参考流程](#thingsboard参考流程)
      - [添加设备和资产](#添加设备和资产)
      - [传输PLC数据到Thingsboard设备](#传输plc数据到thingsboard设备)
      - [配置可视化仪表板](#配置可视化仪表板)
  - [FAQ](#faq)

<!-- /code_chunk_output -->

## 概览
使用过程中，您需要准备以下项：  
- 边缘计算网关IG501/IG902  
- PLC设备  
- 网线/串口线  
- *更新软件版本所需的固件、SDK和App  
- *Thingsboad演示账号  

整体流程如下图所示：  
- [环境准备](#准备硬件设备及其数据采集环境)
- [数据采集配置（配置Device Supervisor App）](#配置Plc_Supervisor_App)
- [监控PLC数据](#监控PLC数据)  
- [附录](#附录)

![](images/2020-05-12-17-19-17.png)

<a id="准备硬件设备及其数据采集环境"> </a>  

## 1.准备硬件设备及其数据采集环境
- [硬件接线](#硬件接线)
- [设置LAN网络参数](#设置LAN网络参数)
- [设置WAN网络参数](#设置WAN网络参数)
- [更新软件版本](#更新InGateway设备软件版本)  

<a id="硬件接线"> </a>  

### 1.1 硬件接线
#### 1.1.1 以太网接线
- IG902以太网接线  
  
  接通IG902的电源并按照拓扑使用以太网线连接IG902和PLC。  <br/>
![](images/2020-03-12-14-02-22.png)  
 &nbsp;

- IG501以太网接线  
  
  接通IG501的电源并按照拓扑使用以太网线连接IG501和PLC。  <br/>
![](images/2020-03-12-14-03-03.png)
#### 1.1.2 串口接线
- IG902串口接线  

  接通IG902的电源并按照拓扑连接IG902和PLC。  <br/>
![](images/2020-03-12-14-03-36.png)  

  IG902串口端子接线说明如下图：  <br/>
  ![](images/2020-01-09-18-47-30.png)  

- IG501串口接线  

  接通IG501的电源并按照拓扑连接IG501和PLC。  <br/>
![](images/2020-03-12-14-04-05.png)  

  IG501串口端子接线说明如下图：  <br/>
  ![](images/2020-03-11-11-38-45.png)
 
<a id="设置LAN网络参数"> </a>  

### 1.2 设置LAN网络参数：在局域网访问PLC
- IG902的GE 0/2口的默认IP地址为`192.168.2.1`。为了使IG902能够通过GE 0/2口访问以太网PLC，需要设置GE 0/2口与PLC处于同一网段，设置方法请参考[在局域网访问IG902](https://ingateway-development-docs.readthedocs.io/zh_CN/latest/IG902%E5%BF%AB%E9%80%9F%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C.html#lan-ig902)。
- IG501的FE 0/1口的默认IP地址为`192.168.1.1`。为了使IG501能够通过FE 0/1口访问以太网PLC，需要设置FE 0/1口与PLC处于同一网段，设置方法请参考[在局域网访问IG501](https://ingateway-development-docs.readthedocs.io/zh_CN/latest/IG501%E5%BF%AB%E9%80%9F%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C.html#lan-ig501)。

<a id="设置WAN网络参数"> </a>  

### 1.3 设置WAN网络参数：传输数据至MQTT服务器
- 设置IG902 WAN网络参数，请参考[IG902连接Internet](https://ingateway-development-docs.readthedocs.io/zh_CN/latest/IG902%E5%BF%AB%E9%80%9F%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C.html#wan-internet)。
- 设置IG501 WAN网络参数，请参考[IG501连接Internet](https://ingateway-development-docs.readthedocs.io/zh_CN/latest/IG501%E5%BF%AB%E9%80%9F%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C.html#wan-internet)。

<a id="更新InGateway设备软件版本"> </a>  

### 1.4 更新InGateway设备软件版本
如需获取InGateway产品最新软件版本及其功能特性信息，请联系客服。如需更新软件版本，请参考如下链接：
- [更新IG902软件版本](https://ingateway-development-docs.readthedocs.io/zh_CN/latest/IG902%E5%BF%AB%E9%80%9F%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C.html#id1)
  使用Device Supervisor时，IG902的固件版本应为`V2.0.0.r12537`及以上；SDK版本应为`py3sdk-V1.3.5`及以上。
- [更新IG501软件版本](https://ingateway-development-docs.readthedocs.io/zh_CN/latest/IG501%E5%BF%AB%E9%80%9F%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C.html#id1)

<a id="配置Plc_Supervisor_App"> </a>  

## 2.配置Device Supervisor App
- [安装并运行Device Supervisor](#安装并运行PLC_Supervisor)
- [Device Supervisor数据采集配置](#PLC_Supervisor数据采集配置)
  - [添加PLC设备](#添加PLC设备)
  - [添加变量](#添加变量)
  - [配置告警策略](#配置告警策略)
  - [配置变量分组](#配置变量分组)

<a id="安装并运行PLC_Supervisor"> </a>  

### 2.1 安装并运行Device Supervisor
- IG902如何安装并运行Python App请参考[IG902安装和运行Python App](https://ingateway-development-docs.readthedocs.io/zh_CN/latest/IG902%E5%BF%AB%E9%80%9F%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C.html#python-app)，Device Supervisor正常运行后如下图所示： 
  ![](images/2020-02-21-17-57-15.png)
 &nbsp;
- IG501如何安装并运行Python App请参考[IG501安装和运行Python App](https://ingateway-development-docs.readthedocs.io/zh_CN/latest/IG501%E5%BF%AB%E9%80%9F%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C.html#python-app)，Device Supervisor正常运行后如下图所示：  
  ![](images/2020-02-21-17-57-15.png)  

<a id="PLC_Supervisor数据采集配置"> </a>  

### 2.2 Device Supervisor数据采集配置
<a id="添加PLC设备"> </a>  

#### 2.2.1 添加PLC设备
- 添加ISO on TCP通讯的PLC设备  
  
  点击“添加PLC”按钮，在添加设备页面选择PLC协议为“ISO on TCP”并配置PLC的通讯参数。（除PLC为S7-200 Smart时机架号和槽号需要配置为0，1；其余类型的S7系列PLC默认使用0，0即可）<font color=#FF0000>注意：设备名称不能重复。</font>  

  ![](images/2020-02-27-14-18-01.png)  

  添加成功后如下图所示：  
![](images/2020-02-27-14-32-51.png)  
 &nbsp;
</br>

- 添加ModbusTCP通讯的PLC设备  
  
  点击“添加PLC”按钮，在添加设备页面选择PLC协议为“ModbusTCP”并配置PLC的通讯参数。（端口号和字节序默认为502和abcd；使用时需根据实际情况调整）<font color=#FF0000>注意：设备名称不能重复。</font>  

  ![](images/2020-03-06-15-02-16.png)  

  添加成功后如下图所示：  
  ![](images/2020-03-06-15-02-42.png)  
 &nbsp;
</br>

- 添加ModbusRTU通讯的PLC设备  
  
  点击“添加PLC”按钮，在添加设备页面选择PLC协议为“ModbusRTU”并配置PLC的通讯参数。<font color=#FF0000>注意：设备名称不能重复。</font>  

  ![](images/2020-02-27-14-19-59.png)  

  添加成功后如下图所示：
![](images/2020-03-06-15-03-41.png)  

  如需修改RS232/RS485串口的通讯参数，请在“边缘计算 > 设备监控 > 全局参数”页面修改。修改后所有串口设备的通讯参数将自动修改并按照修改后的通讯参数通讯。  
![](images/2020-02-27-14-26-04.png)

<a id="添加变量"> </a>  

#### 2.2.2 添加变量
- 添加ISO on TCP变量  
  
  点击“添加变量”按钮，在弹出框中配置变量参数：
  - 变量名：变量名称<font color=#FF0000>（同一设备下变量名称不能重复）</font>    
  - 寄存器类型：变量寄存器类型，包括I/Q/M/DB四种类型  
  - DB索引：寄存器类型为DB时变量的DB号  
  - 地址：变量的寄存器地址  
  - 数据类型：变量数据类型，包括：  
    - BOOL：True或False  
    - BIT：0或1  
    - BYTE：8位无符号数据  
    - SINT：8位有符号数据  
    - WORD：16位无符号数据  
    - INT：16位有符号数据  
    - DWORD：32位无符号数据  
    - DINT：32位有符号数据  
    - FLOAT：32位浮点数  
    - STRING：8位字符串  
    - BCD ：16位BCD码  
  - 小数位：数据类型为FLOAT时变量小数点后的数据长度，最大6位  
  - 长度：数据类型为STRING时字符串长度，读取1个字符串的长度为1  
  - 位：数据类型为BOOL或BIT时变量的位偏移，可输入0~7中任一数字  
  - 读写权限：  
    - Read：只读，不可写  
    - Write：只写，不可读  
    - Read/Write：可读可写  
  - 采集模式：  
    - Realtime：按照所属分组的采集间隔采集变量并按照上报间隔上报数据  
    - Onchange：变量数值有变化时才采集变量并按照上报间隔上报数据  
  - 单位：变量单位  
  - 描述：变量描述  
  - 所属分组：变量所属的采集组  
   &nbsp;
   
  下图是添加一个地址为%I0.0的开关变量的例子：  </br>
  ![](images/2020-02-27-18-06-40.png)  

  下图是添加一个地址为%IB1的字节变量的例子：  </br>
  ![](images/2020-02-27-18-08-22.png)  

  下图是添加一个地址为%IW3的字变量的例子：  </br>
  ![](images/2020-02-28-16-26-04.png)  

  下图是添加一个地址为%ID4的双字变量的例子：  </br>
  ![](images/2020-02-28-16-27-49.png)  

  下图是添加一个地址为%DB6.DBD18的浮点数变量的例子：  </br>
  ![](images/2020-02-28-16-25-21.png)  
 &nbsp;
 </br>

- 添加Modbus变量  
  
  点击“添加变量”按钮，在添加变量弹出框中配置PLC变量参数：
  - 变量名：变量名称<font color=#FF0000>（同一设备下变量名称不能重复）</font>    
  - 地址：变量的寄存器地址  
  - 数据类型：变量数据类型，包括：  
    - BOOL：True或False  
    - BIT：0或1  
    - WORD：16位无符号数据  
    - INT：16位有符号数据  
    - DWORD：32位无符号数据  
    - DINT：32位有符号数据  
    - FLOAT：32位浮点数  
    - STRING：8位字符串   
  - 小数位：数据类型为FLOAT时变量小数点后的数据长度，最大6位  
  - 长度：数据类型为STRING时字符串长度  
  - 位：地址为`30001~40000`,`310001~365535`，`40001~50000`，`410001~465535`且数据类型为BOOL或BIT时变量的位偏移，可输入0~15中任一数字  
  - 读写权限：  
    - Read：只读，不可写  
    - Write：只写，不可读  
    - Read/Write：可读可写  
  - 采集模式：  
    - Realtime：按照固定采集间隔采集变量并按照上报间隔上报数据  
    - Onchange：变量数值变化后才采集并按照上报间隔上报数据  
  - 单位：变量单位  
  - 描述：变量描述  
  - 所属分组：变量所属的采集组  
   &nbsp;

  下图是添加一个地址为00001的线圈变量的例子：  </br>
  ![](images/2020-03-06-15-33-59.png)  

  下图是添加一个地址为10001的开关变量的例子：  </br>
![](images/2020-03-06-15-35-15.png)  

  下图是添加一个地址为30001的整数变量的例子：  </br>
  ![](images/2020-03-06-15-36-37.png)  

  下图是添加一个地址为40001的浮点数变量的例子：  </br>
  ![](images/2020-03-06-15-37-11.png)

<a id="配置告警策略"> </a>

#### 2.2.3 配置告警策略

你可以进入“边缘计算 > 设备监控 > 告警 > 告警策略”页面中配置告警策略，点击“添加”按钮后，在弹出框中配置告警策略参数。告警策略支持两种配置方式“使用新变量”和“引用已有变量”，参数如下：
- 使用新变量
  - 名称：告警名称
  - 分组：告警所属分组
  - 变量来源：“使用新变量”即告警变量未在“设备列表”中配置，需要自行设置变量参数（该操作不会在“设备列表”中新增变量）
  - 设备：告警变量所属设备
  - 地址：告警变量地址
  - 数据类型：告警变量数据类型
  - 告警条件
    - 判断条件：支持“=”、“!=”、“>”、“≥”、“<”、“≤”
    - 逻辑条件
      - 无逻辑条件：仅通过单个判断条件判断告警
      - &&：通过两个判断条件相与判断告警
      - ||：通过两个判断条件相或判断告警
  - 描述：告警描述

  下图是添加一个告警变量，该变量数值>30且<50时产生告警；不在此范围时不产生告警或告警消除。  

  ![](images/2020-05-12-18-22-19.png)  
  
- 引用已有变量
  - 名称：告警名称
  - 分组：告警所属分组
  - 变量来源：“引用已有变量”即告警变量已在“设备列表”中配置，可以输入已有变量名称直接使用
  - 设备：告警变量所属设备
  - 地址：告警变量的地址
  - 告警条件
    - 判断条件：支持“=”、“!=”、“>”、“≥”、“<”、“≤”
    - 逻辑条件
      - 无逻辑条件：仅通过单个判断条件判断告警
      - &&：通过两个判断条件相与判断告警
      - ||：通过两个判断条件相或判断告警
  - 描述：告警描述

  下图是引用已有变量生成一条告警变量，该变量数值>30且<50时产生告警；不在此范围时不产生告警或告警消除。  

  ![](images/2020-05-12-18-27-57.png)  

<a id="配置变量分组"> </a>  

#### 2.2.4 配置分组
如需为变量或告警配置不同的采集间隔或需要按照不同的MQTT主题上报相应的变量数据时，可在“边缘计算 > 设备监控 > 分组”页面添加新分组。  
![](images/2020-05-12-18-30-02.png)

- 添加采集分组
  下图添加了一个名为“group2”的采集分组，该采集分组每5秒采集一次分组中的变量：  

  ![](images/2020-05-12-18-37-36.png)  

  添加采集分组后，添加变量时可以选择将变量关联到该分组或者在变量列表中选择需要关联的变量添加到指定分组中，分组中的变量会按照分组的采集间隔采集数据。  

  ![](images/2020-02-27-17-05-33.png)  

  ![](images/2020-02-27-17-06-20.png)

- 添加告警分组
  下图添加了一个名为warn_group2的告警分组，该告警分组每5秒检测一次分组中的告警变量是否处于告警状态：  

  ![](images/2020-05-12-18-46-20.png)
  
  添加告警分组后，添加告警策略时可以选择将告警策略关联到该分组或者在告警列表中选择需要关联的告警策略添加到指定分组中，分组中的告警策略会按照分组的采集间隔检测变量告警状态。 
  
  ![](images/2020-05-12-18-53-33.png)  

  ![](images/2020-05-12-18-52-58.png)  
  
<a id="监控PLC数据"> </a>  

## 3.监控PLC数据
- [本地监控PLC数据](#本地监控PLC数据)
- [在Thingsboard上监控PLC数据](#在Thingsboard上监控PLC数据)  
  
<a id="本地监控PLC数据"> </a>  

### 3.1 本地监控PLC数据
#### 3.1.1 本地监控数据采集
数据采集配置完成后，可以在“边缘计算 > 设备监控 > 设备列表”页面查看数据采集情况。点击设备列表中的设备卡片可切换需要查看的PLC数据
![](images/2020-02-27-17-14-47.png)  

点击数值栏的按钮可进行写入操作  
![](images/2020-03-06-16-59-18.png)  

![](images/2020-03-06-16-59-44.png)  

修改成功如下图所示  
![](images/2020-03-06-17-01-19.png)

#### 3.1.2 本地监控告警
告警策略配置完成后，可以在“边缘计算 > 设备监控 > 告警”页面查看变量告警情况。
- 实时告警：查看当前未消除的告警信息

  ![](images/2020-05-13-09-39-29.png)  
  
- 历史告警：筛选查看任意告警信息
  
  ![](images/2020-05-13-09-41-00.png)  
  
<a id="在Thingsboard上监控PLC数据"> </a>  

### 3.2 在Thingsboard上监控PLC数据
#### 3.2.1 配置Thingsboard
Thingsboard的详细使用方法请查看[Thingsboard入门手册](https://thingsboard.io/docs/getting-started-guides/helloworld/)，您也可以按照[参考流程](#Thingsboard参考流程)进行测试。

#### 3.2.2 配置云服务
进入“边缘计算 > 设备监控 > 云服务”页面，勾选启用云服务并配置相应的MQTT连接参数，配置完成后点击提交。
- 服务器地址：`demo.thingsboard.io`
- 端口号：1883
- MQTT客户端ID：任一唯一ID
- MQTT用户名：Thingsboard设备的访问令牌，访问令牌获取方式见[传输PLC数据到Thingsboard设备](#传输PLC数据到Thingsboard设备)
- MQTT密码：任意6~32位密码
- 其余项使用默认配置即可  

配置完成后如下图所示：  </br>
![](images/2020-02-28-17-38-08.png)

提交后点击“高级设置”以配置发布和订阅主题。发布和订阅主题的配置方法请参考[自定义数据格式](#自定义数据格式)。
- 发布主题：
  - 主题：`v1/devices/me/telemetry`
  - Qos(MQTT)：`1`
  - 分组类型：`采集`
  - 分组：需要上传数据至thingsboard的分组名称，本文档为`default`
  - 主函数：入口函数名称，本文档为`test_upload`
  - 脚本：
    ```python
    from common.Logger import logger #导入打印日志模块logger
    
    def upload_test(data, wizard_api): #定义主函数upload_test
        logger.info(data) #在日志中以info等级打印采集到的数据
        value_dict = {} #定义上报的数据字典value_dict
        for device, val_dict in data['values'].items(): #遍历data中的values字典，该字典中包含设备名称和设备下的变量数据
            for id, val in val_dict.items(): #遍历变量数据，为value_dict字典赋值
                value_dict[id] = val["raw_data"]
            value_dict["timestamp"] = data["timestamp"]
        logger.info(value_dict) #在日志中以info等级打印value_dict
        return value_dict #将value_list发送给App，由App自行顺序上传至MQTT服务器。最终的value_list格式为{'bool': False, 'byte': 7, 'real': 0.0, 'timestamp': 1583990892.5429199}
    ```
  配置完成后如下图所示：  

  ![](images/2020-05-16-19-36-03.png)

- 订阅主题
  - 主题：`v1/devices/me/rpc/request/+`
  - Qos(MQTT)：`1`
  - 主函数：入口函数名称，本文档为`ctl_test`
  - 脚本：
    ```python
    from common.Logger import logger #导入打印日志模块logger
    import json #导入json模块
    
    def ctl_test(topic, payload, wizard_api): #定义主函数ctl_test
        logger.info(topic) #打印订阅主题
        logger.info(payload) #打印订阅数据，Thingsboard的下发数据格式为{"method":"setValue","params":true}
        payload = json.loads(payload) #反序列化订阅数据
        if payload["method"] == "setValue": #检测是否为写入数据
            message = {"bool":payload["params"]} #定义修改变量值消息，包括变量名称和变量值
            ack_tail = [topic.replace('request', 'response'), message] #定义确认数据，包括响应的主题和消息
            wizard_api.write_plc_values(message, ack, ack_tail) #调用write_plc_values方法，将message字典中的数据下发至指定变量；调用ack方法并发送ack_tail给ack方法
    
    
    def ack(data, ack_tail, wizard_api): #定义ack方法
        resp_topic = ack_tail[0] #定义响应主题
        resp_data = ack_tail[1] #定义响应数据
        wizard_api.mqtt_publish(resp_topic, json.dumps(resp_data), 1) #调用mqtt_publish将响应数据发送给MQTT服务器，响应数据格式为{'bool': True}
    ```

  配置完成后如下图所示：  

  ![](images/2020-05-16-20-33-45.png)

<a id="附录"> </a>  

## 附录
- [导入导出配置](#导入导出配置)
- [自定义数据格式](#自定义数据格式)
- [其他网关操作](#其他网关操作)
- [Thingsboard参考流程](#Thingsboard参考流程)  

<a id="导入导出配置"> </a>  

### 导入导出配置
Device Supervisor的数据采集配置总共包含三个CSV格式的配置文件，您可以通过导入导出配置文件快速实现采集配置。各配置文件内容如下：
- device.csv：设备配置文件,详细参数如下
  - device_name：设备名称
  - protocol：通讯协议名称，如`ModbusTCP`
  - ip/serial：以太网设备填写ip地址；串口设备填写RS485或RS232
  - port：以太网设备的通讯端口号
  - rack（仅ISO on TCP设备）：设备机架号
  - slot（仅ISO on TCP设备）：设备槽号
  - slave（仅Modbus设备）：从站地址
  - byte_order（仅Modbus设备）：字节序  

  导出方式为设备列表页面的设备列表导出
  ![](images/2020-03-10-17-17-06.png)  

  示例配置如下：  

  ![](images/2020-03-10-17-15-00.png)  

  &nbsp;
  </br>
  
- var.csv:变量配置文件，详细参数如下
  - var_name：变量名称
  - device：变量所属设备
  - protocol：通讯协议名称
  - dbnumber（仅ISO on TCP设备）：DB号
  - register_type（仅ISO on TCP设备）：寄存器类型，如`DB`
  - register_addr：寄存器地址
  - register_bit：位偏移
  - data_type：数据类型
  - read_write：读写权限，包括`Read/Write`、`Write`、`Read`
  - float_repr：小数位，1~6
  - mode：采集模式，包括`realtime`、`onchange`
  - unit：单位
  - size：字符串长度
  - desc：描述
  - group：所属分组  

  导出方式为设备列表页面的变量列表导出
  ![](images/2020-03-10-17-18-01.png)  

  示例配置如下：  

  ![](images/2020-03-10-17-13-54.png)  

  &nbsp;
  </br>
  
- group.csv:分组配置文件，详细参数如下
  - group_name：分组名称
  - polling_interval：采集间隔
  - upload_interval：上传间隔  
 &nbsp;

  导出方式为分组页面的分组导出  </br>
  ![](images/2020-03-10-17-19-04.png)  

  示例配置如下：  

  ![](images/2020-03-10-17-15-33.png)

### 全局参数
你可以访问“边缘计算 > 设备监控 > 全局参数”页面配置Device Supervisor的通用设置。  
- 全局参数  
  你可以在全局参数中设置Device Supervisor的系统参数或者自行添加通配符参数  
  - `catch_recording`：最大可缓存的变量数据的MQTT消息数量，默认为`100000`  
  - `log_level`：日志等级，设置不同的日志等级可以在Device Supervisor的日志中看到不同等级的日志信息。默认为`INFO`  
  - `warning_recording`：最大可缓存的告警数据的MQTT消息数量，默认为`2000`  
  - `自定义通配符`：你可以自行添加全局参数作为云服务中的通配符使用。使用方法为`${参数名称}`，如下图所示：  
    ![](images/2020-05-16-15-34-08.png)  

    ![](images/2020-05-16-15-20-49.png)   

- 串口设置  
  你可以在串口设置中配置RS485和RS232串口的通讯参数，如下图所示：  

  ![](images/2020-05-18-17-14-19.png)  

<a id="自定义数据格式"> </a>  

### 自定义数据格式
- [发布](#发布)
- [订阅](#订阅)  
  
您可使用云服务中的高级设置功能配置MQTT主题、上报数据等参数并支持使用Python语言自定义MQTT发布和订阅主题的数据上报、处理等逻辑。无需二次开发即可实现与多种MQTT服务器进行数据上传和下发。

<a id="发布"> </a>  

#### 发布
自定义发布主题中包含以下配置项：
- `名称`：用户自定义发布名称
- `主题`：发布主题，与MQTT服务器订阅的主题保持一致
- `Qos(MQTT)`：发布Qos，建议与MQTT服务器的Qos保持一致
  - `0`：只发送一次消息，不进行重试  
  - `1`：最少发送一次消息，确保消息到达MQTT服务器
  - `2`：确保消息到达MQTT服务器且只收到一次
- `分组类型`：发布变量数据时请选择“采集”，随后在`分组`中仅能选择“采集组”；发布告警数据时请选择“告警”，随后在`分组`中仅能选择“告警组”
- `分组`：选择相应的分组后，分组下所有变量通过该发布配置将数据上传至MQTT服务器（可选择多个分组）
- `主函数`：主函数名称，即入口函数名称，与脚本中的入口函数名称保持一致
- `脚本`：使用Python代码自定义组包和处理逻辑，发布中的主函数参数包括：
  - `参数1`：Device Supervisor将采集后的变量数据发送给该参数，数据格式如下：  
    - 变量数据格式：
      ```python
      {
          'timestamp': 1589434519.5458372,  #数据产生时间戳
          'group_name': 'default'  #采集组名称
          'values':  #变量数据字典，包含PLC名称，变量名称和变量值
          {
              'S7-1200':  #PLC名称
              {
                  'Test1':  #变量名称
                  {
                      'raw_data': False,  #变量值
                      'status': 1  #采集状态，非1即采集异常
                  },
                  'Test2':
                  {
                      'raw_data': 2,
                      'status': 1
                  }
              }
          },
      }
      ```

    - 告警数据格式：  
      ```python
      {
          'timestamp': 1589434527.3628697,  #告警产生时间戳
          'group_name': 'warning'  #告警组名称
          'values':  #告警数据字典，包含告警名称等告警信息
          {
              'Warn1':  #告警名称
              {
                  'timestamp': 1589434527,  #告警产生时间戳
                  'current': 'on',  #告警状态。on:已触发，off:已消除
                  'status': 0,  #告警状态。0:已触发，1:已消除
                  'value': 33,  #告警触发时告警变量的数值
                  'alarm_content': '速度超过30！',  #告警描述
                  'level': 1  #预留字段
              }
          },
      }
      ```  

  - `参数2`：该参数为Device Supervisor提供的api接口，参数说明见[Device Supervisor的api接口说明](#device-supervisor-api-description)  

以下是常见的自定义发布方法示例<font color=#FF0000>（请勿将`mqtt_publish`或`save_data`方法与`return`命令同时使用）</font>：

<a id="sub-example1"> </a>

- 发布示例1：使用`return`方式上传变量数据  
  本示例实现了使用`return`方式上传变量数据时，将处理后的变量数据使用`return`命令发送给Device Supervisor。Device Supervisor自行根据发布中配置的主题和Qos将变量数据按照采集时间顺序上传至MQTT服务器。如果发送失败则缓存变量数据等待MQTT连接正常后按采集时间顺序上传至MQTT服务器。发布和代码配置示例如下：  
    
  ![](images/2020-05-16-09-51-27.png)  

  ```python
  import logging
  """
  在网关中打印日志通常有两种办法。
  1.import logging：使用logging.info(XXX)打印日志，该方法的日志显示不受全局参数页面中的日志等级参数控制。
  2.from common.Logger import logger：使用logger.info(XXX)打印日志，该方法的日志显示受全局参数页面中的日志等级参数控制。
  """

  def test_upload(data, global_argv, mqtt_publish=None, save_data=None): #定义发布主函数
      value_list = [] #定义数据列表
      for device, val_dict in data['values'].items(): #遍历values字典，该字典中包含设备名称和设备下的变量数据
          value_dict = { #自定义数据字典
                        "Device": device,
                        "timestamp": data["timestamp"],
                        "Data": {}
                        }
          for id, val in val_dict.items(): #遍历变量数据，为Data字典赋值
              value_dict["Data"][id] = val["raw_data"]
          value_list.append(value_dict) #依次将设备名称和设备的数据添加到value_list中
      logging.info(value_list) #在App日志中打印value_list
      return value_list #将value_list发送给App，App将自行按照采集时间顺序上传至MQTT服务器。如果发送失败则缓存数据等待连接恢复后按时间顺序上传至MQTT服务器
    ```  

<a id="sub-example2"> </a>

- 发布示例2：使用`return`方式上传告警数据  
  本示例实现了上传告警数据。发布和代码配置示例如下：  

  ![](images/2020-05-16-11-12-54.png)  

  ```python
  import logging
  """
  在网关中打印日志通常有两种办法。
  1.import logging：使用logging.info(XXX)打印日志，该方法的日志显示不受全局参数页面中的日志等级参数控制。
  2.from common.Logger import logger：使用logger.info(XXX)打印日志，该方法的日志显示受全局参数页面中的日志等级参数控制。
  """
  
  def alarms_upload_test(data_collect, wizard_api): #定义发布主函数
      alarm_list = [] #定义告警列表
      for alarm_name, alarm_info in data_collect['values'].items(): #遍历values字典，该字典中包含告警名称和告警时间等信息
          alarm_dict = { #自定义数据字典
                        "Alarm_name": alarm_name,
                        "timestamp": data_collect["timestamp"],
                        "Alarm_status": alarm_info['current'],
                        "Alarm_value": alarm_info['value'],
                        "Alarm_content": alarm_info['alarm_content']
                        }
          alarm_list.append(alarm_dict)
      logging.info(alarm_list) #在App日志中打印alarm_list
      return alarm_list #将alarm_list发送给App，App将自行按照采集时间顺序上传至MQTT服务器。如果发送失败则缓存数据等待连接恢复后按时间顺序上传至MQTT服务器
  ```

<a id="sub-example3"> </a>

- 发布示例3：使用`mqtt_publish`上传变量数据并使用`save_data`存储上传失败的变量数据  
  本示例实现了使用`mqtt_publish`方法将变量数据上传至MQTT服务器，当MQTT连接异常导致变量数据上传失败时，使用`save_data`方法存储主题、qos和变量数据至数据库，存储的变量数据会在MQTT连接正常时按照先存先传的方式通过存储数据中的主题和Qos上传至MQTT服务器。发布和代码配置示例如下：  

  ![](images/2020-05-16-10-26-27.png)  

  ```python
  from common.Logger import logger
  import json
  from datetime import datetime
  """
  在网关中打印日志通常有两种办法。
  1.import logging：使用logging.info(XXX)打印日志，该方法的日志显示不受全局参数页面中的日志等级参数控制。
  2.from common.Logger import logger：使用logger.info(XXX)打印日志，该方法的日志显示受全局参数页面中的日志等级参数控制。
  """
  
  def vars_cache_test(data_collect, wizard_api): #定义发布主函数
      value_list = [] #定义数据列表
      utc_time = datetime.utcfromtimestamp(data_collect["timestamp"]) #转换LINUX时间戳为UTC时间
      for device, val_dict in data_collect['values'].items(): #遍历values字典，该字典中包含设备名称和设备下的变量数据
          value_dict = { #自定义数据字典
                        "DeviceSN": device,
                        "Time": utc_time.strftime('%Y-%m-%dT%H:%M:%S.%fZ'),
                        "Data": {}
                         }
          for id, val in val_dict.items(): #遍历变量数据，为Data字典赋值
              value_dict["Data"][id] = val["raw_data"]
          value_list.append(value_dict) #依次将设备名称和设备的数据添加到value_list中
      if not wizard_api.mqtt_publish("v1/xxx/yyy", json.dumps(value_list), 1): #调用wizard_api模块中的mqtt_publish方法将value_list数据通过主题“v1/xxx/yyy”，qos等级1发送至MQTT服务器并检测是否发送成功
          value_list = {"topic": "v1/xxx/yyy", "qos": 1, "payload": value_list}
          wizard_api.save_data(value_list) #发送失败则存储数据，等待连接恢复后将按时间顺序上传存储数据
          logger.info("Save data:%s" %value_list)
      logger.info(value_list) #在App日志中打印value_list
  ```

<a id="sub-example4"> </a>

- 发布示例4：使用`mqtt_publish`上传变量数据并使用`save_data`存储上传失败的变量数据  
  本示例实现了使用`mqtt_publish`方法将变量数据上传至MQTT服务器，当MQTT连接异常导致变量数据上传失败时，使用`save_data`方法存储变量数据和分组名称，存储的变量数据会在MQTT连接正常时按照先存先传的方式将变量数据按照分组在云服务中关联的主题和Qos上传至MQTT服务器<font color=#FF0000>（请勿将`mqtt_publish`或`save_data`方法与`return`命令同时使用）</font>。发布和代码配置示例如下： 

  ![](images/2020-05-16-10-27-18.png)  

  ```python
  from common.Logger import logger
  import json
  from datetime import datetime
  """
  在网关中打印日志通常有两种办法。
  1.import logging：使用logging.info(XXX)打印日志，该方法的日志显示不受全局参数页面中的日志等级参数控制。
  2.from common.Logger import logger：使用logger.info(XXX)打印日志，该方法的日志显示受全局参数页面中的日志等级参数控制。
  """
  
  def vars_cache_test(data_collect, wizard_api): #定义发布主函数
      value_list = [] #定义数据列表
      utc_time = datetime.utcfromtimestamp(data_collect["timestamp"]) #转换LINUX时间戳为UTC时间
      for device, val_dict in data_collect['values'].items(): #遍历values字典，该字典中包含设备名称和设备下的变量数据
          value_dict = { #自定义数据字典
                        "DeviceSN": device,
                        "Time": utc_time.strftime('%Y-%m-%dT%H:%M:%S.%fZ'),
                        "Data": {}
                         }
          for id, val in val_dict.items(): #遍历变量数据，为Data字典赋值
              value_dict["Data"][id] = val["raw_data"]
          value_list.append(value_dict) #依次将设备名称和设备的数据添加到value_list中
      if not wizard_api.mqtt_publish("v1/xxx/yyy", json.dumps(value_list), 1): #调用wizard_api模块中的mqtt_publish方法将value_list数据通过主题“v1/xxx/yyy”，qos等级1发送至MQTT服务器并检测是否发送成功
          wizard_api.save_data(value_list,'default') #发送失败则存储数据，等待连接恢复后将按时间顺序上传存储数据
          logger.info("Save data:%s" %value_list)
      logger.info(value_list) #在App日志中打印value_list
  ```

<a id="sub-example5"> </a>

- 发布示例5：使用`get_tag_config`获取设备、变量和告警点表  
  本示例实现了每次重启App时使用`get_tag_config`方法获取设备、变量和告警点表并分别上传至MQTT服务器。发布和代码配置示例如下：  
  
  ![](images/2020-05-16-15-04-00.png)

  ```python
  from common.Logger import logger
  import json
  """
  在网关中打印日志通常有两种办法。
  1.import logging：使用logging.info(XXX)打印日志，该方法的日志显示不受全局参数页面中的日志等级参数控制。
  2.from common.Logger import logger：使用logger.info(XXX)打印日志，该方法的日志显示受全局参数页面中的日志等级参数控制。
  """
  
  IS_UPLOAD_CONFIG = True  #定义变量用于判断是否需要获取并上传点表
  
  def upload_tagconfig(recv, wizard_api):  #定义发布主函数
      global IS_UPLOAD_CONFIG  #声明变量为全局变量
      if IS_UPLOAD_CONFIG:  #判断是否需要获取并上传点表
          wizard_api.get_tag_config(tagconfig)  #定义获取点表的回调函数tagconfig
          IS_UPLOAD_CONFIG = False  #获取并上传点表后不再上传点表
  
  def tagconfig(config, tail, wizard_api): #定义获取点表的回调函数
      logger.info(config) #打印点表信息，包含设备、分组、变量和告警点表
      deviceConfiguration_list = [] #定义设备点表列表
      for device in config['devices']: #遍历设备点表
          deviceInfo = {} #定义设备信息字典
          if device['protocol'] == "ModbusTCP": #判断设备通讯协议是否为ModbusTCP
              deviceInfo["Device"] = device['device_name']
              deviceInfo["PLCProtocol"] = device['protocol']
              deviceInfo["IP Address"] = device['ip']
              deviceInfo["Port"] = device['port']
              deviceInfo["SlaveAddress"] = device['slave']
              deviceInfo["Endian"] = device['byte_order']
              deviceConfiguration_list.append(deviceInfo) #依次将设备信息添加到deviceConfiguration_list中
          elif device['protocol'] == "ModbusRTU": #判断设备通讯协议是否为ModbusRTU
              deviceInfo["Device"] = device['device_name']
              deviceInfo["PLCProtocol"] = device['protocol']
              deviceInfo["Port"] = device['serial']
              deviceInfo["Baudrate"] = device['baudrate']
              deviceInfo["DataBits"] = device['bytesize']
              deviceInfo["Parity"] = device['parity']
              deviceInfo["StopBits"] = device['stopbits']
              deviceInfo["SlaveAddress"] = device['slave']
              deviceInfo["Endian"] = device['byte_order']
              deviceConfiguration_list.append(deviceInfo)
          elif device['protocol'] == "ISO-on-TCP": #判断设备通讯协议是否为ISO-on-TCP
              deviceInfo["Device"] = device['device_name']
              deviceInfo["PLCProtocol"] = device['protocol']
              deviceInfo["IP Address"] = device['ip']
              deviceInfo["Port"] = device['port']
              deviceInfo["Rack"] = device['rack']
              deviceInfo["Slot"] = device['slot']
              deviceConfiguration_list.append(deviceInfo)
      logger.info(deviceConfiguration_list)
      wizard_api.mqtt_publish("Config/DeviceInfo", json.dumps(deviceConfiguration_list), 1) #调用wizard_api模块中的mqtt_publish方法将deviceConfiguration_list数据通过主题“Config/DeviceInfo”，qos等级1发送至MQTT服务器
  
  
      tagConfiguration_list = [] #定义变量点表列表
      device_group_info_dict = {} #定义设备下的分组信息字典
      group_info_dict = {} #定义分组信息字典
      for groupinfo in config['groups']: #遍历点表中的分组
          group_info_dict[groupinfo["group_name"]] = groupinfo #建立组名与分组信息的对应关系
      for device in config['devices']: #遍历点表中的设备
          group_list= [] #定义设备下的分组列表
          for var in config['vars']: #遍历点表中的变量
              if device['device_name'] == var['device'] and var['group'] not in group_list: #判断设备下存在变量，且该变量所属的分组未存在与group_list中，则将该分组添加到group_list中
                  group_list.append(var['group'])
          device_group_info_dict[device['device_name']] = group_list #建立设备与设备下的分组列表的对应关系
      for device, group_list in device_group_info_dict.items(): #遍历设备下的分组信息字典
          if group_list == []: #如果设备下的分组列表为空，即该设备下未定义变量，则跳过该设备
              continue
          tagConfiguration = {} #定义变量点表字典
          tagConfiguration["Device"] = device #添加设备信息
          tagConfiguration["Collections"] = []
          for group in group_list: #遍历设备下的分组并添加分组信息
              group_info = {}
              group_info["CollectionName"] = group
              group_info["SampleRate"] = group_info_dict[group]["polling_interval"]
              group_info["PublishInterval"] = group_info_dict[group]["upload_interval"]
              group_info["TagData"] = []
              tagConfiguration["Collections"].append(group_info)
              for var in config['vars']: #遍历点表中的变量
                  if var['device'] != device or var['group'] != group: #如果该变量不属于当前设备和分组，则跳过该变量
                      continue
                  index_number = tagConfiguration["Collections"].index(group_info) #获取该分组在tagConfiguration["Collections"]中的索引
                  data_info = {} #定义变量信息字典
                  data_info["Tag"] = var["var_name"]
                  data_info["Address"] = var["address"]
                  data_info["ValueType"] = var["data_type"]
                  data_info["AccessLevel"] = var["read_write"]
                  data_info["Mode"] = var["mode"]
                  data_info["Unit"] = var["unit"]
                  data_info["Description"] = var["desc"]
                  tagConfiguration["Collections"][index_number]["TagData"].append(data_info) #将变量信息添加至指定分组的TagData中
          tagConfiguration_list.append(tagConfiguration) #依次将设备点表添加至tagConfiguration_list中
      logger.info(tagConfiguration_list) #打印变量点表
      for tagConfiguration in tagConfiguration_list: #遍历每个设备下的变量点表并依次上传至MQTT服务器
          wizard_api.mqtt_publish("Config/TagConfiguration", json.dumps(tagConfiguration), 1)
  
  
      alarmConfiguration_list = [] #定义告警点表
      for alarm in config['warning']: #遍历点表中的告警
          alarmInfo = {} #定义告警信息字典
          alarmInfo['Warn_name'] = alarm['warn_name']
          alarmInfo['Group'] = alarm['group']
          alarmInfo['Alarm_content'] = alarm['alarm_content']
          alarmInfo['Condition1'] = alarm['condition1']
          alarmInfo['Operand1'] = alarm['operand1']
          alarmInfo['Combine_method'] = alarm['combine_method']
          alarmInfo['Condition2'] = alarm['condition2']
          alarmInfo['Operand2'] = alarm['operand2']
          alarmInfo['Device'] = alarm['device']
          alarmInfo['Var_name'] = alarm['var_name']
          alarmInfo['Address'] = alarm['address']
          alarmConfiguration_list.append(alarmInfo) #依次将告警信息添加至alarmConfiguration_list中
      logger.info(alarmConfiguration_list) #打印告警点表
      wizard_api.mqtt_publish("Config/AlarmInfo", json.dumps(alarmConfiguration_list), 1) #调用wizard_api模块中的mqtt_publish方法将alarmConfiguration_list数据通过主题“Config/AlarmInfo”，qos等级1发送至MQTT服务器
  ```

<a id="sub-example6"> </a>

- 发布示例6：使用`get_global_parameter`获取全局参数设置  
  本示例实现了获取全局参数中设置的参数`device_id`，并通过通配符`${device_id}`的配置方式配置MQTT主题。发布和代码配置示例如下：  

  ![](images/2020-05-16-15-34-08.png)  

  ![](images/2020-05-16-15-20-49.png)  

  ```python
  import logging
  """
  在网关中打印日志通常有两种办法。
  1.import logging：使用logging.info(XXX)打印日志，该方法的日志显示不受全局参数页面中的日志等级参数控制。
  2.from common.Logger import logger：使用logger.info(XXX)打印日志，该方法的日志显示受全局参数页面中的日志等级参数控制。
  """
  
  def vars_upload_test(data_collect, wizard_api): #定义发布主函数
      global_parameter = wizard_api.get_global_parameter() #定义全局参数变量
      logging.info(global_parameter) #打印全局参数变量
      value_list = [] #定义数据列表
      for device, val_dict in data_collect['values'].items(): #遍历values字典，该字典中包含设备名称和设备下的变量数据
          value_dict = { #自定义数据字典
                        "Device": device,
                        "DeviceID": global_parameter["device_id"], #获取全局变量中定义的设备ID
                        "timestamp": data_collect["timestamp"],
                        "Data": {}
                        }
          for id, val in val_dict.items(): #遍历变量数据，为Data字典赋值
              value_dict["Data"][id] = val["raw_data"]
          value_list.append(value_dict) #依次将设备名称和设备的数据添加到value_list中
      logging.info(value_list) #在App日志中打印value_list，数据格式为[{'Device': 'S7-1200', 'DeviceID': '1', 'timestamp': 1589538347.5604711, 'Data': {'Test1': False, 'Test2': 12}}]
      return value_list #将value_list发送给App，App将自行按照采集时间顺序上传至MQTT服务器。如果发送失败则缓存数据等待连接恢复后按时间顺序上传至MQTT服务器
  ```

<a id="订阅"> </a>  

#### 订阅
自定义订阅中包含以下项：
- `名称`：自定义订阅名称
- `主题`：订阅主题，与MQTT服务器发布的数据主题保持一致
- `Qos(MQTT)`：订阅Qos，建议与MQTT服务器的Qos保持一致
- `主函数`：主函数名称，即入口函数名称，与脚本中的入口函数名称保持一致
- `脚本`：使用Python代码自定义组包和处理逻辑，订阅中的主函数参数包括：
  - `参数1`：该参数为接收到的主题，数据类型为`string`
  - `参数2`：该参数为接收到的数据，数据类型为`string`
  - `参数3`：该参数为Device Supervisor提供的api接口，参数说明见[Device Supervisor的api接口说明](#device-supervisor-api-description)  

以下是三个常见的自定义订阅方法示例：

<a id="pub-example1"> </a>

- 订阅示例1：下发变量名称和变量值写入PLC数据且不返回写入结果  
  本示例实现了从MQTT服务器下发指定命令修改变量数值，发布和代码配置示例如下：  

  ![](images/2020-05-16-15-41-58.png)  

  ```python
  import logging
  import json
  
  def ctl_test(topic, payload, wizard_api): #定义订阅主函数
      logging.info(topic) #打印订阅主题,假定topic为write/plc
      logging.info(payload) #打印订阅数据,假定payload数据为{"method":"setValue", "TagName":"SP1", "TagValue":12.3}
      payload = json.loads(payload) #反序列化订阅数据
      if payload["method"] == "setValue": #检测是否为写入数据
          message = {payload["TagName"]:payload["TagValue"]} #定义下发消息，包括下发的变量名称和变量值
          wizard_api.write_plc_values(message) #调用write_plc_values方法，将message字典中的数据下发至指定变量
  ```

<a id="pub-example2"> </a>

- 订阅示例2：下发设备名称，变量名称和变量值写入PLC数据且不返回写入结果
  本示例实现了从MQTT服务器下发指定命令修改变量数值，发布和代码配置示例如下：  

  ![](images/2020-05-16-15-50-25.png)  

  ```python
  import logging
  import json
  
  def test_ctl(topic, payload, wizard_api): #定义订阅主函数
      logging.info(topic) #打印订阅主题
      logging.info(payload) #打印订阅数据
      #假定payload数据为{"method":"setValue","Device":"Modbus_test", "TagName":"SP1", "TagValue":12.3}
      payload = json.loads(payload) #反序列化订阅数据
      data_dict = {payload["TagName"]:payload["TagValue"]} #定义下发的数据字典，包含下发的变量名称和变量值
      var_device = payload["Device"] #定义设备名称
      if payload["method"] == "setValue": #检测是否为写入数据
          message = {var_device:data_dict} #定义下发消息，包括设备名称和下发的数据字典
          wizard_api.write_plc_values(message) #调用write_plc_values方法，将message字典中的数据下发至指定变量
  ```

<a id="pub-example3"> </a>

- 订阅示例3：写入变量数据并返回写入结果  
  本示例实现了从MQTT服务器下发指定命令修改变量数值并返回修改结果，发布和代码配置示例如下：  

  ![](images/2020-05-16-15-57-57.png)  

  ```python
  import logging
  import json
  
  def ctl_test(topic, payload, wizard_api): #定义订阅主函数
      logging.info(topic) #打印订阅主题，假定topic为request/v1
      logging.info(payload) #打印订阅数据
      #假定payload数据为{"method":"setValue","Device":"Modbus_test", "TagName":"SP1", "TagValue":12.3}
      payload = json.loads(payload) #反序列化订阅数据
      data_dict = {payload["TagName"]:payload["TagValue"]} #定义下发的数据字典，包含下发的变量名称和变量值
      var_device = payload["Device"] #定义设备名称
      if payload["method"] == "setValue": #检测是否为写入数据
          message = {var_device:data_dict} #定义下发消息，包括设备名称和下发的数据字典
          ack_tail = [topic.replace('request', 'response'), message] #定义确认数据，包括响应的主题和消息
          logging.info(message)
          wizard_api.write_plc_values(message, ack, ack_tail, timeout = 10) #调用write_plc_values方法，将message字典中的数据下发至指定变量；定义回调函数ack并将ack_tail传递给回调函数ack
  
  def ack(send_result, ack_tail, wizard_api): #定义ack函数
      topic = ack_tail[0] #定义响应主题
      if not send_result: #检测下发是否超时
          resp_data = {"Status":"timeout", "Data":ack_tail[1]} #定义下发超时的响应数据
      else:
          resp_data = {"Status":send_result[0]["result"], "Data":ack_tail[1]} #定义下发未超时的响应数据
      wizard_api.mqtt_publish(topic, json.dumps(resp_data), 0) #调用mqtt_publish将响应数据发送给MQTT服务器
  ```

- 订阅示例4：立即召回数据  
  本示例实现了从MQTT服务器下发指定命令时，立即读取所有变量数值并发送至MQTT服务器，发布和代码配置示例如下：  

  ![](images/2020-05-16-17-11-32.png)  

  ```python
  from common.Logger import logger
  import json
  
  def recall_test(topic, payload, wizard_api): #定义订阅主函数
      logger.info(topic) #打印订阅主题，假定topic为recall/v1
      payload = json.loads(payload) #反序列化订阅数据
      logger.info(payload) #打印订阅数据，假定payload数据为{"command":"Upload immediately"}
      if payload["command"] == "Upload immediately": #检测是否需要数据召回
          wizard_api.recall_data(recall) #调用recall_data方法，定义回调函数recall
  
  def recall(data_collect, tail, wizard_api): #定义recall函数
      logger.info(data_collect) #打印读取到的数据
      value_list = [] #定义数据列表
      for device, val_dict in data_collect["values"].items(): #遍历values字典，该字典中包含设备名称和设备下的变量数据
          value_dict = { #自定义数据字典
                        "DeviceSN": device,
                        "timestamp": data_collect["timestamp"],
                        "Data": []
                         }
          for id, val in val_dict.items(): #遍历变量数据，为Data列表赋值
              var_dict = {} #定义变量字典
              var_dict[id] = val["raw_data"]
              value_dict["Data"].append(var_dict) #依次将变量字典添加到value_dict中
          value_list.append(value_dict) #依次将数据字典添加到value_list中
      logger.info(value_list) #打印value_list 
      wizard_api.mqtt_publish("v1/xxx/yyy", json.dumps(value_list), 1) #调用wizard_api模块中的mqtt_publish方法将value_list数据通过主题“v1/xxx/yyy”，qos等级1发送至MQTT服务器
  ```

<a id="device-supervisor-api-description"> </a>  

#### Device Supervisor的api接口说明
Device Supervisor提供的api接口，包含以下方法：
- `mqtt_publish`：MQTT发布消息方法，用于将指定数据通过相应的主题发送到MQTT服务器并返回发送结果：发送成功（True），发送失败（False），使用示例请参考[发布示例3](#pub-example3)。该方法包含以下参数：  
  - `参数1`：MQTT主题，数据类型为`string`。通过至该主题发送数据到MQTT服务器  
  - `参数2`：需要发送的数据  
  - `参数3`：qos等级（包括0/1/2）

- `save_data`：存储数据至数据库方法，被存储的数据将在MQTT连接正常时按先存先传的方式上传至MQTT服务器  使用示例请参考[发布示例3](#pub-example3)，[发布示例4](#pub-example4)。该方法包含以下参数：
  - `参数1`：需要存储的数据。如果调用`save_data`时只提供了参数1，则`参数1`的数据类型为`dict`，在储存的数据中需具备以`topic`、`qos`和`payload`为键的键值对，当MQTT连接正常后将以存储数据中的`topic`和`qos`将`payload`发送至MQTT服务器。  
  - `参数2`（可选参数`group`）：需要存储的数据的分组名称，数据类型为`string`。如果调用`save_data`时提供`参数2`，则将以该分组在云服务中配置的主题和qos上传`参数1`至MQTT服务器。

- `write_plc_values`：下发数据至指定变量方法并支持返回修改结果，使用示例请参考[订阅示例1](#sub-example1)，[订阅示例2](#sub-example2),[订阅示例3](#sub-example3)。该方法包含以下参数：
  - `参数1`：下发数据，该数据可以有两种形式：
    - `形式1`：传入一个以变量名称和变量值为键值对的`dict`。使用此方法修改变量数值时，需要确保该变量的名称在设备列表中唯一，数据格式示例如下：
      ```python
      {
          "SP1": 12.3,  #变量名称和变量值的键值对
          "SP2": 12.4
      }
      ```
    - `形式2`：传入一个包含设备名称、变量名称和变量值的`dict`，数据格式示例如下：
      ```python
      {
          "S7-1200":  #设备名称
          {
              "SP1": 12.3,  #变量名称和变量值的键值对
              "SP2": 12.4
          }
      }
      ```
  - `参数2`（可选参数`callback`）：返回修改结果的回调函数名称，回调函数说明见[回调函数说明](#callback-function-description)  
  - `参数3`（可选参数`tail`）：已有`参数2`时，可将需要传递给返回修改结果的回调函数的数据赋值给`参数3`
  - `参数4`（可选参数`timeout`）：写入超时时间，数据类型为`整数`。默认为60秒

- `get_tag_config`：获取点表配置方法，点表配置包括PLC、变量、分组和告警配置，使用示例请参考[发布示例5](#pub-example5)。该方法包含以下参数：
  - `参数1`：获取点表配置的回调函数的名称，回调函数说明见[回调函数说明](#callback-function-description)
  - `参数2`（可选参数`tail`）：可将需要传递给获取点表配置的回调函数的数据赋值给`参数2`
  - `参数3`（可选参数`timeout`）：获取点表超时时间，数据类型为`整数`。默认为60秒

- `recall_data`：立即读取所有变量数值方法，使用示例请参考[订阅示例4](#sub-example4)。该方法包含以下参数：  
  - `参数1`：立即读取所有变量数值的回调函数的名称，回调函数说明见[回调函数](#callback-function-description)
  - `参数2`（可选参数`tail`）：可将需要传递给立即读取所有变量数值的回调函数的数据赋值给`参数2`
  - `参数3`（可选参数`timeout`）：立即读取所有变量的超时时间，数据类型为`整数`。默认为60秒

- `get_global_parameter`：获取全局参数设置方法，使用示例请参考[发布示例6](#pub-example6)。该方法会返回一个全局参数设置的字典，数据格式如下：
  
  ```python
  {
      'gateway_sn': 'GT9021934001090',
      'log_level': 'INFO',
      'catch_recording': 100000,
      'warning_recording': 2000,
      'device_id': '1'
  }
  ```

<a id="callback-function-description"> </a>  

#### 回调函数说明
1. `write_plc_values`回调函数说明  
`write_plc_values`回调函数包含以下参数：
    - `参数1`: `write_plc_values`方法的写入结果
      - 写入超时时返回值为
        ```python
        ("error", -110, "timeout")
        ```
      - 写入成功时返回值格式为：
        ```python
        [
        {
            'value': 12,
            'device': 'S7-1200',
            'var_name': 'Test2',
            'result': 'OK',
            'error': ''
        }]
        ```;
      - 写入失败时返回值格式为：
        ```python
        [
        {
            'value': 12.3,
            'device': 'Modbus_test',
            'var_name': 'SP1',
            'result': 'Failed',
            'error': 'No device found. Modbus_test'
        }]
        ```
    - `参数2`：`write_plc_values`方法中配置的`参数3`，如果未在`write_plc_values`中配置`参数3`，则该参数为`None`
    - `参数3`：该参数为Device Supervisor提供的api接口，参数说明见[Device Supervisor的api接口说明](#device-supervisor-api-description)  

2. `get_tag_config`回调函数说明  
`get_tag_config`回调函数包含以下参数：
    - `参数1`: `get_tag_config`方法返回的点表配置。获取点表超时时返回值为`("error", -110, "timeout")`，正常返回点表配置时数据格式如下：  
      ```python
      {
          'devices': [  #设备点表
          {
              'protocol': 'ISO-on-TCP',  #设备协议
              'device_name': 'S7-1200',  #设备名称
              'ip': '10.5.16.73',  #IP地址
              'port': 102,  #端口号
              'rack': 0,  #机架号
              'slot': 0,  #槽号
              'id': '6358f50294dc11ea8d890018050ff046'  #设备ID
          }],
          'groups': [  #分组点表
          {
              'group_name': 'warning',  #分组名称
              'polling_interval': 10,  #采集间隔
              'upload_interval': '',  #上报间隔间隔
              'group_type': 'alarm',  #分组类型。collect：采集组，alarm：告警组
              'id': '84c371902eb911eabab11a4f32d1ee44'  #分组ID
          }],
          'warning': [  #告警点表
          {
              'warn_name': 'Warn1',  #告警名称
              'group': 'warning',  #告警所属分组
              'quotes': 1,  #告警变量来源。0： 直接使用地址，1：引用地址
              'device': 'S7-1200',  #告警变量所属设备
              'alarm_content': '速度超过30！',  #告警描述
              'condition1': 'Gt',  #告警条件1。Eq：等于,Neq：不等于,Gt：大于,Gne：大于等于,Lne：小于等于,Lt：小于
              'operand1': '30',  #告警阈值1
              'combine_method': 'And',  #告警条件连接方式。None：空，And：&&，Or：||
              'condition2': 'Lt',  #告警条件2
              'operand2': '50',  #告警阈值1
              'var_name': 'Test2',  #告警变量名称
              'var_id': '96c93c3094dd11eabd400018050ff046',  #告警变量ID
              'size': 1,  #告警变量的数据类型为STRING时的字符串长度
              'float_repr': 2,  #告警变量的数据类型为FLOAT时变量小数点后的数据长度
              'id': '9165ed78943e11ea8a000018050ff046',  #告警ID
              'address': 'DB6.2',  #告警变量地址
              'protocol': 'ISO-on-TCP',  #告警设备通讯协议
              'data_type': 'WORD',  #告警变量数据类型
              'register_type': 'DB',  #告警变量寄存器类型
              'register_addr': 2,  #告警变量寄存器地址
              'read_write': 'read/write',  #告警读写权限。read：只读、write：只写、'read/write：可读可写
              'mode': 'realtime',  #告警变量采集模式
              'unit': '',  #告警变量单位
              'desc': '',  #告警变量描述
              'dbnumber': 6,  #告警变量寄存器类型为DB时变量的DB号
              'register_bit': ''  #告警变量数据类型为BOOL或BIT时变量的位偏移
          }],
          'vars': [  #变量点表
          {
              'device': 'S7-1200',  #变量所属设备的名称
              'protocol': 'ISO-on-TCP',  #变量所示设备的通讯协议
              'data_type': 'BOOL',  #变量数据类型
              'register_type': 'I',  #变量寄存器类型
              'var_name': 'Test1',  #变量名称
              'register_addr': 0,  #变量寄存器地址
              'read_write': 'read/write',  #变量读写权限。read：只读、write：只写、'read/write：可读可写
              'mode': 'realtime',  #变量采集模式
              'unit': '',  #变量单位
              'desc': '',  #变量描述
              'group': 'default',  #变量所属分组
              'register_bit': 0,  #变量数据类型为BOOL或BIT时变量的位偏移
              'size': 1,  #变量的数据类型为STRING时的字符串长度
              'float_repr': 2,  #变量的数据类型为FLOAT时变量小数点后的数据长度
              'dbnumber': 0,  #变量寄存器类型为DB时变量的DB号
              'id': 'a1d9439a94dc11eaa2830018050ff046',  #变量ID
              'address': 'I0.0'  #变量地址
          },
          {
              'device': 'S7-1200',
              'protocol': 'ISO-on-TCP',
              'data_type': 'WORD',
              'register_type': 'DB',
              'var_name': 'Test2',
              'register_addr': 2,
              'read_write': 'read/write',
              'mode': 'realtime',
              'unit': '',
              'desc': '',
              'group': '2222',
              'dbnumber': 6,
              'size': 1,
              'float_repr': 2,
              'register_bit': '',
              'id': '96c93c3094dd11eabd400018050ff046',
              'address': 'DB6.2'
          }]
      }
      ```

    - `参数2`：`write_plc_values`方法中配置的`参数3`，如果未在`write_plc_values`中配置`参数3`，则该参数为`None`
    - `参数3`：该参数为Device Supervisor提供的api接口，参数说明见[Device Supervisor的api接口说明](#device-supervisor-api-description)  

3. `recall_data`回调函数说明  
  `recall_data`回调函数包含以下参数：
    - `参数1`: `recall_data`方法返回的变量数据。获取变量数据超时时返回值为`("error", -110, "timeout")`，正常返回变量数据时数据格式如下：
      ```python
      {
          'timestamp': 1589507333.2521989,  #数据产生时间戳
          'values':  #数据字典，包括PLC名称，变量名称和变量值
          {
              'S7-1200':  #PLC名称
              {
                  'Test1':  #变量名称
                  {
                      'raw_data': False,  #变量值
                      'status': 1  #采集状态，非1即采集异常
                  },
                  'Test2':
                  {
                      'raw_data': 33,
                      'status': 1
                  }
              }
          }
      }
      ```

    - `参数2`：`write_plc_values`方法中配置的`参数3`，如果未在`write_plc_values`中配置`参数3`，则该参数为`None`

    - `参数3`：该参数为Device Supervisor提供的api接口，参数说明见[Device Supervisor的api接口说明](#device-supervisor-api-description)   

<a id="其他网关操作"> </a>  

### 其他网关操作
关于网关的其他常用操作请查看[IG501快速使用手册](http://doc.ig.inhand.com.cn/zh_CN/latest/IG501%E5%BF%AB%E9%80%9F%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C.html)或[IG902快速使用手册](http://doc.ig.inhand.com.cn/zh_CN/latest/IG902%E5%BF%AB%E9%80%9F%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C.html)。

<a id="Thingsboard参考流程"> </a>  

### Thingsboard参考流程
- [添加设备和资产](#添加设备和资产)
- [传输PLC数据到Thingsboard设备](#传输PLC数据到Thingsboard设备)
- [配置可视化仪表板](#配置可视化仪表板)  
   
<a id="添加设备和资产"> </a>  

#### 添加设备和资产
访问`https://demo.thingsboard.io/login`，输入登录账号和密码。如果未注册过账号则需要先注册账号后再登录（注册账号时需要能够访问海外网络，否则可能无法正常注册）。  

![](images/2020-02-26-16-27-53.png)  <br/>

登录后，进入属性页面修改语言为简体中文
![](images/2020-02-26-09-50-27.png)

- 添加一个资产
![](images/2020-02-26-10-27-13.png)  

  ![](images/2020-02-26-10-27-40.png)  

  添加成功后如下图所示：  <br/>

  ![](images/2020-02-26-09-52-39.png)  
&nbsp;
</br>

- 添加一个设备
![](images/2020-02-26-09-54-14.png)  

  ![](images/2020-02-26-09-54-27.png)  
&nbsp;

  建立资产与设备的关联
![](images/2020-02-26-09-56-01.png)  

  添加完成后如下图所示
![](images/2020-02-26-09-56-57.png)  
&nbsp;

<a id="传输PLC数据到Thingsboard设备"> </a>  
  
#### 传输PLC数据到Thingsboard设备
资产和设备配置完成后，复制已添加设备的访问令牌并粘贴至网关的云服务页面的用户名参数中以将数据传输至Thingsboard中的ISO on TCP设备。
![](images/2020-02-26-09-58-18.png)  

随后可在设备的最新遥测中查看已上传的数据。
![](images/2020-02-26-10-38-50.png)

<a id="配置可视化仪表板"> </a>  

#### 配置可视化仪表板
- [添加仪表板](#添加仪表板)
- [添加趋势图](#添加趋势图)
- [添加开关](#添加开关)
<a id="添加仪表板"> </a>  

- 添加仪表板  
  
  点击添加仪表板，选择创建新的仪表板
![](images/2020-02-26-10-40-22.png)  
    
  ![](images/2020-02-26-10-40-58.png)  

  添加完成后单击配置仪表板
![](images/2020-02-26-10-00-23.png)  

  点击进入编辑模式
![](images/2020-02-26-10-00-45.png)  

  为仪表板添加实体别名
![](images/2020-02-26-10-01-13.png)  

  在添加别名页面按下图配置：
![](images/2020-02-26-10-02-41.png)  

  配置完成后保存配置即可。
![](images/2020-02-26-10-03-00.png)  

&nbsp;

<a id="添加趋势图"> </a>  
- 添加趋势图  
  在仪表板中点击进入编辑并点击添加新的部件  
  ![](images/2020-03-19-18-01-37.png)  
  
  ![](images/2020-02-26-10-03-23.png)
  在部件中选择Charts并点击Timeseries-Flot。
![](images/2020-02-26-10-03-51.png)  

  在图表的数据页面为趋势图添加展示数据。
![](images/2020-02-26-10-14-28.png)  

  ![](images/2020-02-26-10-15-50.png)  

  添加完成后如下图所示：
![](images/2020-02-26-10-18-00.png)
&nbsp;

<a id="添加开关"> </a>  
- 添加开关  
  
  点击添加，选择创建新部件以添加一个控制开关。
![](images/2020-02-26-10-21-47.png)  

  在部件中选择Control widgets并点击Switch control。
![](images/2020-02-26-10-20-10.png)  

  随后选择目标设备。
![](images/2020-02-26-10-20-36.png)

  配置完成后调整部件的大小和布局并保存。
![](images/2020-02-26-10-22-40.png)
  <br> </br>
  随后可以通过开关下发控制命令以及通过趋势图查看数据趋势。
![](images/2020-02-26-10-22-59.png)  


## FAQ  
- 查看云服务脚本是否正确  
  打开Device Supervisor App日志。脚本编写完成并点击“确定”后，通过日志中的`Build module: <主函数名称>, type: <publish/subscribe>`信息查看脚本是否构建成功。  
  
  脚本构建成功如下图所示：  

  ![](images/2020-05-18-17-20-05.png)  
  
  脚本构建失败如下图所示：  

  ![](images/2020-05-18-17-22-30.png)  

- 查看App的输出是否正确  
  使用`logger`和`logging`输出重要日志。下图是在脚本中的第6行使用了`logging.info`方法，在日志中可以通过搜索`<string> 6`查看输出结果是否符合预期。  
  
  ![](images/2020-05-18-17-25-03.png) 