# MobiusPi API手册

  - [概述](#概述)
  - [安装](#安装)
  - [Python要求](#python要求)
  - [1. cellular](#cellularapi)
    - [Getting Started](#getting-started)
    - [Cellular](#cellular)
    - [Option functions](#option-functions)
  - [2. serial](#serialapi)
    - [Getting Started](#getting-started2)
    - [Serial](#serial)
    - [Option functions](#option-functions2)

## 概述
本文档描述了`mobiuspi_lib`库的源代码，该库实现了获取MobiusPi的运行状态和调用MobiusPi物理接口等功能。

## 安装
映翰通提供包含`mobiuspi_lib`库的软件开发工具包（SDK），如需获取MobiusPi的SDK及其功能特性信息，请联系客服。安装和升级SDK请参考[IG902更新软件版本](http://fw.ig.inhand.com.cn/zh_CN/latest/IG902%E5%BF%AB%E9%80%9F%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C.html#id1)。  

## Python要求
MobiusPi Python SDK适用于Python 3.7和3.8，如果您使用其他版本的Python，则可能导致代码运行异常。
您可以打开命令提示符或启动python IDE，使用`python`命令确认您的Python版本。本文档假设您使用了Python 3.7和3.8。

![](images/2020-05-26-17-14-56.png)  

<a id="cellularapi"> </a>  

## 1. cellular
### Getting Started
Here is a very simple example that shows how to get cellular info of device:  
``` python
# import Cellular
from mobiuspi_lib.cellular import Cellular

# build Cellular instance
cellular = Cellular()

# get modem info
modem = cellular.get_modem()
print("get modem: %s" % modem)

```

### Cellular
You can use the Cellular class as an instance, within a class or by subclass. The general usage flow is as follows:

* Create a Cellular instance
* Use  `get_modem()` to get model info
* Use  `get_network()` to get network info

### Option functions
#### get_modem()
##### Request parameter
None  

##### Description
You can use this function to get system info.  

##### Returns
- Return type  

  dict  

- Return value  

  ```python
  {
      "active_sim": "SIM 1",
      "imei_code": "811622048741556",
      "imsi_code": "411220441893359",
      "iccid_code": "84463317227780999882",
      "phone_number": "+8611162203133",
      "signal_level": 0,
      "dbm": 113,
      "rerp": 0,
      "rerq": 0,
      "register_status": 0,
      "operator": "CHN-CT",
      "apns": "",
      "network_type": "4G",
      "lac": "BB00",
      "cell_id": "DD788B81"
  }
  ```  

- Return structure
  - active_sim(string)：当前 SIM 卡
  - imei_code(string)：IMEI 号码
  - imsi_code(string)：IMSI 号码
  - iccid_code(string)：ICCID 号码
  - phone_number(string)：电话号码
  - signal_level(int)：信号值
  - dbm(int)：dBm值
  - rerp(int)：RSRP，预留参数
  - rerq(int)：RSRQ，预留参数
  - register_status(int)：注册状态
    - 0：正在注册到网络
    - 1：注册网络成功
    - 5：注册网络成功，漫游状态
    - 6：尚未注册到网络
    - 7：未注册
  - operator(string)：运营商
  - apns(string)：APN，预留参数
  - network_type(string)：网络类型
  - lac(string)：位置区码
  - cell_id(string)：小区 ID

#### get_network()
##### Request parameter
None  

##### Description
You can use this function to get network info.  

##### Returns
- Return type  

  list  

- Return value  

  ```python
  [
  {
      'status': 0,
      'ip_addr': '0.0.0.0',
      'netmask': '0.0.0.0',
      'gateway': '0.0.0.0',
      'dns': '0.0.0.0',
      'mtu': 1200,
      'connect_time': 0
  }]
  ```

- Return structure
  - status(int)：网络状态
    - 0：未连接
    - 1：已连接
  - ip_addr(string)：IP 地址
  - netmask(string)：子网源码
  - gateway(string)：网关
  - dns(string)：DNS
  - mtu(int)：MTU
  - connect_time(int)：连接时间，单位为秒

#### get_signal_level()
##### Request parameter
None  

##### Description
You can use this function to get signal level info.  

##### Returns
- Return type  

  int  

- Return value  

  ```python
  28
  ```

#### get_dbm()
##### Request parameter
None  

##### Description
You can use this function to get dbm info.  

##### Returns
- Return type  

  int  

- Return value

  ```python
  57
  ```

#### get_active_sim()
##### Request parameter
None  

##### Description
You can use this function to get active sim info.  

##### Returns
- Return type  

  str  

- Return value  

  ```python
  SIM 1
  ```

#### get_imei_code()
##### Request parameter
None  

##### Description
You can use this function to get imei info.  

##### Returns
- Return type  

  str  

- Return value  

  ```python
  811622048741556
  ```

#### get_imsi_code()
##### Request parameter
None  

##### Description
You can use this function to get imsi info.  

##### Returns
- Return type  

  str  

- Return value  

  ```python
  411220441893359
  ```

#### get_iccid_code()
##### Request parameter
None  

##### Description
You can use this function to get iccid info.  

##### Returns
- Return type  

  str  

- Return value 

  ```python
  84463317227780999882
  ```

#### get_phone_number()
##### Request parameter
None  

##### Description
You can use this function to get phone number info.  

##### Returns
- Return type  

  str  

- Return value 

  ```python
  +8611162203133
  ```

#### get_rerp()
##### Request parameter
None  

##### Description
You can use this function to get rerp info.  

##### Returns
- Return type  

  int  

- Return value 

  ```python
  0
  ```

#### get_rerq()
##### Request parameter
None  

##### Description
You can use this function to get rerq info.  

##### Returns
- Return type  

  int  

- Return value 

  ```python
  0
  ```
        
#### get_register_status()
##### Request parameter
None  

##### Description
You can use this function to get register status info.  

##### Returns
- Return type  

  int  

- Return value 

  ```python
  0
  ```

- Return structure
  - 0：正在注册到网络
  - 1：注册网络成功
  - 5：注册网络成功，漫游状态
  - 6：尚未注册到网络
  - 7：未注册

#### get_operator()
##### Request parameter
None  

##### Description
You can use this function to get operator info.  

##### Returns
- Return type  

  str  

- Return value 

  ```python
  CHN-CT
  ```

#### get_apns()
##### Request parameter
None  

##### Description
You can use this function to get apns info
##### Returns
- Return type  

  str  

- Return value  

  ```python
  
  ```
        

#### get_network_type()
##### Request parameter
None  

##### Description
You can use this function to get network type info.  

##### Returns
- Return type  

  str  

- Return value  
  
  ```python
  4G
  ```

#### get_lac()
##### Request parameter
None  

##### Description
You can use this function to get lac info.  

##### Returns
- Return type  

  str  

- Return value  

  ```python
  BB00
  ```

#### get_cell_id()
##### Request parameter
None  

##### Description
You can use this function to get cell id info.  

##### Returns
- Return type  

  str  

- Return value  

  ```python
  DD788B81
  ```

<a id="serialapi"> </a>  

## 2. serial

<a id="getting-started2"> </a>  

### Getting Started
Here is a very simple example that shows how to get the path of the 232/285 serial:  
``` python
from mobiuspi_lib.serial import Serial

# you can use serial as an instance of Serial class
serial = Serial()

# get 232 path
path_232 = serial.get_serial232_path()
print("232 path: %s" % path_232 )

# get 485 path
path_485 = serial.get_serial485_path()
print("485 path: %s" % path_485

```

### Serial
You can use the Serial class as an instance, within a class or by subclassing. The general usage flow is as follows:

* Create a Serial instance
* Use  `get_serial232_path()` to get 232 path
* Use  `get_serial485_path()` to get 484 path

<a id="option-functions2"> </a>  

### Option functions
#### get_serial232_path()
##### Request parameter
None  

##### Description
You can use this function to get 232 path.  

##### Returns
- Return type  

  str  

- Return value  

  ```python
  /dev/ttyO1
  ```

- Return structure
  - /dev/ttyO5：IG501时返回此数值
  - /dev/ttyO1：IG902时返回此数值

#### get_serial485_path()
##### Request parameter
None  

##### Description
You can use this function to get 485 path.  

##### Returns
- Return type  

  str  

- Return value  

  ```python
  /dev/ttyO3
  ```
- Return structure
  - /dev/ttyO1：IG501时返回此数值
  - /dev/ttyO3：IG902时返回此数值