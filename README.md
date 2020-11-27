# Progetto Pepper Sociale
### Gruppo 5 
### Barbato Raffaele 0622701280 Bianco Giovanni 0622701176 Carpinelli Camilla 062270172 Finelli Natalia 0622701263
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
## Interazione tra nodi e scelte progettuali
La comunicazione tra i nodi avviene attraverso i Topics seguendo il paradigma Publish/Subscribe.
Il nodo *head* presenta due publisher che pubblicano sui topic *Position* e *JointAngles* rispettivamente la posizione della testa di Pepper e gli angoli dei giunti della testa (Pitch e Yaw). Il topic *Position* viene sottoscritto all'interno del nodo *Image_taken* richiamando la callback *take_image()* in cui viene scattata la foto attraverso l'utilizzo della API ALVideoDevice di NaoQIVision. 
Successivamente, all'interno del nodo *Image_taken* vi è un publisher che pubblica sul topic *Image* l'immagine appena elaborata. Questo topic viene sottoscritto all'interno del nodo *detector_node* e nella callback *rcv_image* viene effettuata la detection dell'immagine utilizzando il seguente detector: *faster_rcnn_resnet50_v1_640x640_coco17_tpu-8*.
Questo detector è stato scelto perchè, dopo aver effettuato vari test utilizzando altri detector, è risultato essere il migliore in termini di velocità e accuratezza.
A questo punto, viene effettuata la detection dell'immagine. Una volta terminata, la detection viene pubblicata sul topic *detection*, sottoscritto nel nodo *visualization_node*, al cui subscriber corrisponde la callback *rcv_detection* in cui viene salvato l'insieme degli oggetti riconosciuti all'interno di un messaggio di tipo *detector_msg* (creato dal gruppo e contenente l'insieme degli oggetti e la rispettiva posizione). Successivamente, viene pubblicato il *detector_msg*, che a sua volta viene sottoscritto all'interno del nodo *speech_node*, il cui subscriber richiama *callback* che fa parlare Pepper e l'immagine viene mostrata a schermo. 
Una volta che Pepper ha terminato di parlare, in *speech_node* viene pubblicato un messaggio di tipo bool che verrà sottoscritto dal nodo *head* e renderà possibile il successivo movimento. 
Quindi, la scelta progettuale effettuata è stata quella di eseguire tutte le operazioni (cattura dell'immagine, detection, speech) alla fine di ogni movimento della testa (destra,centro,sinistra).
