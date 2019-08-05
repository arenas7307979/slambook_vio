有两个包,一个是ros, 一个非ros

安裝要先build code_util（此時src內不能有imu_util), code_utili build以後
才能將imu_utili抓進來src 一起 build

針對模擬有兩個環境：：vio_data_simulation-master / vio_data_simulation-ros_version
兩個專案不能同時在src中build
需要分開執行, 目標不一樣 一個仿imu靜止（vio_data_simulation-ros_version為了校正）   一個仿動態（vio_data_simulation-master為了積分）


对非 ros :（仿imu動態情況下的狀態）
vio_data_simulation-master ==>無法用catkin build  要用以下方式
1. 编译 
2. 执行bin/data_gen, 生成数据 
3. 执行python_tools/draw_trajectory.py 画出轨迹
4. 换成中值积分, 再重做一遍上述1,2,3过程


ROS包是針對靜態imu狀態去倣真：
相當倣真 並且 讀取imu靜止2小時 或 5小時的時間
对ROS: 使用 imu_utils（仿imu靜止狀態）
1. ros下编译 
2. 执行, 生成 imu.bag 
3. rosbag play -r 500 imgimu_utils.bag 回放（五個小時？）
4. 用imu_utils进行接收和分析
5. 用imu_utils下的scripts/下的matlab 脚本画allan曲线

对ROS: 使用 kalibr_allan
1. ros下编译 
2. 执行, 生成 imu.bag 
3. 用kalibr_allan的bagconver把bag转成 mat文件, 见readme
4. 用kalibr_allan的m脚本对mat文件进行分析, (需修改m文件中的mat文件路径)
5. 用kalibr_allan的m脚本画allan曲线, (需修改m文件中的result文件路径)

m脚本运行需要matlab, 安装耗时,  最好提前做. 最好是2018版本
