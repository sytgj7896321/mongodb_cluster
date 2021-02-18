Install mongodb cluster of any size(include shard config mongos) in one role

Just for Centos/Redhat

Create var files in group_vars like

---
Server_Roles:
  Mongodb:
    Version: '4.4.3'
    Install_Dir: '/usr/local/mongodb'
    Data_Dir: '/data/mongodb'
    Username: 'mongod'
    Groupname: 'mongod'
    Comment: 'MongoDB Database Server'
    Home_Dir: '/var/lib/mongo'
    Shell_Type: '/bin/false'
    
---
Mongodb_Roles:
  Type: config
  Binary: mongod
  Port: 27018
  ReplSetName: config
  
---
Mongodb_Roles:
  Type: mongos
  Binary: mongos
  Port: 27017
  Config_Port: 27018
  Config_Group_Name: Sheca_MongoDB_Config_01
  ReplSetName: config
  Shard_List:
  - { shard_group_name: 'Sheca_MongoDB_Shard_01_A1',shard_replicaset_name: 'shard01a1',shard_port: '27019' }
  - { shard_group_name: 'Sheca_MongoDB_Shard_01_A2',shard_replicaset_name: 'shard01a2',shard_port: '27020' }
  - { shard_group_name: 'Sheca_MongoDB_Shard_01_A3',shard_replicaset_name: 'shard01a3',shard_port: '27021' }

---
Mongodb_Roles:
  Type: shard
  Binary: mongod
  Port: 27019
  ReplSetName: shard01a1

---
Mongodb_Roles:
  Type: shard
  Binary: mongod
  Port: 27020
  ReplSetName: shard01a2
 
 ---
Mongodb_Roles:
  Type: shard
  Binary: mongod
  Port: 27021
  ReplSetName: shard01a3
