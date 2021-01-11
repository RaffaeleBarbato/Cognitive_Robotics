# Progetto Pepper Sociale
## Gruppo 5 
### Barbato Raffaele 0622701280 
### Bianco Giovanni 0622701176 
### Carpinelli Camilla 0622701172 
### Finelli Natalia 0622701263
Per completare il task assegnato sono stati implementati i seguenti nodi:

  - image_taken
  - head
  - detector_node
  - visualization_node
  - speech_node
 
Per eseguire il task su Pepper è necessario seguire i seguenti passi:
```sh
$ cd gruppo5/gruppo5_ws
$ catkin build
$ source devel/setup.bash
$ roslaunch pepper_bringup pepper_full_py.launch nao_ip:=10.0.1.230
```

```sh
$ source devel/setup.bash
$ rosrun nodes detector_node
```

```sh
$ source devel/setup.bash
$ rosrun nodes visualization_node
```

```sh
$ source devel/setup.bash
$ rosrun nodes image_taken
```

```sh
$ source devel/setup.bash
$ rosrun nodes speech_node --pip 10.0.1.230
```

```sh
$ source devel/setup.bash
$ rosrun nodes head
```

