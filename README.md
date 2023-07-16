# Code

This Project Contains Code for Apex Specialist Super Badge Challenges Solution

# Topics
*Triggers*

*AggregateResult* - SOQL Query to find the minimum,Max,AVG,SUM of list of values in related record
AggregateResult[] results = [Select Maintenance_Request__c,
            MIN(Equipment__r.Maintenance_Cycle__c)cycle from Equipment_Maintenance_Item__c Where
            Maintenance_Request__c IN :validCaseIds Group by Maintenance_Request__c];
            
-->Step 1 : Convert into Map<String,Object> (Id,Object)

Map<Id,Decimal> CaseMaintenance = new Map<Id,Decimal>();
for(AggregateResult ar:results){
               CaseMaintenance.put((Id)ar.get('Maintenance_Request__c'),(Decimal)ar.get('cycle'));
            }

Deserialize the below Data
--------------------------
[
{"_id": "55d66226726b611100aaf741","replacement": false,"quantity": 5,"name": "Generator 1000 kW","maintenanceperiod": 365,"lifespan": 120,"cost": 5000,"sku": "100003"},
{"_id": "55d66226726b611100aaf742","replacement": true,"quantity": 183,"name": "Cooling Fan","maintenanceperiod": 0,"lifespan": 0,"cost": 300,"sku": "100004"},
{"_id": "55d66226726b611100aaf743","replacement": true,"quantity": 143,"name": "Fuse 20A","maintenanceperiod": 0,"lifespan": 0,"cost": 22,"sku": "100005"}
]

Step 1 : 
        // Deserialization to List<Object>
        List<Object> jsonlist = (List<Object>)JSON.deserializeUntyped(res.getBody()); 

Step 2:
for(Object eo:jsonlist){
		 // Convert to Map<String,Object>
            Map<String,object> mapobj = (Map<String,object>) eo;
    }

HttpCalloutMock to Test Callout
-------------------------------

While Testing Async Service - Include it with in
Test.startTest(), Test.stopTest()
--------------------------------
Ex: 
Test.startTest();
WarehouseCalloutService.makewarehousecallout(); // this is a Future method
Test.stopTest();

If you Test a Schedule class which Refer Async Service class.Then set mock for that class.
------------------------------------------------------------------------------------------
Ex:
Test.startTest();
Test.setMock(HttpCalloutMock.class, new WarehouseCalloutServiceMock());
ID jobId = System.schedule('Warehouse Schedule Job',Schedule, new WarehouseSyncSchedule());
Test.stopTest();

