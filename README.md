# RBX1 - ROS
Se cambia provisionalmente el nombre del topic "/cmd_vel" del nodo "arbotix" a "/turtle1/cmd_vel".

*El ejercicio consiste en que utilizando el comando del teleoperado(turtle_teleop_key), del paquete de "turtlesim", se controla utilizando las flechas del teclado el robot del paquete "rbx1"; aprovechando que tanto "turtlesim" como "rbx1" tienen el mismo tipo de mensaje en el comando de velocidad se puede utilizar el comando "remap" para cambiar el nombre del topic suscriptor del nodo "arbotix" de "rbx1".*

*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**Descargar Paquetes:**

***Paquete rbx1:***

1. *El paquete "rbx1" se debe descargar dentro de un workspace; por ejemplo: "/home/workspace/src"*

```
$ cd workspace/src

$ git clone https://github.com/pirobot/rbx1.git
```

2. *Utilizando el terminal se abre el archivo ".bashrc" y se agrega el workspace en caso de que no este agregado:*

```
$ cd

$ sudo gedit .bashrc 
```

- *Al final del archivo .bashrc se agrega la siguiente linea:*

```
source ~/workspace/devel/setup.bash
```

- *luego en una ventana de terminal estando en "/home" nos dirigimos al workspace y compilamos:*

```
$ cd workspace

$ catkin_make
```


***Paquete turtlesim:***

1. *Se instala el paquete con la siguiente instrución:*

```
$ sudo apt-get install ros-version_ros-turtlesim
```

*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**Remap**

1. *Se debe dirigir al archivo "fake_turtlebot.launch" ubicado en la dirección:  "/home/workspace/src/rbx1/rbx1_bringup/launch"*

2. *Posteriormente se agrega la instruccion: ```<remap from="/cmd_vel" to="/turtle1/cmd_vel"/>``` después de la línea 11:*

```
<launch>
  <param name="/use_sim_time" value="false" />

  <!-- Load the URDF/Xacro model of our robot -->
  <arg name="urdf_file" default="$(find xacro)/xacro.py '$(find rbx1_description)/urdf/turtlebot.urdf.xacro'" />
   
  <param name="robot_description" command="$(arg urdf_file)" />
    
  <node name="arbotix" pkg="arbotix_python" type="arbotix_driver" output="screen" clear_params="true">
      <rosparam file="$(find rbx1_bringup)/config/fake_turtlebot_arbotix.yaml" command="load" />
      <param name="sim" value="true"/>
      <remap from="/cmd_vel" to="/turtle1/cmd_vel"/>
  </node>
  
  <node name="robot_state_publisher" pkg="robot_state_publisher" type="state_publisher">
      <param name="publish_frequency" type="double" value="20.0" />
  </node>
  
</launch>
```
*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**Ejecución:**

1. *Primero se ha de abrir una ventana del términal e inicirar ros con el siguiente comando: - En una ventana de terminal se inica ros con el siguiente comando:*

```
$ roscore
```

2. *Posterior a esto en una ventana diferente del términal ejecutamos el archivo .launch de la siguiente forma:*

```
$ roslaunch rbx1_bringup fake_turtlebot.launch
```

3. *Luego de esto ha que ejecuter el rviz para visualizar el robot:*

```
$ rosrun rviz rviz -d`rospack find rbx1_nav`/sim.rviz
```

![alt text](Dirección de la imágen subida a github*)

4. *En caso de querer ajustar la velocidad de desplazamiento del robot se debe ejecutar la siguiente linea en una nueva ventana del términal:*

```
$ rosparam set teleop_turtle/scale_linear 0.5
```

5. *Como último paso para el funcionamiento del robot debemos ejecutar el comando teleoperado de turtlesim para así poder controlarlo con las flechas del teclado:*

```
$ rosrun turtlesim turtle_teleop_key
```

![alt text](Dirección de la imágen subida a github*)

6. *Tras llevar a cabo el paso a paso anterior podremos ver las coneiones entre nodos al ejecutar la siguente linea en otra ventana del términal:*

```
$ rosrun rqt_graph rqt_graph
```

![alt text](Dirección de la imágen subida a github*)

*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**Autores**

*Elian Andrés Diaz Vargas*

*Camilo Andrés Sosa Gutierrez*

*Daniel Alberto Cruz Porras*
