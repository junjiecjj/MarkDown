# 问题

## Q1

DoaMeas.c、Struct.h文件中以下几个结构体是记录什么的?以下结构体通过RapidIO传输

```c
TAP_HJ_ABTION_BAND_DATA_MSG;　   ／／基带接口，基带发给BCN的结构体，调频、扩测、扩遥一样，都是这个结构体

TAP_HJ_GET_FM_FPGA_DATA_MSG;　　　／／调频接口　　DBF发给BCN
TAP_HJ_GET_SM_FPGA_DATA_MSG;      ／／单频点　　　DBF发给BCN
TAP_HJ_GET_SUMDIFF_FPGA_DATA_MSG;　 ／／和差　　DBF发给BCN
TAP_HJ_GET_SUMAMP_FPGA_DATA_MSG;　　／／多波束测角　　DBF发给BCN，做9波束跟踪
MEASURE_STRUCT;

```





+ TAP_HJ_ABTION_BAND_DATA_MSG;　   ／／基带接口
  + 调频
  + 扩测
  + 扩遥

+ 多波束  TAP_HJ_GET_SUMAMP_FPGA_DATA_MSG;　　／／多波束测角　　DBF发给BCN
  + 

+ TAP_HJ_GET_FM_FPGA_DATA_MSG;　　　／／调频接口　　DBF发给BCN
+ TAP_HJ_GET_SM_FPGA_DATA_MSG;      ／／单频点　　　DBF发给BCN
+ TAP_HJ_GET_SUMDIFF_FPGA_DATA_MSG;　 ／／和差　　DBF发给BCN



# 调试打印

[vReportHJDBFBandMes] send to DBF,g_WorkMode = 10,g_mode = 1, T0: azi = 0.000000,ele = 3.475500, T1: azi = 0.000000,ele = 3.475500, UV: xUDoaCos = 0, zUDoaCos = 1348236,------ 62707576

[vReportHJDBFBandMes] send to DBF,g_WorkMode = 10,g_mode = 1, T0: azi = 0.000000,ele = 5.217200, T1: azi = 0.000000,ele = 5.217200, UV: xUDoaCos = 0, zUDoaCos = 2022331,------ 62707626
[ScanMsg mode]  : point mode OK
[Disk ScanMsg mode]  : ScanMsg value scanmode = 0  ,timedelt = 0 ,plusNum = 0 ,scantimes = 0
[ScanMsg DISK mode]  : send ScanMsg mode result : scanmode = 0,Time = 62707668 OK





# 系统

## 组成









> 测控系统

Ka有源相控阵天线



伺服分系统



阵列信息处理



基带分系统



记录分系统



数据交互



测势标胶



> 　时统系统



> 测姿测位





> 卫通系统





> 通信网络







> 载车及方仓





> 综合监控





> 电源





## 状态

程引：预先设定的轨道，



数引：外部引导数据，地心地固下的xyz，BCN根据xyz外推



自跟踪：波束自动切换跟踪目标，



