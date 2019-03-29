<a name="1300"></a>
command type:DynamicSceneAdded
## 1)Command 1300
   Command no
   1300- JSON format

   Required
   Command,CommandType,Payload,almondMAC

   SQL
   2.Insert on AlmondplusDB.SCENE
   params:AlmondMAC

   Functional
   1.Command 1300

   Flow
   socket(packet)->controller->processor->genericModel.execute->genericModel.add ->insertUpdate


<a name="1300b"></a>
command type:DynamicSceneActivated
## 1)Command 1300
   Command no
   1300- JSON format

   Required
   Command,CommandType,Payload,almondMAC

   SQL
   2.Insert on AlmondplusDB.SCENE
   params:AlmondMAC

   Functional
   1.Command 1300


   Flow
   socket(packet)->controller->processor->genericModel.execute->genericModel.Update-> genericModel.add


<a name="1300c"></a>
command type:DynamicSceneUpdated
## 1)Command 1300
   Command no
   1300- JSON format

   Required
   Command,CommandType,Payload,almondMAC

   SQL
   2.Insert on AlmondplusDB.SCENE
   params:AlmondMAC
   

   Functional
   1.Command 1300

   Flow
   socket(packet)->controller->processor->genericModel.execute->genericModel.update->genericModel.add

   <a name="1300d"></a>
command type:DynamicSceneRemoved
## 4)Command 1300
   Command no
   1300- JSON format

   Required
   Command,CommandType,Payload,almondMAC

   SQL
   2.Delete from SCENE
   params:AlmondMAC

   Functional
   1.Command 1300

   Flow
   socket(packet)->controller->processor->genericModel.execute->genericModel.remove
<a name="1300e"></a>

## 5)DynamicSceneRemoved (Command 1300)
   Command no
   1300- JSON format

   Required
   Command,CommandType,Payload,almondMAC

   SQl
   2.Delete on SCENE
     params:AlmondMAC

   Functional
   1.Command 1300

   Flow
   socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(remove)
