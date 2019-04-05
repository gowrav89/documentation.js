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
## 7)DynamicDeviceAdded (Command 1200)
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
   
   
   
   a name="1500"></a>
## 8)DynamicClientList(Command 1500)
   Command no
   1500- JSON format

   Required
   Command,CommandType,Payload,almondMAC

   SQL
   2.insert into  AlmondplusDB.WIFICLIENTS              /*if (!doBackUp)*/
     Params:AlmondMAC

   3.Delete from WIFICLIENTS
     Params:AlmondMAC

   4.insert into  AlmondplusDB.WIFICLIENTS
     Params:AlmondMAC


   Functional
   1.Command 1500

   Flow
   consumer(processMessage)->controller(processor)->preprocessor(doNothing)->genericModel(execute),genericModel(addAll),insertBackUpAndUpdate,removeAndInsert.

 a name="1500"></a>
## 9)DynamicClientUpdated (Command 1500)
   Command no
   1500- JSON format

   Required
   Command,CommandType,Payload,almondMAC

   SQL
   2.insert into  AlmondplusDB.WIFICLIENTS
     Params:AlmondMAC

   Functional
   1.Command 1500

   Flow
   consumer(processMessage)->controller(processor)->preprocessor(doNothing)->genericModel(execute),genericModel(add).
   
 
<a name="1500a"></a>
## 10)DynamicClientAdded (Command 1500)
   Command no
   1500- JSON format

   Required
   Command,CommandType,Payload,almondMAC

   SQl
   2.Insert on AlmondplusDB.WIFICLIENTS
     params: AlmondMAC
   3.Select on NotificationID
     params: UserID
   15.Update on AlmondplusDB.NotificationID
      params:RegID

   /*if (oldRegid && oldRegid.length > 0) */
   16.Delete on AlmondplusDB.NotificationID
      params: RegID

   REDIS
   4.hmget on AL_<AlmondMAC>                   // params: ["name"]
   5.LPUSH on AlmondMAC_Client                 // params: redisData

   /* if (res > count + 1) */
   6.LTRIM on AlmondMAC_Client                //here count = 9, res = Result from step 5
             
              (or)

   /* if (res == 1) */
   6.expire on AlmondMAC_Client             //here, res = Result from step 5
   
   7.LPUSH on AlmondMAC_All                // params: redisData
   /* if (res > count + 1) */
   8.LTRIM on AlmondMAC_All                //here count = 19, res = Result from step 7
             
              (or)

   /* if (res == 1) */
   8.expire on AlmondMAC_All             //here, res = Result from above step 7

   Postgres
   9.Insert on recentactivity
     params: mac, id, time, index_id, client_id, name, type, value

   Cassandra
   12.Insert on notification_store.notification_records
      params: usr_id, noti_time, i_time, msg
   13.Update on notification_store.badger
      params: usr_id
   14.Select on notification_store.badger
      params: usr_id
   
   Functional
   1.Command 1500
   10.delete ans.AlmondMAC;
     delete ans.CommandType;
     delete ans.Action;
     delete ans.HashNow;
     delete ans.Devices;
     delete ans.epoch;
   11.delete input.users;

   Flow
   socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(add)->receive(mainFunction)->notify(sendAlwaysClient)->generator(wifiNotificationGenerator)->cassandra(qtoCassHistory)->cassandra(qtoCassConverter)->msgService(notificationHandler)->msgService(handleResponse).
   
   
   
   <a name="1500"></a>
## 11)DynamicAllClientsRemoved(Command 1500)
   Command no
   1500- JSON format

   Required
   Command,CommandType,Payload,almondMAC

   Cassandra
   2.INSERT INTO  notification_store.almondhistory
     Params:mac,type,data,time

   8.INSERT INTO notification_store.notification_records
     Params:"usr_id", "noti_time" , "i_time" , "msg"
   
   9.UPDATE notification_store.badger           // SET b_cnt = b_cnt + 1
     Params:usr_id  

   10.select from notification_store.badger     // b_cnt as count,usr_id as id
     Params:usr_id  


   SQL
   3.Delete from WIFICLIENTS
     Params:AlmondMAC  
   4.select from NotificationID
     Params:UserID 
   11.UPDATE on AlmondplusDB.NotificationID
     Params:RegID
   12.DELETE FROM AlmondplusDB.NotificationID
     Params:RegID   

   Redis
   5.hmget on AL_<almondMAC>                            //     Params: ["name"]  
                                   

   Functional
   1.Command 1500

   6.delete ans.AlmondMAC;
     delete ans.CommandType;
     delete ans.Action;
     delete ans.HashNow;
     delete ans.Devices;
     delete ans.epoch;

   7.delete input.users  

   Flow
   socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(removeAll)->receive(mainFunction)->notify(alwaystrue)->sqlPool(getID)->generator(wifiNotificationGenerator),getAlmondName->sendNotification->cassandra(qtoCassConverter),getNotificationData,insertDynamicUpdate,loggerPoint1(execute),cassandra(getQuery),insertDynamicUpdate->msgService(notificationHandler)->msgService(qToGCMConverter),sql(updateRegID)->sql.deleteID.
   
   
<a name="1200"></a>
## 12)DynamicDeviceList(Command 1200)
   Command no
   1200- JSON format

    Required
   Command,CommandType,Payload,almondMAC

    Redis
   2.hgetall on MAC:<AlmondMAC> 
     
    multi
   4.hmset on AL_<AlmondMAC>                    // params:["deviceRestore", "restore"]

    multi
   5.hgetall on MAC:<AlmondMAC>:deviceIds[id]

     multi 
   9.del on MAC:<almondMAC>

     multi
   10.del on MAC:<payload AlmondMAC>:<removeIds>

   /*if(Object.keys(variables).length==0)*/
     multi.
   11.hmset on MAC:<AlmondMAC>,key,variables

                (or)

   /* if (deviceArray.length>0) */
   11.multi.hmset on MAC:<AlmondMAC>, deviceArray

    SQL
   3.SELECT from DEVICE_DATA
     Params:AlmondMAC
   7.insert into  AlmondplusDB.DEVICE_DATA
     Params:AlmondMAC

   8.Insert on AlmondplusDB.DEVICE_DATA
     params: AlmondMAC 

   12.select  from SCSIDB.CMSAffiliations CA,AlmondplusDB.AlmondUsers AU,SCSIDB.CMS CMS
   Params:CA.CMSCode, AU.AlmondMAC

    Cassandra
   6.INSERT INTO  notification_store.almondhistory
   Params:mac,type,data,time 

   Functional
   1.Command 1200

    Flow
   consumer(processMessage)->controller(processor)->preprocessor(dymamicAddAllDevice)->device(addAll)->redisDeviceValue(getDeviceList)->genericModel(insertBackUpAndUpdate)->device(getDevices)->genericModel(get)->redisDeviceValue(getFormatted)->genericModel(removeAndInsert)->redisDeviceValue(addAll)->receive(mainFunction)->scsi(sendFinal)

<a name="1200"></a>
## 13)DynamicIndexUpdated(Command 1200)
   Command no
   1200- JSON format

    Required
   Command,CommandType,Payload,almondMAC

    Redis
    /*if(data.index == 0 && packet.cmsCode)*/
    2.hgetall on MAC:<almondMAC>,data.deviceId

    /*if(Object.keys(variables).length==0)*/
     multi.
    3.hmset on MAC:<AlmondMAC>,key,variables

                 (or)

   /* if (deviceArray.length>0) */
   multi
   3.hmset on MAC:<AlmondMAC>, deviceArray

   7.LPUSH on AlmondMAC_Device  
      // params: redisData  
   
    /* if (res > count + 1) */
   8.LTRIM on AlmondMAC_Device                
             
              (or)

   /* if (res == 1) */
   8.expire on AlmondMAC_Device  

   9.LPUSH on AlmondMAC_All

   /* if (res > count + 1) */
   10.LTRIM on AlmondMAC_All
             
              (or)

   /* if (res == 1) */
   10.expire on AlmondMAC_All  
                                   

    SQL
   4.SELECT AlmondplusDB.NotificationPreferences
     Params:AlmondMAC, DeviceID, UserID IN
   5.select from NotificationID
     Params:UserID
   6.select from DeviceData
     Params:AlmondMAC, DeviceID 

   17.Update on AlmondplusDB.NotificationID
      params:RegID

   /*if (oldRegid && oldRegid.length > 0) */
   18.Delete on AlmondplusDB.NotificationID
      params: RegID
      
   19.select from SCSIDB.CMSAffiliations CA,AlmondplusDB.AlmondUsers AU,SCSIDB.CMS CMS
      Params:CA.CMSCode, AU.AlmondMAC   

    postgres
   11.Insert on recentactivity
     params: mac, id, time, index_id,index_name, name, type, value

    Functional
   1.Command 1200

  12.delete ans.AlmondMAC;
     delete ans.CommandType;
     delete ans.Action;
     delete ans.HashNow;
     delete ans.Devices;
     delete ans.epoch;

   13delete input.users  

     cassandra
   14.INSERT INTO notification_store.notification_records
     params:"usr_id", "noti_time" , "i_time" , "msg"

   15.Update on notification_store.badger
      params: usr_id
   16.Select on notification_store.badger
      params: usr_id

    Flow
   socket(packet)->controller(processor)->preprocessor(dynamicIndexUpdated)->device(updateIndex)->DeviceStore(update)->updateDeviceIndex->rec(mainFunction)->container(dynamicIndexUpdate)->sql(preferenceCheck)->sql(getID)->container(generator.deviceIndexUpdat)->sql(queryDeviceData)->cassandra(qtoCassHistory),addToHttpRedis,pushToRedis->cassandra(qtoCassConverter)->getNotificationData->insertNotification->insertDynamicUpdate->cassandra(getQuery)->scsi(sendFinal).
   
    <a name="1500"></a>
## 14)DynamicClientRemoved(Command 1500)
   Command no
   1500- JSON format

    Required
   Command,CommandType,Payload,almondMAC

   SQL
   2.select from WifiClients
     Params:AlmondMAC, ClientID

   3.Delete from WIFICLIENTS
     Params:AlmondMAC

   4.select from NotificationID 
     Params:UserID  

  16.UPDATE on AlmondplusDB.NotificationID
     Params:RegID 

  /*if (oldRegid && oldRegid.length > 0)*/
  17.DELETE FROM AlmondplusDB.NotificationID
     Params:RegID IN 

    Redis
   5.hmget on AL_<almondMAC>
     // params:"name"

   6.LPUSH on AlmondMAC_Clients  
      // params: redisData 

    /* if (res > count + 1) */
   7.LTRIM on AlmondMAC_Clients               
             
              (or)

   /* if (res == 1) */
   7.expire on AlmondMAC_Clients    

   8.LPUSH on AlmondMAC_All

   /* if (res > count + 1) */
   9.LTRIM on AlmondMAC_All
             
              (or)

   /* if (res == 1) */
   9.expire on AlmondMAC_All   

   Postgres

  10.INSERT INTO recentactivity
     Params:mac, id, time, index_id, client_id, name, type, value  

    Cassandra
  13.INSERT INTO notification_store.notification_records 
     Params:"usr_id", "noti_time" , "i_time" , "msg" 
  
  14.UPDATE notification_store.badger
     Params:usr_id

  15.select from  notification_store.badger
     Params:usr_id   


    Functional
   1.Command 1200

  11.delete ans.AlmondMAC;
     delete ans.CommandType;
     delete ans.Action;
     delete ans.HashNow;
     delete ans.Devices;
     delete ans.epoch;

  12.delete input.users  
 

    Flow
   socket(packet)->controller(processor)->preprocessor(dynamicClientRemoved)2.CP(query)->genericModel(execute)3.genericModel(remove)->container(sendAlwaysClient)->4.sql.getID-> generate(wifiNotificationGenerator)->5.getAlmondName->cassandra(qtoCassHistory)->getInsertData->addToHttpRedis->6,7.pushToRedis->8,9.pushToRedis->10.postgres(query)->cassandra(qtoCassConverter)->11.getNotificationData->12.insertNotification->13.insertDynamicUpdate-> 14.insertDynamicUpdate->15insertDynamicUpdate->msgService(notificationHandler)->msgService(qToGCMConverter)->msgService(handleResponse)->16.sql(updateRegID)->17.sql(deleteID).
   
