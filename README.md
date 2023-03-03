

## 运行配置文件
conda env create -f py36.yaml

## How to train
You may start with training in Stage1 and when it is well-trained you can transfer to Stage2 base on the policy model of Stage1, this is exactly what Curriculum Learning means. Training Stage2 from scratch may converge at a lower performance or not even converge.
Please note that the motivation of training in Stage2 is to generalize the model, which hopefully can work well in real environment.

Please use the `stage_ros-add_pose_and_crash` package instead of the default package provided by ROS.

## 配置ROS空间
```
mkdir -p catkin_ws/src
cp stage_ros-add_pose_and_crash catkin_ws/src
cd catkin_ws
catkin_make
source devel/setup.bash
```

To train Stage1, modify the hyper-parameters in `ppo_stage1.py` as you like, and running the following command:

## 启动ROS
roscore


## 启动环境-环境1
cd catkin_ws
(-g 表示是否显示界面)
rosrun stage_ros_add_pose_and_crash stageros -g ../worlds/stage1.world
## 运行RL算法
conda activate env(你的conda环境名)
mpiexec -np 5 python ppo_stage1.py


##　环境２
To train Stage2, modify the hyper-parameters in `ppo_stage2.py` as you like, and running the following command:
```
rosrun stage_ros_add_pose_and_crash stageros -g ../worlds/stage2.world
mpiexec -np 5 python ppo_stage2.py
```

## How to test(测试代码我还没调)
```
rosrun stage_ros_add_pose_and_crash stageros worlds/circle.world
mpiexec -np 50 python circle_test.py
```

## Notice
I am not the author of the paper and not in their group either. You may contact [Jia Pan](https://sites.google.com/site/panjia/) (jpan@cs.hku.hk) for the paper related issues. 
If you find it useful and use it in your project, please consider citing:
```
@misc{Tianyu2018,
	author = {Tianyu Liu},
	title = {Robot Collision Avoidance via Deep Reinforcement Learning},
	year = {2018},
	publisher = {GitHub},
	journal = {GitHub repository},
	howpublished = {\url{https://github.com/Acmece/rl-collision-avoidance.git}},
	commit = {7bc682403cb9a327377481be1f110debc16babbd}
}
```
