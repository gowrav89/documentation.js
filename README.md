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
## 2)Command 1300
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
## 3)Command 1300
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
   
   
   <a name="1050"></a>
## 6)AlmondProperties (Command 1050)
   Command no
   1050- JSON format

   Required
   Command,CommandType,Payload,almondMAC

   Redis
   2.hmset on AL_<data.AlmondMAC>								// if (data[key])

   SQL
   3.SELECT on ALMONDPROPERTIES
   params:AlmondMAC

   4.UPDATE on AlmondProperties2
   params:AlmondMAC

   5.select from NotificationID 
   params:UserID 

   14.select from SCSIDB.CMSAffiliations CA,                 // if (!MACS[data.almondMAC])
                  lmondplusDB.AlmondUsers AU,
                  SCSIDB.CMS CMS   
     params:CA.CMSCode=CMS.CMSCode and AU.AlmondMAC = CA.AlmondMAC                                

   Redis
   6.hmget on AL_<almondMAC>

   7.LPUSH on AlmondMAC_Client

   8.LTRIM on AlmondMAC_Client                              // if (res > count + 1)

   9.expire AlmondMAC_Client                                // if (res == 1)

   10. LPUSH on AlmondMAC_All

   11.LTRIM on AlmondMAC_All                                 // if (res > count + 1)

   12.expire AlmondMAC_All                                  // if (res == 1)

     Functional
   1.Command 1050

   13.delete ans.AlmondMAC;
      delete ans.CommandType;
      delete ans.Action;
      delete ans.HashNow;
      delete ans.Devices;
      delete ans.epoch;
      return ans

   Flow
   socket(packet)->controller(processor)->preprocessor(doNothing)->DynamicAlmondProperties(redisUpdate)->genericModel(get)->notification(mainFunction)->container(almondProperties),container(propertiesNotification),getAlmondName->cassandra(qtoCassHistory),addToHttpRedis(pushToRedis)->sendNotification,cassandra.qtoCassConverter,getNotificationData->scsi(sendFinal),CMS(sendFinal)
   
   
