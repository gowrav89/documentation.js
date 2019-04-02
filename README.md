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
								// if (data[key])

   SQL
   3.SELECT on ALMONDPROPERTIES
   params:AlmondMAC

   4.UPDATE on AlmondProperties2
   params:AlmondMAC

   5.select from NotificationID 
   params:UserID 

   18.Update on AlmondplusDB.NotificationID
   params:RegID

   19.Delete on AlmondplusDB.NotificationID               //if (oldRegid && oldRegid.length > 0)
   params: RegI

   20.select from SCSIDB.CMSAffiliations CA,             // if (!MACS[data.almondMAC])
                  lmondplusDB.AlmondUsers AU,
                  SCSIDB.CMS CMS   
   params: CA.CMSCode,AU.AlmondMAC                            

   Redis
   2.hmset on AL_<data.AlmondMAC>                          // params: [redisKey[key], data[key]]

   6.hmget on AL_<almondMAC>                               // params: ["name"]


   7.LPUSH on AlmondMAC_device                             // params: redisData

   8.LTRIM on AlmondMAC_device                             // if (res > count + 1)

            (or)

   8.expire on AlmondMAC_device                             // if (res == 1)

   10. LPUSH on AlmondMAC_All                               //params: redisData

   11.LTRIM on AlmondMAC_All                               // if (res > count + 1)
                 
		 (or)
		 
   11.expire AlmondMAC_All                                // if (res == 1)

   postgres
   12.INSERT INTO recentactivity
      params:mac, id, time, index_id,index_name, name, type, value

   
   cassandra
   15.INSERT INTO notification_store.notification_records
      params:usr_id,noti_time,i_time,msg  

   16.select from notification_store.badger
      params:usr_id

   17.UPDATE notification_store.badger  
      params:usr_id


   Functional
   1.Command 1050

   

   13.delete ans.AlmondMAC;
      delete ans.CommandType;
      delete ans.Action;
      delete ans.HashNow;
      delete ans.Devices;
      delete ans.epoch;
      
   14.delete input.users

   Flow
    socket(packet)->controller(processor)->preprocessor(doNothing)->DynamicAlmondProperties(redisUpdate)->genericModel(get)->notification(mainFunction)->container(almondProperties),container(propertiesNotification),getAlmondName->cassandra(qtoCassHistory),addToHttpRedis(pushToRedis)->sendNotification,cassandra.qtoCassConverter,getNotificationData->scsi(sendFinal),CMS(sendFinal)
   
    <a name="1200"></a>
## 3)DynamicDeviceAdded (Command 1200)
   Command no
   1200- JSON format

   Required
   Command,CommandType,Payload,almondMAC

   Redis
   /*if(Object.keys(variables).length==0) */
   2.multi.hmset on MAC:%s, AlmondMAC key variable

   /* if (deviceArray.length>0) */
   3.multi.hmset on MAC:%s, AlmondMAC

   SQL
   4.insert into  AlmondplusDB.DEVICE_DATA 
   Params:AlmondMAC

   5.select from SCSIDB.CMSAffiliations CA,AlmondplusDB.AlmondUsers AU,SCSIDB.CMS CMS 
   Params:CA.CMSCode, AU.AlmondMAC 

   Functional
   1.Command 1200

   Flow
   socket(packet)->controller(processor)->preprocessor(dymamicAddAllDevice)->model(device.execute),redisDeviceValue,genericModel-> scsi(sendFinal),CMS(sendFinal),updateMACS.

