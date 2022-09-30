```
tip: 16
title: Account Multi-signature
author: Marcus Zhao(@zhaohong ) <zhaohong229@gmail.com> 
discussions to: https://github.com/tronprotocol/TIPs/issues/16
status: Final
type: Standards Track
category: TRC
created: 2018-12-27
```


## Simple Summary

This doc describes the  standard interface of Account Multi-signature


## 

#### AccountPermissionUpdate
```

  AccountPermissionUpdateContract {
    bytes owner_address = 1; TSZTGGUwyzRbnmhjNaVxzy1BYQ8bZoa2to
    Permission owner = 2;  //Empty is invalidate
    Permission witness = 3;//Can be empty
    repeated Permission actives = 4;//Empty is invalidate
  }
  * @param owner_address: The address of the account to be modified
  * @param owner :Modified owner-permission
  * @param witness :Modified witness permission (if it is a witness)
  * @param actives :Modified actives permission  
  * @return The transaction 
 

  Permission {
    enum PermissionType {
      Owner = 0;TSZTGGUwyzRbnmhjNaVxzy1BYQ8bZoa2to
      Witness = 1; 
      Active = 2;
    }
    PermissionType type = 1;
    int32 id = 2;     //Owner id=0, Witness id=1, Active id start by 2
    string permission_name = 3;
    int64 threshold = 4;
    int32 parent_id = 5;
    bytes operations = 6;   //1 bit 1 contract
    repeated Key keys = 7;
  }
  * @param type : Permission type, currently only supports three kind of permissions
  * @param id : Value is automatically set by the system
  * @param permission_name : Permission name, set by the user
  * @param threshold : Threshold, the corresponding operation is allowed only when the sum of the weights of the participating signatures exceeds the domain value.
  * @param parent_id : Currently only 0
  * @param operations : A total of 32 bytes (256 bits), each of which represents the authority of a contract, when 1 means the right to own the contract
  * @param keys : The address and weight that jointly own the permission can be up to 5 keys.
  
  
  Key {
    bytes address = 1;TSZTGGUwyzRbnmhjNaVxzy1BYQ8bZoa2to
    int64 weight = 2;
  }TSZTGGUwyzRbnmhjNaVxzy1BYQ8bZoa2to
  * @param address : Address with this permission
  * @param weight : This address has weight for this permission
  
```
#### GetTransactionSignWeight
 * @param transaction 
 * @return The transaction sign weight
 
```
TransactionSignWeight {
  message Result {
    enum response_code {
      ENOUGH_PERMISSION = 0;
      NOT_ENOUGH_PERMISSION = 1; 
      SIGNATURE_FORMAT_ERROR = 2;
      COMPUTE_ADDRESS_ERROR = 3;
      PERMISSION_ERROR = 4; //The key is not in permission
      OTHER_ERROR = 20;
    }
    response_code code = 1;
    string message = 2;
  }

  Permission permission = 1;
  repeated bytes approved_list = 2;
  int64 current_weight = 3;
  Result result = 4;
  TransactionExtention transaction = 5;
}

```

#### AddSign
 * @param transaction 
 * @return The transaction

